---
layout: post
post_author: Michael Guterl
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'vim + tmux: A Perfect Match'
publish_date: 2014-07-24T18:00:00.000+00:00
feature_post_image: ''
post_images: []
slug: vim-plus-tmux-a-perfect-match

---
> tmux is a terminal multiplexer: it enables a number of terminals
> to be created, accessed, and controlled from a single screen. tmux
> may be detached from a screen and continue running in the
> background, then later reattached.
>
> -- <cite>[tmux manpage][]</cite>

When I first heard of tmux I just assumed it was a beefed up version of screen.
I had used screen many years ago in order to keep an IRC client logged in even
if I was not connected to my shell. At the time I didn't see much benefit in
adding a tool like this to my development environment.

For a long time I heard developers singing the praise of vim and tmux. Hearing
about the release of iTerm2 2.0 and it's integration with tmux sparked my
interest in tmux and vim once again. I still don't know exactly how iTerm2 and
tmux integrate, but I'm really happy with my workflow with vim and tmux.

I have been using vim+tmux together for a couple of weeks now and there's no
looking back. If you're willing to invest a little bit of time to set things
up, you'll quickly make up the time with your streamlined workflow.

## Prerequisites

`brew install tmux`  
`brew install macvim --override-system-vim`  

[Download](http://www.iterm2.com/downloads/stable/iTerm2_v2_0.zip) and install iTerm2 *(optional)*

## Out of the Box

Below are some of the most basic commands for interacting with tmux.

### Create a session

The first thing you'll want to do after installing tmux is create a session.

`$ tmux new -s gaslight-blog`

### Detach session

You can detach from the session at any point by pressing:

`Ctrl-b d`

### Attach session

You can attach to the session from the command line with:

`$ tmux attach -t gaslight-blog`

### Split horizontally

You no longer have to be dependent on your terminal application to create split panes.

`Ctrl-b %`

### Split vertically

`Ctrl-b "`

### Pane Navigation

<table style="width:125px">
<thead>
<tr>
<td>Keybinding</td>
<td>Action</td>
</tr>
</thead>
<tbody>
<tr>
<td>Ctrl-b &#8593;</td><td>Up</td>
</tr>
<tr>
<td>Ctrl-b &#8595;</td><td>Down</td>
</tr>
<tr>
<td>Ctrl-b &#8592;</td><td>Left</td>
</tr>
<tr>
<td>Ctrl-b &#8594;</td><td>Right</td>
</tr>
</tr>
</tbody>
</table>

### tmux.conf

I ran into numerous issues when starting out with vim, tmux, iterm2, and mac
os. My [tmux.conf](https://github.com/mguterl/dotfiles/blob/master/tmux.conf) is pretty
slim and well documented at the moment and I recommend you check it out.

## vim workflow

The main reason that I continue to use tmux is the great integration that can
be achieved with vim+tmux. Prior to using tmux I used MacVim, but if you're
going to use vim and tmux together you'll need to use terminal vim.

### vim-tmux-navigator

Perhaps my favorite feature about using vim and tmux together is
[vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator). This
plugin allows you to treat vim and tmux as one a unified session, with the
ability to seamlessly navigate between vim splits and tmux panes.


<table style="width:125px">
<thead>
<tr>
<td>Keybinding</td>
<td>Action</td>
</thead>
<tbody>
<tr>
<td>Ctrl-l</td><td>right</td>
</tr>
<tr>
<td>Ctrl-k</td><td>up</td>
</tr>
<tr>
<td>Ctrl-j</td><td>down</td>
</tr>
<tr>
<td>Ctrl-h</td><td>left</td>
</tr>
</tr>
</tbody>
</table>

This assumes you're already navigating your vim splits like a sane person using
these bindings by adding this setting to your .vimrc:

```
nnoremap <c-j> <c-w>j
nnoremap <c-k> <c-w>k
nnoremap <c-h> <c-w>h
nnoremap <c-l> <c-w>l
```

### rspec.vim + tslime.vim

One thing that I missed when I was transitioning from emacs to vim was
rspec-mode. I quickly discovered [rspec.vim](https://github.com/thoughtbot/vim-rspec) which allows you to run specs from within your editor.

I didn't spend too much time working with rspec.vim before I was
frustrated by the default behavior which blocks the entire vim
process. This is no fault of rspec.vim, rather it is more of an
issue with vim. Thankfully rspec.vim has a lot of flexibility for
generating custom commands and you can easily integrate with
[Dispatch](https://github.com/tpope/vim-dispatch) or
[tslime.vim](https://github.com/jgdavey/tslime.vim). I am currently
using tslime.vim using the instructions
[here](http://robots.thoughtbot.com/running-specs-from-vim-sent-to-tmux-via-tslime)
and it seems to be working well.

### vim-like copy and paste

I am still learning the ins and outs of copy and paste while using vim and tmux
together. If you're willing to give up using the mouse for selection [this
article](http://robots.thoughtbot.com/tmux-copy-paste-on-os-x-a-better-future)
provides instructions on getting copy and paste setup very similar to vim.

### colors

When I first started vim inside of tmux the colors were not correct. After
searching I found that I needed to add this to my tmux.conf:

```
set -g default-terminal "screen-256color"
```

I also had to make sure that iTerm2 was reporting the terminal type as
`xterm-256color`.

## References

This article was made possible with the help of many other articles:

[http://blog.sanctum.geek.nz/reloading-tmux-config/](http://blog.sanctum.geek.nz/reloading-tmux-config/)  
[https://wiki.archlinux.org/index.php/tmux](https://wiki.archlinux.org/index.php/tmux)  
[http://robots.thoughtbot.com/a-tmux-crash-course](http://robots.thoughtbot.com/a-tmux-crash-course)  
[https://coderwall.com/p/j9wnfw](https://coderwall.com/p/j9wnfw)  
[http://robots.thoughtbot.com/tmux-copy-paste-on-os-x-a-better-future](http://robots.thoughtbot.com/tmux-copy-paste-on-os-x-a-better-future)  
[http://www.dayid.org/os/notes/tm.html](http://www.dayid.org/os/notes/tm.html)  
[https://github.com/altercation/solarized/issues/159#issuecomment-5566892](https://github.com/altercation/solarized/issues/159#issuecomment-5566892)  
[http://stackoverflow.com/questions/10158508/lose-vim-colorscheme-in-tmux-mode](http://stackoverflow.com/questions/10158508/lose-vim-colorscheme-in-tmux-mode)  

[tmux manpage]:http://manpages.ubuntu.com/manpages/precise/en/man1/tmux.1.html