---
title: "How To Build The Perfect Minimal Status Bar (hyprland + waybar)"
imgUrl: "/post-images/how-to-setup-waybar-with-hyprland/thumbnail.jpg"
youtubeId: "T5Itsza4PhE"
publishedAt: "2026-05-28"
summary: "Use this guide along with my youtube video to build the perfect minimal status bar for hyprland with waybar."
---

Sign up for the Terminal Hackers Waitlist/Newsletter: [Click Here](https://terminalhackers.com)

You can find the source code for my config [here](https://github.com/josean-dev/dev-environment-files).

I'm using [Arch Linux](https://youtu.be/TS1ghG3c3xI) & [Hyprland](https://youtu.be/PEgDssV0nW0) for this.

- 🎥 [Arch Linux Installation Guide](https://youtu.be/TS1ghG3c3xI)
- 🎥 [Hyprland Setup Guide](https://youtu.be/PEgDssV0nW0)

## Install yay

Yay is a wrapper for pacman that allows you to install packages from the Arch User Repository.

You can install it with:

```bash
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

## Install ghostty

```bash
yay -S ghostty
```

## Install code editor

For Neovim use `yay -S nvim`. For VS Code use `yay -S code`.

## Install waybar

To install waybar use:

```bash
yay -S waybar
```

## Create Config Directory

Create config directory:

```bash
mkdir -p ~/.config/waybar
```

## Watch For Changes

```bash
yay -S inotify-tools
```

Then run:

```bash
while inotifywait -e close_write ~/.config/waybar; do killall -SIGUSR2 waybar; done
```

You can also create a script (something like `auto-reload.sh`), make it an executable with `chmod +x auto-reload.sh`,
add the code above and then run it.

## Install Nerd Font

For the icons to show properly you'll need to install a nerd font:

```bash
yay -S ttf-jetbrains-mono-nerd
```

## Waybar Config File

For more information on how to configure Waybar click [here](https://github.com/Alexays/Waybar/wiki/Configuration)

Open the config file with your editor of choice:

```bash
nvim ~/.config/waybar/config.jsonc
```

Then you can add the following to it:

```json
{
  "layer": "top",
  "modules-left": ["hyprland/workspaces"],
  "modules-center": ["clock"],
  "modules-right": [
    "hyprland/submap",
    "group/volume",
    "battery",
    "bluetooth",
    "network",
    "cpu",
    "memory"
  ],
  "hyprland/window": {
    "max-length": 50
  },
  "battery": {
    "interval": 60,
    "format": "{capacity}% {icon}",
    "format-icons": {
      "default": [
        "󰂎",
        "󰁺",
        "󰁻",
        "󰁼",
        "󰁽",
        "󰁾",
        "󰁿",
        "󰂀",
        "󰂁",
        "󰂂",
        "󰁹"
      ],
      "charging": [
        "󰢟",
        "󰢜",
        "󰂆",
        "󰂇",
        "󰂈",
        "󰢝",
        "󰂉",
        "󰢞",
        "󰂊",
        "󰂋",
        "󰂅"
      ]
    }
  },
  "bluetooth": {
    "format": "",
    "format-disabled": "", // an empty format will hide the module
    "format-connected": " {num_connections}",
    "tooltip-format": "{controller_alias}\t{controller_address}",
    "tooltip-format-connected": "{controller_alias}\t{controller_address}\n\n{device_enumerate}",
    "tooltip-format-enumerate-connected": "{device_alias}\t{device_address}",
    "on-click": "exec ghostty --title=bluetui -e bluetui"
  },
  "cpu": {
    "interval": 10,
    "format": " {}%",
    "max-length": 10,
    "on-click": "exec ghostty --title=btop -e btop"
  },
  "memory": {
    "interval": 30,
    "format": " {percentage}%",
    "tooltip-format": "{used:0.1f}G/{total:0.1f}G used",
    "on-click": "exec ghostty --title=htop -e htop"
  },
  "network": {
    "format": "",
    "format-wifi": "",
    "format-ethernet": "",
    "format-disconnected": "󰤭", //An empty format will hide the module.
    "tooltip-format": "{ifname} via {gwaddr} 󰊗",
    "tooltip-format-wifi": "{essid} ({signalStrength}%) ",
    "tooltip-format-ethernet": "{ifname} ",
    "tooltip-format-disconnected": "󰖪",
    "max-length": 50,
    "on-click": "exec nmrs"
  },
  "clock": {
    "format": "󰥔 {:%a, %d. %b %I:%M %p}",
    "tooltip-format": "<tt><small>{calendar}</small></tt>",
    "calendar": {
      "format": {
        "today": "<span color='#75f1fa'><b>{}</b></span>"
      }
    },
    "on-click": "exec chromium --app=https://calendar.google.com"
  },
  "pulseaudio": {
    "format": "{volume}% {icon}",
    "format-muted": "",
    "format-icons": {
      "default": ["", ""]
    },
    "scroll-step": 1,
    "ignored-sinks": ["Easy Effects Sink"],
    "on-click": "pavucontrol"
  },
  "pulseaudio/slider": {
    "min": 0,
    "max": 100,
    "orientation": "horizontal"
  },
  "group/volume": {
    "orientation": "horizontal",
    "modules": ["pulseaudio", "pulseaudio/slider"]
  }
}
```

## Waybar Styling

For more information on how to style Waybar click [here](https://github.com/Alexays/Waybar/wiki/Styling)

Open `~.config/style.css` with your editor:

```bash
nvim ~/.config/style.css
```

Then add the following to get my styles:

```css
@define-color highlight rgba(117, 241, 250, 1);
@define-color dark-9 rgba(24, 24, 27, 1);
@define-color dark-8 rgba(39, 39, 42, 1);
@define-color dark-7 rgba(63, 63, 70, 1);
@define-color dark-6 rgba(82, 82, 91, 1);
@define-color dark-5 rgba(113, 113, 122, 1);

* {
  /* `otf-font-awesome` is required to be installed for icons */
  font-family: JetBrainsMono Nerd Font Propo;
  font-size: 20px;
}

window#waybar {
  background-color: @dark-9;
  color: white;
}

#workspaces button {
  padding: 3px;
  margin: 7px 0px 7px 7px;
  background: transparent;
  border-radius: 2px;
  color: white;
}

#workspaces button.active {
  background: @highlight;
  color: black;
  font-weight: 700;
}

#submap,
#battery,
#bluetooth,
#network,
#cpu,
#memory,
#volume {
  border-radius: 8px;
  padding: 5px 8px;
  background: @dark-8;
  margin: 5px 10px 5px 0px;
}

#submap,
#workspaces button.urgent {
  background: rgba(227, 81, 73, 1);
  color: black;
}

#pulseaudio-slider {
  padding: 0px;
  margin: 0px;
  margin-left: 8px;
}

#pulseaudio-slider slider {
  all: unset;
  min-height: 0;
  min-width: 0;
  opacity: 0;
  background-image: none;
  border: none;
  box-shadow: none;
}

#pulseaudio-slider trough {
  min-height: 8px;
  min-width: 75px;
  border-radius: 5px;
  background: @dark-7;
}

#pulseaudio-slider highlight {
  min-width: 10px;
  border-radius: 5px;
  background: @highlight;
}

tooltip {
  background: @dark-9;
  border-radius: 5px;
}

tooltip label {
  color: white;
}
```

## Custom Media Player Module

### Install the necessary packages

```bash
yay -S playerctl python-setuptools zscroll
```

Create a directory for the customs scripts we'll need:

```bash
mkdir -p ~/.config/waybar/custom_modules/media
```

Navigate into it: `cd ~/.config/waybar/custom_modules/media`

Create the file for the scrolling status of the currently playing **track** and **artist**.

```bash
nvim media-now-playing.sh
```

Add the following to it:

```bash
#!/usr/bin/env bash

zscroll -l 20 \
    --delay 0.3 \
    --update-check true \
    "playerctl metadata --format '{{title}} - {{artist}}'" 2>/dev/null

wait
```

Then do the same for the bars animation:

```bash
nvim media-animation.sh
```

Add the following to it:

```bash
#!/usr/bin/env bash

# "▁", "▂", "▃", "▄", "▅", "▆", "▇", "█"

animation_frames=("▂▄▆" "▄▂▆" "▄▆▂" "▆▄▂" "▆▂▄")
while :; do
  for frame in "${animation_frames[@]}"; do
    status=$(playerctl metadata --format '{{status}}' 2>/dev/null)

    if [ "$status" == "Playing" ]; then
        echo "$frame"
    elif [ "$status" == "Paused" ]; then
        echo ""
    else
        echo ""
    fi
    sleep 0.1
  done
done
```

Then do the same to show time information for the playing media:

```bash
nvim media-time.sh
```

Add the following to it:

```bash
#!/usr/bin/env bash

playerctl metadata --format '{{duration(position)}}/{{duration(mpris:length)}}' 2>/dev/null
```

### Add the Custom Modules To The Config

Open the config file:

```bash
nvim ~/.config/waybar/config.jsonc
```

Then at the bottom add:

```json
    "group/media": {
      "orientation": "horizontal",
      "modules": [
        "custom/media-animation",
        "custom/media-now-playing",
        "custom/media-time"
      ]
    },
    "custom/media-now-playing": {
      "exec": "~/.config/waybar/custom_modules/media/media-now-playing.sh",
      "escape": true,
      "tooltip": false,
      "on-click": "playerctl play-pause",
      "on-click-middle": "playerctl previous",
      "on-click-right": "playerctl next",
    },
    "custom/media-animation": {
      "exec": "~/.config/waybar/custom_modules/media/media-animation.sh",
      "tooltip": false,
    },
    "custom/media-time": {
      "exec": "~/.config/waybar/custom_modules/media/media-time.sh",
      "escape": true,
      "interval": 1,
      "tooltip": false,
    }
```

Then at the top of the file add the new media group to the bar:

```json{3}
{
    "layer": "top",
    "modules-left": ["hyprland/workspaces", "group/media"],
    "modules-center": ["clock"],
    "modules-right": ["hyprland/submap", "group/volume", "battery", "bluetooth", "network", "cpu", "memory"],
...
```

Finally, open the styles file:

```bash
nvim ~/.config/waybar/style.css
```

And add the following at the bottom:

```css
#media {
  color: #42ff9f;
  margin: 7px 0px 7px 80px;
}

#custom-media-animation {
  font-size: 13px;
  margin-right: 10px;
}

#custom-media-now-playing {
  margin-right: 10px;
}

#custom-media-time {
  color: @dark-5;
  font-size: 14px;
}
```

## Make Sure Waybar Auto Starts With Hyprland

Open hyprland config file

```bash
nvim ~/.config/hypr/hyprland.lua
```

Then look for `hyprland.start` so you can add the following:

```bash{2-3}
hl.on("hyprland.start", function()
	hl.exec_cmd("waybar)
	hl.exec_cmd("playerctld daemon")
end)
```

This will auto start waybar and the playerctl daemon.

That's it!! 🚀
