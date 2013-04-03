I use tmux splits (panes). Inside one of these panes there's a Vim process, and it has its own splits (windows).

In Vim I have key bindings `C-h/j/k/l` set to switch windows in the given direction. (Vim default mappings for windows switching are the same, but prefixed with `C-W`.) I'd like to **use the same keystrokes for switching tmux panes**.

An extra goal that I've solved with a dirty hack is to *toggle between last active panes* with `C-\`.

![](http://f.cl.ly/items/3O0B0U2I041c420u163w/tmux%20vim%20panes%20hack.png)

Here's how it should work:

1. If I'm in "vim window 2", going in left (`C-h`) or down (`C-j`) direction should switch windows _inside vim_.
2. However, if I'm in "vim window 3", going right (`C-l`) or down (`C-j`) should _select the next tmux pane_ in that direction.

## The solution

The solution has 3 parts:

1. In `~/.tmux.conf`, I bind the keys I want to execute a custom `tmux-vim-select-pane` command;
2. `tmux-vim-select-pane` checks if the foreground process in the current tmux pane is Vim, then forwards the original keystroke to the vim process. Otherwise it simply switches tmux panes.
3. In Vim, I set bindings for the same keystrokes to a custom function. The function tries to switch windows in the given direction. If the window didn't change, that means there are no more windows in the given direction inside vim, and it forwards the pane switching command to tmux by shelling out to `tmux select-pane`.

## Installation

```sh
curl -fsSL https://gist.github.com/mislav/5189704/raw/install.sh | bash -e
```