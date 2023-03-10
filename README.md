# Simple Link Manager

This CLI tool allows to store & retrieve links with tags utilizing git repo storage.

https://user-images.githubusercontent.com/32129186/219489278-324db874-930c-43a8-abad-14322406c8f0.mov


## Install

- [FZF](https://github.com/junegunn/fzf) should be installed to use this script. 
    * `{pacman -S / brew install / apt install} fzf`
- `bash` version should be `>= bash 4.0-alpha`. On linux it is usully true, on OS X you should update.
    * `brew upgrade ; brew install bash`

Download shell script and place it somewhere in your `PATH`, like here:
```bash
wget --no-cache -q "https://raw.githubusercontent.com/SmirnovAlexander/lnk/master/lnk" && chmod +x lnk && sudo mv lnk /usr/local/bin
```

Run `lnk init`

If you want to backup your local changes with remote repo:
1. Create new empty repo, e.g. `link-store`: https://github.com/new
2. `lnk git remote add origin remote-repo-url`
3. `lnk git push --set-upstream origin $(lnk git branch --show-current)`
4. When you want to push all changes to remote repo, run: `lnk git push`

### OS X

I recommend adding two shortcuts: for inserting links and for retrieving them.
For each of scripts do: 
- open Automator
- create a new Application
- add an action "Run AppleScript"
- save it with easy-to-retrieve names, e.g. **insert**

**lnk insert** (will automatically paste the link from the buffer)
```
tell application "Terminal"
    launch
    do script "exec lnk insert $(pbpaste)"
    activate
end tell
```

**lnk find**
```
tell application "Terminal"
    launch
    do script "exec lnk find"
    activate
end tell
```

Now you can execute program just by searching in apps by the name under which you saved the scripts.
If your terminal does not exits automatically after executing the script, change your preferences in `Terminal Preferences` -> `Settings` -> `Shell` to `close the window if the shell exited cleanly`.

### i3

To seamlessly use it in i3 add to your config (set your own bindings and terminal emulator):
```
for_window [title="^lnk$"] floating enable, resize set 80 ppt 80 ppt, move position center
bindsym $mod+Shift+u  exec --no-startup-id $TERMINAL -e lnk insert $(xclip -selection clipboard -out)
bindsym $mod+Shift+y  exec --no-startup-id $TERMINAL -e lnk find
```


## Usage

```
lnk init
    Initialize the link store.
lnk insert link-address
    Insert a new link.
lnk find [--no-browser,-n]
    Launches FZF to search for a link and open it in the browser.
    If --no-browser is specified, the link is just copied to the clipboard.
lnk edit
    Launches FZF to search for a link then prompts you to edit tags.
lnk rm
    Launches FZF to search for a link and remove it.
lnk git git-command-args...
    Execute a git command specified by git-command-args, e.g. 'lnk git push'
    or 'lnk git diff HEAD^ HEAD' to see changes from last commit or
    'lnk git reset --hard HEAD^' to undo last commit.
lnk update
    Update the script itself from source.
lnk help
    Show this text.
```

## ToDo

- lnk -> choose what to do, read 1 symbol and make that (add, rm, edit)
- add snippets support with preview
