---
title: "How To Build The Perfect Minimal Status Bar (hyprland + waybar)"
imgUrl: "/post-images/how-to-setup-waybar-with-hyprland/thumbnail.jpg"
youtubeId: "T5Itsza4PhE"
publishedAt: "2026-05-28"
summary: "Use this guide along with my youtube video to build the perfect minimal status bar for hyprland with waybar."
---

You can find the source code for my config [here](https://github.com/josean-dev/dev-environment-files).

I'm using [Arch Linux](https://youtu.be/TS1ghG3c3xI) & [Hyprland](https://youtu.be/PEgDssV0nW0) for this.

🎥 Arch Linux Installation Guide: [Watch Now](https://youtu.be/TS1ghG3c3xI)
🎥 Hyprland Setup Guide: [Watch Now](https://youtu.be/PEgDssV0nW0)

## Install yay

Yay is a wrapper for pacman that allows you to install packages from the Arch User Repository.

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

## Waybar Config File

Open the config file with your editor of choice:

```bash
nvim ~/.config/waybar/config.jsonc
```

For more information on how to configure Waybar click [here](https://github.com/Alexays/Waybar/wiki/Configuration)
