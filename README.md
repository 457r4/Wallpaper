# Wallpaper

Get NASA's Astronomy Picture of the Day (APOD) every day and display it as your phone's lock-screen wallpaper. This script was made in and for Termux, that is, it is meant to work on an Android device.

## Usage
### Update
```zsh
$ wallpaper -u
```
Downloads and "formats" (rotates) the APOD.
### Set
```zsh
$ wallpaper -s
```
Sets the APOD as your lock-screen wallpaper.
### Print
```zsh
$ wallpaper -p
```
Prints the explanation of the APOD in a newspaper style.
### Display
```zsh
$ wallpaper -d
```
Opens the APOD in whichever app you prefer.


## Requirements
In order for this script to work properly you'll need to install `zsh`, `cronie`, `termux-api` and `termux-services`.
```zsh
$ pacman -S zsh cronie termux-api termux-services
$ ln -s $PREFIX/bin/zsh ~/.termux/shell
```
Termux API needs the app with the same name which you can download via F-Droid, for `termux-services` you can check how to set it up on the wiki.

## Setup
For `wallpaper` to work as intended getting and setting the APOD as your lock-screen wallpaper everyday you'll need to setup a routine with `crond` for the script to be executed in a schedule, you can do that with 
```zsh
$ crontab -e
```
That will open your editor where you can schedule the daily execution of the script, if you don't know the syntax you can set it as follows
```vim
mm hh * * * /path/to/wallpaper
```
It is important to write the whole route to wallpaper for crond to find it, finally to start running it you can just do 
```zsh
$ sv-enable crond
$ sv up crond
```

Now that you've got crond running it already has scheduled the execution of the script, however it might just not work if you close Termux... that's unfortunate, you might already have removed the battery optimization for the app but it will also need to remain awake in the background for which you can use the command
```zsh
$ termux-wake-lock
```
But even then, if you restart your phone you'll need to get `crond` running again, for that I recommend installing Termux Boot (from F-Droid as well) and creating a script in the directory `~/.termux/boot/` with the following content
```zsh
#! /data/data/com.termux/files/usr/bin/zsh

termux-wake-lock
sv up crond
```
With this the script will run in the background completely autonomously and won't need your intervention anymore.


## Configuration
You can set `wallpaper` to change the time at which it displays the newly downloaded image. APOD is updated on the source around 00:30 UTC-4 so that's the least you should schedule it in `crontab` for your local time, but the image won't be set imediately unless you want to, to change the time at which the new image will be displayed simply edit the variable `OFFSET` for the time in seconds of the day at your local time you want to start to see the new image.

You can change the color of the newspaper type explanation by editing `TITLE_STYLE` with the variables of style you see above :) 
