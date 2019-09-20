---
layout: post
post_author: Zack Kayser
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: The Beauty of Folds and Union Types in Elm
publish_date: 2018-04-06 16:58:00 +0000
feature_post_image: ''
post_images: []
slug: the-beauty-of-folds-and-union-types-in-elm

---
# The Beauty of Folds and Union Types in Elm



### The Problem



After finishing up a side project in Elm about a month ago, I decided that I enjoyed the language so much that I might as well start work on a whole new side project. At the time I started the project, I had a couple weeks before I was scheduled to give an introductory talk on Elm at the Cincinnati JavaScript meetup (which you can watch [here](https://www.youtube.com/watch?v=emGIAWeFVUU&t=11s)), and I wanted to have a smaller, relatively straightforward Elm app to show off as an example I could point to during my talk. The app is a Scrabble-esque word game that follows along with the rules of Scrabble pretty closely, except that players do not share a board (I have more features planned... stay tuned for those!)



A word game where players are simply laying tiles on a board seemed like a relatively well-defined domain, and I didn't anticipate running into any super scary problems. After getting a drag and drop implementation up and running smoothly in just an hour or so of work on the first day, I did, however, run into a challenge on the second day: How do you validate whether or not a turn taken by a player is valid?



Let's set up the problem in a little more detail. I chose to represent my board, or `Grid`, as a simple list of 225 `Cell`s. I did not do anything fancy like break the board down into a two-dimensional list or array -- it is simply a single list of 225 elements (15 x 15). A `Cell` is a record that carries with it data including a `Position` (x, y) and whether or not a `Tile` has been placed on the given cell. My model also contains a `Context` record that stores the state of the board, a list of the moves made by the player in his or her current turn, and the player's remaining unplayed tiles. Here are the data structures in question for reference:



```elm

type alias Grid =

    List Cell



type alias Cell =

    { position : Position

    , multiplier : Multiplier

    , tile : Maybe Tile

    , isCenter : Bool

    }



type alias Position =

    ( Int, Int )



type alias Tile =

    { letter : String

    , id : Int

    , value : Int

    , multiplier : Multiplier

    }



type alias Move =

    { tile : Tile

    , position : Position

    }



type alias Context =

    { grid : Grid

    , movesMade : List Move

    , tiles : List Tile

    }

```



Okay, so the problem arises when we need to check that the tiles placed on the board by a player are placed in sequence along a single row or column. For example, you cannot place a tile in the upper right cell and one on the bottom left in a single turn. You also cannot place one tile in the upper right and one in the bottom right unless you also placed tiles on all the cells in between (or unless there were already tiles on the cells in between from previous turns). How do we check if a turn is valid given the simple list data structure that we're using and the data that we have access to?



Contrary to more mainstream imperative languages, the `List` data structure in Elm does not have a straightforward way like `grid[index]` to access an element at a given index like you might do with an array in JavaScript. (Sidenote: Elm actually does have an `Array` data type with a `get` function to access a given index. I would have reached for this instead of `List` if I felt I really needed it, but I'm pretty satisfied with the solution I came up with). Not having immediate access to an element at a given index in the collection makes it challenging to move back and forth by doing something like `grid[index - 1]` or `grid[index + 1]`, so the solution isn't as trivial as starting out by fetching the element at some index where we know the player placed a tile and navigating back and forth until we see all the tiles played in the current turn, verifying that all the tiles were connected by other tiles with no blank spaces in between. Phew... okay, so admittedly, that probably would not be a trivial solution, but the point is, that is clearly not the way to go given the way `List` works in Elm!



## Solution Part 1: The Easy Stuff



Let's begin with the easy part of the solution before we move into the meaty part of the Elm code. We need to start off by verifying that all of the tiles placed by a player were put either in a single row or a single column. To get this, we can take the list of `Move`s made by the player in the current turn from the `Context` record and map over them, pulling out their `Position`s as we go. This will return us a `List Position` -- a list of positions where the user placed tiles. Here it is in Elm:



```elm

List.map .position context.movesMade

```



`map` is one of the three quintessential tools used all the time by functional programmers (along with `filter` and `reduce/fold`). If you are entirely unfamiliar with FP, `map` in general (not just in Elm) takes a function and a container of things, and applies the function to each element inside of the container. The return value is the container with the new values based on the results of the function passed in.



In the code above, we are using some Elm syntactic magic to shorten things up a bit. The more complete form of the above code would look like: `List.map (\move -> move.position) context.movesMade`. Since `context.movesMade` contains `Move` records and `Move` records contain a `position` key, Elm lets us just call `.position` as the function to call on the elements we are mapping over in our "container" (list).



So now that we have a list of positions, how do we know if those positions were all along the same row or column? Also, what do we need to return to indicate what the result we got was?



To answer these questions, let's create a new Union Type (more about Union Types below) that represents the validity of our results. Let's call it `Dimension` and define it to take one of three possible values:



```elm

type Dimension

= Invalid

| Row Int

| Column Int

```



Cool. Now we have a data type that can accurately and concisely represent the information we need to know after running this validation. The `Int` values contained by `Row` and `Column` represent the number of the row or column marked as valid. We will use this result later.



What we can do now is take the result of our `List.map` function from above and place it in a case statement as shown below:



```elm

case List.map .position context.movesMade of

[] ->

Invalid



(row, column) :: tail ->

if List.all (\(r, _) -> r == row) tail then

Row row

else if List.all (\(_, c) -> c == column) tail then

Column column

else

Invalid



```



The first case we are matching on above is pretty straightforward: if the player didn't make any moves yet, there are obviously no moves to submit, so it is `Invalid`. The second match is a bit more dense. The case statement pattern matches on the list, taking the head (the first value in the list) and pattern matching on the head to give us `row` and `column` variables. These variables will be in scope for us inside the body of this branch. The `:: tail` part of this pattern match is how you destructure a list in Elm: (`head :: tail`) where `head` is the first element of the list and `tail` is everything else.



Inside this branch, we are using the `List.all` function and capitalizing on pattern matching some more. `List.all` takes a function which takes a list element and returns a boolean. The second parameter to `List.all` is a list of things to run the function against. It returns `True` if the given function evaluates to `True` for every element in the list. Inside of the admittedly obscure anonymous function `(\(r, _) -> r == row)`, we are pattern matching on the `Position` elements coming in from the remainder (`tail`) of the list we mapped over earlier and we are taking only the row value. We check to see if the current row value `r` is equal to the `row` value we matched on in the list head on this branch of the case statement. If all elements return `True`, then we know that the player made all of his or her moves along a row, and we know which row that was as well. If this is not true, we then try the same thing for columns. If that fails, we know that something is amiss and we return `Invalid`. Note that if we only have one `Move` in the list, we are always going to get back `Row row` from this function. This may not be immediately obvious behavior, and we might to refine how this works later on.



Alright, so given a `Dimension`, how do we get a finer-grained validation on a player's moves for the current turn?



## Taking a Step Back: Fold/Reduce, the Functional Programmer's Swiss Army Knife



Before we get into the nitty-gritty details, let's take a step back and go over arguably the most important function in functional programming: `fold` (and its mostly identical twin `reduce`). I first became acquainted with `reduce` when I started doing Elixir a couple of years ago, and it really changed the way I look at programming when it clicked for me. Elm has two functions, `foldl` and `foldr` that essentially do the same thing as `reduce`. The `l` and `r` varieties denote which way the function runs through a collection -- either starting from the left or the right, thus `l` and `r`. For those of you who are unfamiliar with the concept, let's take a look at a simple example before getting into some more complex examples.



In general, `fold`s take three parameters: a function (which itself takes two parameters), an initial value, and a list that we want to `fold` over (think "loop" if "fold" sounds weird to you). This allows us to take a list of things and transform those things into some single value. To create a single value from our list of things, the `fold` function needs us to give it a single thing to start working with. This is the initial value parameter. So, say we have a list of numbers [1, 2, 3] and we want to know the sum of those numbers. The initial value we need to give the function is `0` because it hasn't added any of those numbers together yet. This is just the starting point.



The function we pass to `fold` is what takes care of doing the computational work to build our final output value, and in this simple example it would be a simple addition function. As I mentioned earlier, the function parameter is a higher-order function that takes two parameters: the current element from the list we are "looping" over, and an accumulator value that is in charge of storing the intermediate states of the output value as we build it up incrementally. This function is commonly referred to as the "folding function". The accumulator value always starts out as whatever we give to `fold` as the "initial value" parameter. For us, this means on our first pass through the list `[1,2,3]`, our folding function will takes the parameters `1` (<-- current element of the list) and `0` (<-- the initial value we explicitly pass to `fold`). In Elm, our folding function might look like this `(\currentElement accumulator -> currentElement + accumulator)`, so on the first pass through we get `(1 0 -> 1 + 0)`. This returns `1`, and this value will be used as the accumulator parameter on the next iteration over the list. Now we get `(2 1 -> 2 + 1)`. This returns 3, which gets passed to the folding function as the accumulator value for the last iteration on `[1,2,3]`, giving us `(3 3 -> 3 + 3)` for our final result of `6`.



Perfect! We were able to implement a `sum` function using `fold`. This alone is not particularly exciting, but `fold` is not limited to just summing up a simple list of integers -- it can be used to reduce a list of things into an entirely different list or even into a data structure of your own making. This is where the real power of `fold` comes from.



With that overview out of the way, let's jump back into the solving the validation problem.



## Folds and Union Types



We defined a union type earlier for `Dimension`s that can take on one of three values: `Invalid`, `Row Int`, or `Column Int`. The cool thing about union types (also known as Algebraic Data Types or ADTs) is that they allow us to model data in a concise but expressive way. We get "value" constructors like `Row` or `Column`, which we can think of as functions that take in one `Int` parameter and return us a `Dimension` type. We can use an arbitrary number of parameters to pass to our own value constructors and the parameter data can even be of different shapes and sizes: they can be lists, strings, records, or even other union types. We can match on them in case statements using pattern matching, which makes using them nice, expressive, and concise.



To reach a solution to the problem of checking whether or not the moves made by a player in their current turn are valid or not, we need some "validator" machinery that will run through the board for the `Dimension` returned from our earlier validation. The "validator" needs to essentially loop through a subset of the board list and keep track of where a user might have made a valid play. Let's define a union type to keep track of this for us:



```elm

type ValidatorState

= NoMoveDetected

| PossibleMoveFound (List Tile)

| MoveDetected (List Tile)

| Validated Play

| Invalidated

```



* The `Play` type above is a record that is encoded and then passed to the server for further work after validation succeeds.



We define five possible states that we might run into while our "validator" is running through the row or column on the board that we are checking against. Let's go over each one and what it means.



1. `NoMoveDetected` will be our initial state. Before we start looping over the list of cells that represent a list or column, our "validator" obviously has not seen any moves or cells that could possibly represent a move by the player. The validator stays in this state until it sees a cell that contains a tile.



2. `PossibleMoveFound` represents a state in which the validator has found a tile in the row or column, but the tile was not a tile played in the current turn. These would be tiles that were played in earlier turns and can be built upon by the player in the current turn. The `PossibleMoveFound` carries around with it the list of tiles that it has seen so far.



3. `MoveDetected` is the state our validator takes when it finds a tile played by the player in the current turn. It takes the list of tiles from `PossibleMoveFound` if that list exists and builds upon it. At this stage, our validator will know on the next empty `Cell` (meaning a `Cell` with no `Tile`) whether or not the move it detected is valid or not. We can determine it is valid by taking the list of moves made and checking if all the tiles played in the current turn are contained in the `List Tile` carried around by `MoveDetected`. If so, we want to validate the move. If not we want to invalidate it.



4. `Validated` represents a valid `Play` and is one of the two possible outputs from our validator. This is nice because our main application code can pattern match on this value, take the `Play` value out, and submit it directly to the server for further processing.



5. `Invalidated` is the other possible output from our validator. It essentially represents any move that was not validated. This could be if we had `MoveDetected` at some point, but then ran into an empty cell and the `List Tile` held by `MoveDetected` did not contain all of the tiles played in the current turn. It might also be if we end up with `NoMoveDetected` or a `PossibleMoveFound` that never reached a valid `MoveDetected`.



Sooo.... How are we going to put all this together? Well, as you may have guessed, we can just use a `fold`! This time we are going to fold our list (i.e. a `Row` or `Column`) into a `ValidatorState` value.



Let's setup the fold by explaining the three parameters needed:



1. The folding function -- Our folding function is going to actually take three parameters this time, the normal two passed to the folding function (the current element of the list and the accumulator), along with another parameter we are going to supply ourselves. The other parameter will be a `List Tile` that represents the list of tiles played in the current turn. The type signature for our folding function then, will look like this: `List Tile -> Cell -> ValidatorState -> ValidatorState`. That is, it takes a list of tiles, a cell, and a validator state, and it returns a new validator state.



2. The initial value -- We are going to set our initial value to `NoMoveDetected`. This makes sense because we obviously have not detected any moves before we start looking through the list.



3. The list to fold over -- Our list is going to be a list of `Cell` values from the board representing the row or column we are validating.



Let's start it out like this:



```elm

let

playedTiles =

            List.map (\move -> move.tile) context.movesMade

in

List.foldr (updateState playedTiles) NoMoveDetected cells

|> finalizeState playedTiles

```



It is important to note that we are taking advantage of a feature in Elm called partial application in our second parameter `(updateState playedTiles)`. This allows us to pass less than the number of required parameters to a function, and it will return us a new function that is essentially the same but takes in the remaining parameters and locks in the values for the parameters that we supplied. By doing this, we now have a function that will take in the two parameters normally passed to the folding function when we use `fold`.



You can see that instead of using the anonymous function syntax we saw above, we are passing a named function directly as the second parameter to `fold`. Our folding function has a lot of work to do, and it doesn't lend itself to clarity if we try to squeeze it into an anonymous function here. Let's see what the `updateState` function looks like:



```elm

updateState : List Tile -> Cell -> ValidatorState -> ValidatorState

updateState playedTiles cell currentState =

    case currentState of

        NoMoveDetected ->

            case cell.tile of

                Just tile ->

                    if List.member tile playedTiles then

                        MoveDetected [ { tile | multiplier = cell.multiplier } ]

                    else

                        PossibleMoveFound [ { tile | multiplier = cell.multiplier } ]



                Nothing ->

                    NoMoveDetected



        PossibleMoveFound tiles ->

            case cell.tile of

                Just tile ->

                    if List.member tile playedTiles then

                        MoveDetected <| { tile | multiplier = cell.multiplier } :: tiles

                    else

                        PossibleMoveFound <| { tile | multiplier = cell.multiplier } :: tiles



                Nothing ->

                    NoMoveDetected



        MoveDetected tiles ->

            case cell.tile of

                Just tile ->

                    MoveDetected <| { tile | multiplier = cell.multiplier } :: tiles



                Nothing ->

                    if List.all (\tile -> List.member tile.id (idsFor tiles)) playedTiles then

                        Validated <| handleValidationForPlay tiles cell

                    else

                        Invalidated



        Validated play ->

            Validated play



        Invalidated ->

            Invalidated

```



We are taking advantage of pattern matching here in our case statement on the current state of the validator, represented by the `currentState` parameter that gets passed into the function. In the `NoMoveDetected` branch we check to see if the current `Cell` has a tile or not. If it doesn't we can keep our accumulator value `NoMoveDetected`. If the current `Cell` does have a tile, we need to determine whether it was a tile played this turn, or if it was an existing tile on the board. If it was played this turn, we can update the current state to `MoveDetected` and place the tile on to the list of tiles carried around by `MoveDetected`, otherwise we do the same thing but update the state to `PossibleMoveFound` instead.



If our current state is `PossibleMoveFound`, we also need to look at the tile for the current `Cell`. If it empty, we can revert the state to `NoMoveDetected` because this means we have not yet found any tiles played by the player in the current turn. If there is a tile on the current cell, we check to see if the tile was played in the current turn and update the state to `MoveDetected` while we pass along the list of tiles we are carrying around. Otherwise, we keep the state `PossibleMoveFound` and update the list of tiles with the tile from the current cell.



Now, when we are on the `MoveDetected` branch, we are checking to see if the current cell has a tile. If it does, that is good and we keep the state `MoveDetected` while tacking on the new tile to the list of tiles we are carrying around. The current tile may or may not be played by the player in the current turn, but that is okay because it is legal to build a play off of previously placed tiles. If we see that there is no tile on the current cell, we can determine the validity of the play. If the list of tiles we are carrying around in the `MoveDetected` value contains all of the tiles played in the current turn (represented by the `playedTiles` parameter we are supplying to the function), then the move was valid and we can return `Validated play` (the `handleValidationForPlay` function takes care of formatting our list of tiles into a proper `Play` data structure that can be passed on to the server). However, if we see the list of tiles we are carrying around does not contain all of the tiles played in the current round, then the turn is invalid and we return `Invalidated`.



The last two branches of the case statement are pretty straightforward: once we've finished the work of validating, we can just keep the validator state in its current state until the fold is finished. This may cause a small amount of excess work, but we're only folding over 15 elements so it is not too concerning.



After we are done with the fold, we pipe the result into a `finalizeState` function that takes care of some cleanup work (like handling cases where we end up with `NoMoveDetected` or if we ended on a `MoveDetected`). That function is shown here:



```elm

finalizeState : List Tile -> ValidatorState -> ValidatorState

finalizeState playedTiles state =

    case state of

        NoMoveDetected ->

            Invalidated



        MoveDetected tiles ->

            if List.all (\tile -> List.member tile.id (idsFor tiles)) playedTiles then

                Validated <| List.filter (\play -> String.length play.word > 1) [ ScrabblePlay.tilesToPlay tiles ]

            else

                Invalidated



        _ ->

            state

```



Now we have a validator that can not only tell us whether a move was valid or not, it can also give us the data we need to send to the server if the move was valid! Pretty cool :)



I found this solution particularly exciting because I worked on a sizeable side project for several months using Elm and I felt like I never really grokked the true power of union types. I think this solution demonstrates a pretty clear example of how they can be used while shining a light on some of their strengths. I hope you found this post helpful, and if you want to peruse the code, feel free to check it out on my Github page [here](https://github.com/zkayser/elm_scrabble)!



Thank you for reading!