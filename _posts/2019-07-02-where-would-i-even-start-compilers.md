---
layout: post
post_author: Jon Egeland
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'Where Would I Even Start?: Compilers'
publish_date: 2017-08-17 18:24:00 +0000
feature_post_image: "/uploads/compilers.gif"
post_images: []
slug: where-would-i-even-start-compilers

---
Compilers are scary...at least from the outside. On the inside, though, the basics of a compiler are a lot simpler than you may think. A compiler is really just [a program to convert between languages](https://en.wikipedia.org/wiki/Compiler). For example, the [Babel compiler](https://babeljs.io/) converts newer versions of JavaScript to older versions that are more compatible with today’s browsers. The things that make larger compilers so menacing are complicated, ambiguous language designs, large standard libraries, and performance optimizations, just to name a few.

In this post, I want to go through the basics of how a compiler works by writing an interpreter for some algebraic expressions. Along the way, I'll provide references to more detailed material and examples if you want to go more in depth on anything in particular. The final code from this post will all be [available on GitHub](https://github.com/faultyserver/compilers-intro) for you to fork. The entire codebase is just over 300 lines.

I'm going to be skipping over a lot of theory in this post, largely because I don't think it's necessary in forming a basic understanding of compilers. That said, I would still recommend reading up on the references throughout this post if you're interested in learning more about compilers in general. The goal here is to provide a quick introduction to give you a basis from which to learn, and a concrete example of some core concepts to work with.

Also, a quick word of warning: this post will breeze through a lot of implementation-specific details. A good understanding of object-oriented programming is recommended. I'm going to be using [Crystal](https://crystal-lang.org) as the implementation language, primarily because it is strongly-typed and has first-class enums. These aren't necessary, but make some parts of lexing and parsing much more concise. If you are familiar with Ruby, Crystal should seem pretty similar.

Lastly, you may have noticed that this post is titled with "compilers", yet I said we'll be building an "interpreter”. In reality, we’ll be doing both. First, we’ll take source code and compile it into an in-memory bytecode-like structure, then interpret that structure to execute the code. Overall, that means the project that we’ll be building is an “interpreter that just so happens to have a compiler inside it”, something that is really common for modern languages. Ruby, Python, Lua, and JavaScript are all examples of languages that do some compilation before running as an interpreter. For simplicity, I’ll just be using the word “interpreter” from here on out.

## Background and Overview

To start building an interpreter, we first need to define what the language looks like. One of the most common syntaxes for doing so is the [Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form) or EBNF. It provides a clean, consistent way of defining [grammars](https://en.wikipedia.org/wiki/Formal_grammar), or structural definitions of valid syntax. Perhaps more importantly, it's consistency means there are [plenty of tools out there](https://en.wikipedia.org/wiki/Comparison_of_parser_generators) to automatically generate parsers given an EBNF input.

For simplicity, we won't be making use of those tools here, but it's still useful to look at our language in this EBNF form. So, without further adieu, here it is:

```text
program    ::= statement program | statement
statement  ::= assignment | expr
assignment ::= '@' name '=' expr
expr       ::= term '+' expr | term
term       ::= factor '*' term | factor
factor     ::= '(' expr ')' | number | name
number     ::= [0-9]+
name       ::= [a-z]+
```

In short, each line above represents what's called a [_production rule_](https://en.wikipedia.org/wiki/Production_(computer_science)) or _non-terminal_. The name of the rule is the left-hand side of the `::=` and the right-hand side is the series of productions that make up that rule. A production rule can have alternative definitions separated by the pipe character (`|`). For example, the first production rule is called `program`, and it consists of either a `statement` followed by another `program`, or a single `statement` with nothing following it.

The alternative to non-terminals are [_terminal symbols_](https://en.wikipedia.org/wiki/Terminal_and_nonterminal_symbols#Terminal_symbols), and are represented as strings in single quotes, such as `'='` and `')'` shown above, or regex patterns in the case of `number` and `name`.

The nice thing about this grammar is that the [order of operations](https://en.wikipedia.org/wiki/Order_of_operations) are automatically enforced. In other words, using this grammar ensures that multiplication happens before addition, expressions in parentheses evaluate before multiplication, and so on. As you can imagine, parsing these kinds of expressions is common, so much so that there are specialized [Operator-precedence grammars and parsers](https://en.wikipedia.org/wiki/Operator-precedence_grammar) to make their evaluation more efficient. For simplicity, that won't be covered here, but it's a worthwhile read nonetheless.

Anyway, enough theory. What kind of statements does this grammar accept? The answer is any basic algebraic expression as well as assignments to variables. Here are some examples:

```ruby
1 + 1
@sum = 2 + 2 + 2 + 2
@b = a * 2 - 5
@c = (b - a) * 4
@d = sum * b + a / 3
```

Just as important are statements that this grammar does _not_ accept. For example, any operator (`+`, `-`, `*`, `/`, `=`) all need operands on either side and parentheses must be matched. That said, the following are _not_ accepted by this grammar:

```ruby
- 1
a = 2
b 3
c ** 4
a = b = 3
4 /
1 + 1  2 + 2
b + @a
```

Something to note is that these examples all include various whitespace in them. In general, whitespace is ignored when writing EBNFs as it doesn't affect how the language is parsed. It can sometimes be explicitly brought up to clear up ambiguities, but in this case it isn't necessary. This will be covered more clearly in the Lexer section later on.

As an exercise, try to figure out what production rules these examples would match or not match according to our EBNF. As a starting point, let’s look at the first statement, `1+1`.

We start at `program` and see that no matter what, it must start with a `statement`, so we check that production rule. `statement` can either be an `assignment` or an `expr`, but an `assignment` must start with an `@` character, which our example doesn’t have, so we need to look at `expr` instead. We can use the same logic to get down to `number`, where we finally match the `1`. After completing the `number` production rule, we retrace our steps back upward to try to match the `+`. Looking upward, we’ve finished `factor` as well, and the next character is not `*`, so `term` is also done. Finally, we get back to `expr` where we can match the `+` and continue on with the first definition.

---

Whew. That was a lot of words. If your eyes glazed over everything above, don't worry (mine did, too). It's not _really_ important for just writing an interpreter, but hopefully it will serve as a reference point if anything further on doesn't make sense.

For the most part, that's all the theory we'll need to get into. From here on out, we'll see more concrete examples and explanations.

In general, interpreters have three phases: Lexing, Parsing, and Interpreting. Going through and implementing these in order, then, means we'll end up with a working interpreter.

With that out of the way, let's get building.


## Lexing

Lexing involves reading a source material character by character, splitting it up into a stream of tokens and classifying each one. I often find this to be the most tedious phase because there aren't many abstractions to make without sacrificing efficiency. Luckily, with the simple language we're implementing here, it shouldn't be _too_ bad.

First up, let's define a way of classifying tokens. A simple way to do this is with a `TokenType` enum.

```ruby
# token.cr
enum TokenType
  NAME        # [a-z]+
  NUMBER      # [0-9]+
  PLUS        # +
  MINUS       # -
  STAR        # *
  SLASH       # /
  EQUAL       # =
  LPAREN      # (
  RPAREN      # )
  AT          # @
  WHITESPACE  # spaces, newlines, tabs, etc.
  EOF         # End of File character
end
```

Second, we'll need an object to represent a token, including its type and the exact value it represents. For example, a `NUMBER` could be 0, or 1, or 2, and so on.

```ruby
# token.cr
class Token
  property type : TokenType = TokenType::EOF
  property value : String = ""
end
```

Note that `value` is a `String`. Even for `NUMBER` tokens, they're stored as `String`s in the lexing phase. We'll deal with parsing these into numbers during the interpreting phase.

Simple enough; now for the tedious part. For simplicity, let's assume our input will always be a String. All we need to start creating tokens is to iterate the input and identify or "consume" tokens as they come up. Since we're in an object-oriented language, let's wrap this responsibility up in a class. There's also a little bit of boilerplate here that I'll try to explain inline.

```ruby
# lexer.cr
require "./token.cr"

class Lexer
  # The source code being lexed.
  property source : String
  # The current position in the source String.
  property pos : Int32
  # The token currently being lexed.
  property current_token : Token

  def initialize(@source : String)
    @pos = 0
    @current_token = Token.new
  end

  # Reset the current token
  def advance_token
    @current_token = Token.new
  end

  # Scan the source until the next token has been completed and return
  # that new token.
  def read_token
    # We’ll get here next!
  end


  # Return the character at the current position in the source.
  # If there is no next character, return an EOF.
  private def current_char
    @source[@pos]? || '\0'
  end

  # Add the current character to the token buffer and advance the
  # position in the source by one character.
  private def read_char
    @current_token.value += current_char
    @pos += 1
  end
end
```

I like to call this a "forgetful" lexer, as it doesn't keep track of the tokens that have already been parsed. Most interpreters keep a list of lexed tokens to help error reporting and backtracking, but that's less useful here.

Now, let's implement `read_token`. In our language, most tokens are a single character, and those that aren't are easily distinguishable. That means the efficient mode of lexing is also clean: we can switch over the current character and deal with multi-character tokens at the end.

With that, the `read_token` function can look something like this:

```ruby
# lexer.cr
def read_token
  # Ensure a clean slate before lexing the next token.
  advance_token

  # Switch over whatever the current character is.
  case current_char
  when '\0'
    read_char
    @current_token.type = TokenType::EOF
  when '+'
    read_char
    @current_token.type = TokenType::PLUS
  when '-'
    read_char
    @current_token.type = TokenType::MINUS
  when '*'
    read_char
    @current_token.type = TokenType::STAR
  when '/'
    read_char
    @current_token.type = TokenType::SLASH
  when '='
    read_char
    @current_token.type = TokenType::EQUAL
  when '('
    read_char
    @current_token.type = TokenType::LPAREN
  when ')'
    read_char
    @current_token.type = TokenType::RPAREN
  when '@'
    read_char
    @current_token.type = TokenType::AT
  when .ascii_whitespace?
    # Keep reading characters as long as they count as whitespace.
    while current_char.ascii_whitespace?
      read_char
    end
    @current_token.type = TokenType::WHITESPACE
  when .ascii_number?
    # Keep reading characters as long as they are numeric digits.
    while current_char.ascii_number?
      read_char
    end
    @current_token.type = TokenType::NUMBER
  when .ascii_letter?
    # Keep reading characters as long as they are alphabetic.
    while current_char.ascii_letter?
      read_char
    end
    @current_token.type = TokenType::NAME
  else
    raise "Unrecognized character `#{current_char}`."
  end

  # Return the newly read token
  # @current_token.value = @buffer.to_s[0..-1]
  @current_token
end
```

Tedious, right? Thankfully, the `.ascii_*?` methods on `Char` from the stdlib do a good job of cleaning up conditionals.

In any case, that's all there is to it! We can test it out by passing in a string and repeatedly calling `next_token`.

```ruby
# Create a new lexer with some source code
lexer = Lexer.new("1 + 2 + 3")
# Read some tokens out
lexer.read_token #=> #<Token:0x... @type=TokenType::NUMBER>
lexer.read_token #=> #<Token:0x... @type=TokenType::WHITESPACE>
lexer.read_token #=> #<Token:0x... @type=TokenType::PLUS>
# ...
```

## Parsing

Parsing (often treated as a "superset" of lexing) consumes the token stream generated from lexing and determines the semantic meaning behind them. This is also where the syntactic structure of our language gets enforced. The parser handles for determining whether the source program is valid. If not, it should error out (ideally informing the user _why_ their source is invalid).

The parser's output also needs to be something that the interpreter can understand. In most cases, this would be an [Abstract Syntax Tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree) or [N-address code](http://www.nkavvadias.com/hercules/nac-refman.html). These are particularly useful as a flexible representation of the meaning of a program. In other words, it's easy to generate these from one language and then use it to construct a program in a different language with the same meaning.

However, generating and interpreting an AST requires a fair amount of boilerplate, and is really more complex than necessary for our simple language. Instead, we'll go with a basic Virtual Machine setup and have the parser generate a sequence of instructions to feed into that machine. We'll get more into how these instructions are handled later on, but for now, we just need a structure to represent them:

```ruby
# instruction.cr
enum InstructionType
  PUSH
  STORE
  LOAD
  ADD
  SUBTRACT
  MULTIPLY
  DIVIDE
end

class Instruction
  property type : InstructionType
  # Arguments that the instruction requires to execute properly.
  # For example, `PUSH` needs an argument of _what_ to push onto the stack.
  property args : Array(String)

  def initialize(@type : InstructionType, @args = [] of String); end
end
```

Now, for the parser itself: by treating parsing as a superset of lexing, the `Parser` class itself becomes simple. Object-orientation makes this easy, as we can extend the `Lexer` class to create a `Parser`.

```ruby
# parser.cr
require "./instruction.cr"

class Parser < Lexer
  # The list of instructions parsed from the source code. This is what will be fed to the interpreter.
  property instructions : Array(Instruction) = [] of Instruction

  # Advance through the token stream until a significant token is encountered.
  # This abstraction allows the rest of the parser to quietly ignore whitespace
  # and other insignificant tokens.
  def advance
    while read_token.type == TokenType::WHITESPACE; end
    current_token
  end

  # Check if the current token matches the given type and return
  # truthy if it does. Additionally, if the type matches, automatically
  # advance to the next significant token.
  def accept(type : TokenType)
    token = current_token
    if current_token.type == type
      advance
      token
    end
  end

  # Similar to `accept`, but raise an error if the type does not match.
  def expect(type : TokenType)
    accept(type) || raise "Expected #{type}, got #{current_token.type}."
  end

  def parse_program
    # Coming up next!
  end
end
```

Hopefully that's all fairly straightforward and we can dive into the actual parsing. This class takes advantage of the methods defined in `Lexer`, so if you see something that's not defined in `Parser`, be sure to check there for the definition.

Finally, we're ready to start _actually_ parsing. We'll be implementing what's known as a [Recursive Descent Parser](https://en.wikipedia.org/wiki/Recursive_descent_parser). In particular, this will be a [top-down](https://en.wikipedia.org/wiki/Top-down_parsing), [LL(1) parser](https://en.wikipedia.org/wiki/LL_parser) who's structure closely resembles the EBNF from earlier and is fairly easy to write out by hand. For reference, here's that EBNF again:

```text
program    ::= statement program | statement
statement  ::= assignment | expr
assignment ::= '@' name '=' expr
expr       ::= term '+' expr | term
term       ::= factor '*' term | factor
factor     ::= '(' expr ')' | number | name
number     ::= [0-9]+
name       ::= [a-z]+
```

The basic idea is that we'll start by parsing a `statement`, first trying to parse an `assignment`, then trying an `expr` if that doesn't work. To parse an `assignment`, we'll try to parse a `name`, then the equals operator, then an `expr`. Each production in the EBNF will be it's own method, so we can just them as `parse_statement`, or `parse_term`, or `parse_number`.

So let's look at `parse_program`:

```ruby
def parse_program
  advance
  until @current_token.type == TokenType::EOF
    parse_statement
  end
end
```

Fairly simple, right? We're just saying that `program` should keep trying to parse `statement`s until the source code is reached. The `advance` at the beginning just makes sure that any whitespace at the beginning of the program is properly ignored.

But what about `parse_statement`? `statement` has two _different_ production rules that define it, so we need to look at how they can be differentiated. This is one of the primary concerns in LL grammars: the type of production to be parsed _must_ be able to be determined by the current token in the input. Otherwise, there is ambiguity and it's hard to know how to continue parsing. (It's possible to work around this using either backtracking or better lookahead, but those make the parser much more complicated).

In any case, if we look at what makes up a `statement`, we can see that an `assignment` will always start with an `@`, and an `expr` never will, so if we check if the current token is an `@`, we can decide what production to continue.

```ruby
def parse_statement
  if accept(TokenType::AT)
    parse_assignment
  else
    parse_expr
  end
end
```

That's not too bad! With this example, we're also seeing how `accept` can be used to cleanly show the optional expectation of a token. If we used `expect` instead, the parser would raise an error if it didn't see an `@`, which wouldn't allow the alternative `expr` to be considered.

In `parse_assignment`, we'll see our first instruction generation.

```ruby
def parse_assignment
  name = expect(TokenType::NAME)
  expect(TokenType::EQUAL)
  parse_expr
  instructions << Instruction.new(InstructionType::STORE, [name.value])
end
```

Notice first that we aren't expecting the `@` token. It was already matched and consumed in `parse_statement`, so we don't have to worry about it here. Next, we're expecting the `NAME` token, and then remembering it for later on when we generate the instruction.

The last line is where we actually generate the instruction. It may seem a little odd that we're just immediately generating and storing the instruction, but that's one of the nice things about recursive descent parsers. By calling `parse_expr`, we can know for sure that the appropriate instructions that need to come _before_ this `STORE` instruction will have already been generated.

With the basics in place, I'm going to speed through the other three methods, as they're mostly identical to each other anyway.

```ruby
def parse_expr
  parse_term
  if accept(TokenType::PLUS)
    parse_expr
    instructions << Instruction.new(InstructionType::ADD)
  elsif accept(TokenType::MINUS)
    parse_expr
    instructions << Instruction.new(InstructionType::SUBTRACT)
  end
end

def parse_term
  parse_factor
  if accept(TokenType::STAR)
    parse_term
    instructions << Instruction.new(InstructionType::MULTIPLY)
  elsif accept(TokenType::SLASH)
    parse_term
    instructions << Instruction.new(InstructionType::DIVIDE)
  end
end

def parse_factor
  if accept(TokenType::LPAREN)
    parse_expr
    expect(TokenType::RPAREN)
  elsif name = accept(TokenType::NAME)
    instructions << Instruction.new(InstructionType::LOAD, [name.value])
  elsif number = accept(TokenType::NUMBER)
    instructions << Instruction.new(InstructionType::PUSH, [number.value])
  end
end
```

The only really exciting things here are branching based on the operator. If `accept` does _not_ match a token, it _does not_ advance the lexer, so we can use it to check for the existence of a token without worry of messing up the parser's state. Also notice that the first branch of `parse_factor` does _not_ generate an instruction. As mentioned before, calling `parse_expr` ensures that all of the needed instructions are generated beforehand, and the parentheses only serve to influence the order of operations (they don't perform an operation that would need representation as an instruction).


Alright, that took a while, but we made it. We now have a working Lexer and Parser that together generate a series of instructions that can represent any valid program. Let's try it out real quick:

```ruby
parser = Parser.new("a = 1 + 2")
parser.parse_program
parser.instructions
#=> [Instruction::PUSH(1), Instruction::PUSH(2), Instruction::ADD, Instruction::STORE(a)]
```

Something you may have already noticed, but is likely more apparent here is that what we've essentially done is transformed our language that uses infix notation for expressions into [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation) or RPN. While less _human_-readable, RPN is much simpler for a computer to reason about as there is no chance for ambiguity and can be read from left to right in one pass (no parentheses, order of operations, etc.).

Also notice that we didn't even have to instantiate a `Lexer` here. Because `Parser` inherits from `Lexer`, that entire phase is hidden from the outside.


## Interpreting

Interpreting is where the meaning of the program is realized and executed. Somewhat surprisingly, with all of the ground work laid out in the lexer and parser, this is actually the easiest step. The interpreter just has to take in the instruction list generated by the parser and execute each one in order.

I mentioned previously that we're using a Virtual Machine design for our interpreter. In particular, we'll be implementing a [Stack Machine](https://en.wikipedia.org/wiki/Stack_machine). The primary reason for this is that it is extremely easy to reason about if you are familiar with stacks (what goes in last comes out first, etc.). The primary alternative to stack machines are [Register Machines](https://en.wikipedia.org/wiki/Register_machine) which are more akin to what the computer you're on right now uses in its processor. Register machines are much more flexible and allow for [Pipelining](https://en.wikipedia.org/wiki/Instruction_pipelining), which can greatly improve performance, but is far out-of-scope for this introduction.

There isn't much boilerplate this time, but for consistency I'll lay out the class structure here first:

```ruby
# interpreter.cr
class Interpreter
  # The 'stack' in our Stack Machine.
  property stack : Array(Float64)
  # A table of variables and their current values.
  property variables : Hash(String, Float64)
  # The list of instructions to be interpreted.
  property instructions : Array(Instruction)
  
  def initialize(@instructions : Array(Instruction))
    @stack = [] of Float64
    @variables = {} of String => Float64
  end
  
  def run
    # Coming up next!
  end
end
```

Like I said, not too much boilerplate. One thing to notice is that our stack is built on an array of `Float`s. The reason for using `Float` instead of `Int` is for division, where something like `10 / 4` is `2.5`, but using an `Int` would truncate that to just `2`. Even though our language only supports integer inputs, it's simple enough to support floating-point outputs, so it might as well just work that way from the start.

The other important piece is the `variables` property. `variables` acts as a [Symbol Table](https://en.wikipedia.org/wiki/Symbol_table), which stores the current value of variables that exist in the program. For example, a program like `a = 1` would create an entry `a` in `variables` with a value of `1`.

Now, we come to the final method of the entire project, `run`. Interestingly, this method is going to look very similar to `read_token` from the lexer, and in a way, that makes sense. All that `run` has to do is read the current instruction, perform the appropriate action for it, and repeat, similar to how the Lexer would read a character, perform an action, and repeat. There's not too much more to explain, so let's just see what it looks like:

```ruby
def run
  # Attempt to remove the first element from `instructions`.
  # Return nil if none exists, thus breaking out of the loop.
  while (inst = instructions.shift?)
    case inst.type
    when InstructionType::PUSH
      # PUSH <value>
      # Push <value> onto the stack.
      @stack.push(inst.args[0].to_f64)
    when InstructionType::STORE
      # STORE <name>
      # Pop the top value from the stack and store it as the variable <name>.
      @variables[inst.args[0]] = @stack.pop
    when InstructionType::LOAD
      # LOAD <name>
      # Push the value of the variable <name> onto the stack.
      @stack.push(@variables[inst.args[0]])
    when InstructionType::ADD
      # ADD
      # Pop the two values at the top of the stack, add them together, and push the result.
      b = @stack.pop
      a = @stack.pop
      @stack.push(a + b)
    when InstructionType::SUBTRACT
      # SUBTRACT
      # Pop the two values at the top of the stack, subtract them, and push the result.
      b = @stack.pop
      a = @stack.pop
      @stack.push(a - b)
    when InstructionType::MULTIPLY
      # MULTIPLY
      # Pop the two values at the top of the stack, multiply them, and push the result.
      b = @stack.pop
      a = @stack.pop
      @stack.push(a * b)
    when InstructionType::DIVIDE
      # DIVIDE
      # Pop the two values at the top of the stack, divide them, and push the result.
      b = @stack.pop
      a = @stack.pop
      @stack.push(a / b)
    end
  end
end
```

And that's it! We now have a working interpreter that executes a series of instructions! The only thing that's missing is some output to inform the user of the results of their code. Just a little bit of work outside the interpreter can handle that:

```ruby
require "./lexer.cr"
require "./parser.cr"
require "./interpreter.cr"

# Parse the source code into instructions.
parser = Parser.new(%q(
  @a = 2 + 2
  @b = 1 + 3 * 3
  @c = b / a
))
parser.parse_program

# Run those instructions.
vm = Interpreter.new(parser.instructions)
vm.run

# Output results to the user.
puts "Stack contents: #{vm.stack}"
puts "Variables:"
vm.variables.each do |name, value|
  puts "\t#{name} = #{value}"
end
```

Running this code should give you the following:

```text
crystal run src/runner.cr
Stack contents: []
Variables:
    a = 4
    b = 10
    c = 2.5
```

Awesome! We now have the complete system working together. The Parser and Lexer work together to compile a series of instructions, then those instructions are fed to the Interpreter to execute the code!

## Wrapping up

That was a _whole_ lot of words and code. This has been a long and winding journey, so if you've stuck around, thank you! I hope this post made sense and didn't scare you off. As I said earlier, there's a _lot_ of theory that goes into compilers, but I wanted this post to focus more on creating a concrete example that demonstrates the core concepts visually.

If you’re interested in going further, there are plenty of other resources already out there that go through individual concepts in a lot more depth than I've laid out here:

  - **Virtual Machines** are commonly explored in blog posts like this one. In fact, the main inspiration for this post was [Bartosz Sypytkowski's Simple Virtual Machine blog post](http://bartoszsypytkowski.com/simple-virtual-machine/). It's an excellent look into the more complicated features of a Virtual Machine.
  - **Parser generators** tend to be more on the academic side, but there are some great resources out there (particularly for [Lex](http://dinosaur.compilertools.net/lex/) and [Yacc](http://dinosaur.compilertools.net/yacc/)) that go through more advanced lexing and parsing, as well as more of the theory behind them.
  - **Books on compiler design** aren't the most accessible way to get into compilers, but if you really want to get serious with them, [The Dragon Book](https://www.amazon.com/Compilers-Principles-Techniques-Tools-2nd/dp/0321486811) and [Engineering a Compiler](https://www.amazon.com/Engineering-Compiler-Second-Keith-Cooper/dp/012088478X/) are some quintessential resources.
  - **Other introductions:** I'm certainly not the first to write this kind of introduction to writing a compiler. Notable others include [Jack Crenshaw's Let's Build a Compiler](https://compilers.iecc.com/crenshaw/) (sadly not fully finished, but still a great in-depth resource), and a more modern take, [Let's Build a Simple Interpreter by Ruslan Spivak](https://ruslanspivak.com/lsbasi-part1/). Both of these go into far more detail than I did here and touch a lot more on theory than anything else. Other projects like the [Super Tiny Compiler](https://github.com/thejameskyle/the-super-tiny-compiler) are more similar to this post, but all inlined in the code.
  - **Other small languages** are also a great way to get in and see a working compiler in action, without being overwhelmed by long histories and complicated designs. While thinking about this post, I looked at [Charly](https://github.com/charly-lang/charly), [Luna](https://github.com/tj/luna), [wren](https://github.com/munificent/wren), and [Nim](https://github.com/nim-lang/Nim) among many others for inspiration. I'll also shamelessly plug my own language, [Myst](https://github.com/myst-lang/myst), where I used the same process laid out in this post as a starting point.

There's a huge world out there and plenty of big projects that can sometimes seem like guarded castles, but I hope that after reading this you'll be inspired to start hacking on your own compilers and languages. 

Designing a language and having to work through all of the things I take for granted when working on other projects has by far been the best learning experience I've had in a long time. Not only have I been exposed to the inner workings of the things that make my entire career possible, but I've grown a deeper appreciation for the languages I use everyday, seeing the creative thinking and compromises their designers had to make to see their ideas to fruition.
