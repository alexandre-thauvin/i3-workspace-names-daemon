# i3-workspace-names-daemon

This script dynamically updates [i3wm](https://i3wm.org/) workspace names based on the names of the windows therein. 

It also allows users to define an icon to show for a named window from the [Nerd Fonts Icon Picker](https://www.nerdfonts.com/cheat-sheet) icon list.

### tl;dr

![image](https://github.com/user-attachments/assets/059e81fa-6c6f-4f08-ba60-d21f011cfd3d)

### install

Since it's a fork from the [original repo]() I did not deploy on pip so you have to clone and install:
```
git clone git@github.com:alexandre-thauvin/i3-workspace-names-daemon.git
cd i3-workspace-names-daemon
pipx install . --force
```

I use pipx so I don't have to deal with virtual environment


##### font 

Install the [any nerd fonts](https://www.nerdfonts.com/font-downloads) font via your favourite package manager. This is necessary to show the icons.

In this fork there is no name, only icons, indeed I removed the fallback onto names, if there is no icon found for the given app, it will shows the "arch" icon:  
![image](https://github.com/user-attachments/assets/3825a6ee-3171-4224-89e4-67bd3fa5662f)


 

**NB: if the glyphs are not rendering make sure the font is installed.**


### i3 config

Add the following line to your ``~/.i3/config``.

```
exec_always --no-startup-id exec i3-workspace-names-daemon
```

If you use the ``$mod+1`` etc. shortcuts to switch workspaces then update the following so that the *switch to workspace* and *move focussed window to workspace* **shortcuts still work**. 


from 

```
bindsym $mod+1 workspace 1
bindsym $mod+Shift+1 move container to workspace 1
# etc
```

to

```
bindsym $mod+1 workspace number 1
bindsym $mod+Shift+1 move container to workspace number 1
# etc
```


### icons config
Configure what icons to show for what application-windows in the file  ``~/.i3/app-icons.json`` or ``~/.config/i3/app-icons.json`` (in JSON format). For example:

```
alexandre@thauvin: ~$ cat ~/.i3/app-icons.json
{
    "firefox": "firefox",
    "kitty": "kitty",
    "thunderbird": "thunderbird",
    "dolphin": "dolphin",
    "Signal": "signal",
    "Spotify": "spotify",
    "TelegramDesktop": "telegram",
    "code-oss": "code",
    "gitkraken": "gitkraken",
    "_no_match": "no_match"
}
```

NB: to validate your config file is formatted correctly run this command and check it doesn't  report an error

```
python3 -m json.tool  /path/to/your/app-icons.json
```

where the key is the name of the i3-window (ie. what is shown in the i3-bar when it is not configured yet) and  the value is the font-awesome icon name you want to show instead, see [picking icons](#picking-icons).

Note: the hard-coded list above is used if you don't add this icon-config file.

### matching windows

You can debug windows names with `xprop`

Windows names are detected by inspecting in the following priority
- name
- title
- instance
- class

If there is no window name available a question mark is shown instead.

Another (simpler) way for debugging window names is running this script with `-v` or `--verbose` flag, it is suggested to use a terminal emulator that supports unicode (eg. kitty or urxvt)

### unrecognised windows

If a window is not in the icon config then by default the window title will be displayed instead.

The maximum length of the displayed window title can be set with the command line argument `--max_title_length` or `-l`.

To show a specific icon in place of unrecognised windows, specify an icon for window `_no_match` in the icon config.
If you want to show only that icon (hiding the name) then use the `--no-match-not-show-name` or `-n` option.

### picking icons 

The easiest way to pick an icon is to search for one in the [Cheat Sheet](https://www.nerdfonts.com/cheat-sheet).

### windows delimiter

The window delimiter can be specified with `-d` or `--delimiter` parameter by default it is `|`.

