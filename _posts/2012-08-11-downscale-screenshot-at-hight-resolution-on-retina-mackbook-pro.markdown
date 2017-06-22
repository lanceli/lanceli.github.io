---
layout: post
comments: true
title:  "Downscale Screenshot at high resolution on retina macbook pro or retina 5K iMac"
date:   2012-08-11 09:39:22 +0800
categories: MacOS
---
**UPDATE: Use applescript copy small screenshot to clipboard**

As we know retina macbook pro or retina 5K iMac have the world’s highest-resolution notebook display. In case of you’re running at the 1440×900@mbp, 2560×1440@iMac effective resolution, using the Cmd+Shift+3/4 native commands would create a double resolution screenshot, the full screen screenshot dimension is 2880×1800@mbp, 5120×2880@iMac, but sometimes we just need a small one.

Here is my solution:

In OS X, there is a command named _screencapture_, for example run _screencapture -i screenshot.png_ in Terminal, it’s almost the same as Cmd+Shift+4\. so we can make a service to the services menu of each application and assign a shortcut. If you assign Cmd+Shift+4 to instead the Native one, then everything looks just like native.

### Step 1: Make a service for screenshot

Or you can download this service from [github](https://web.archive.org/web/20141209200313/https://github.com/lanceli/downscale-retina-screenshot-service), just go to step 2 after install it. You are welcome to contribute.

- Start Applications » Automator
- Select “Service” for the template of the new Automator workflow
- In the top of the right pane, select “Service receives no input in any application”
- Drag action “Run Shell Script” from the left pane into the workflow on the right pane
- Leave Shell at its default “/bin/bash”, and replace the default command cat with the following

```
# the path where screenshots to save
# if you want to save them to your desktop, SS_PATH should be "/Users/YOURNAME/Desktop"
SS_PATH="/tmp"

# a variable of unix timestamp for screenshot file name
NOW=$(date +%s)

# image format to create, default is png (other options include pdf, jpg, tiff and other formats)
SS_TYPE=PNG

# name of screenshot
SS_NAME=screenshot$NOW

# full path of screenshot file
SS_1X=$SS_PATH/$SS_NAME@1X.$SS_TYPE
SS_2X=$SS_PATH/$SS_NAME@2X.$SS_TYPE

# execute screen capture command and save to $SS_2X
screencapture -i -r -t $SS_TYPE $SS_2X

# check if screenshot is existing
if [ -f $SS_2X ]; then

    # get the 50% width of screenshot by sips
    WIDTH=$(($(sips -g pixelWidth $SS_2X | cut -s -d ':' -f 2 | cut -c 2-)/2))

    # scale down by sips
    sips --resampleWidth $WIDTH $SS_2X --out $SS_1X
    
    # copy small one to clipboard by applescript
    # if you hold control key when do capture, causes screen shot 2X to go to clipboard
    osascript -e 'set the clipboard to (read (POSIX file "'$SS_1X'") as «class PNGf»)'
	
	rm -rf $SS_PATH/screenshot$NOW@2X.$SS_TYPE
fi
rm -rf $SS_PATH/screenshot$NOW@1X.$SS_TYPE
```





- click the Run button to test
- Hit Cmd-S to save. The name you type will be the name in the Services menu. The workflow will be saved in ~/Library/Services.

### Step 2: To assign a keyboard shortcut:

- Open System Preferences » Keyboard » pane Keyboard Shortcuts
- Select “Services” in the left pane
- Scroll down to General in the right pane
- Double-click to the right of the Automator workflow you just created
- Press the keys you want to use, and switch panes to ensure the new shortcut is saved

I have assigned Cmd+Shift+4 to it, and assigned Cmd+Shift+5 to the native screenshot command.