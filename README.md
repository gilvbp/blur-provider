# blur-provider
A Gnome extension that allows you to manually apply blur to applications, and provides an easy way for applications to request blur themselves

While Gnome supports blur since 3.36, it isn't as easy as applying that blur to any old window. The problem is, the blur also applies to the shadow around windows and looks terrible. This extension works by adding an extra actor that excludes the shadows and tracking that actor behind a target window. Then blur is applied to that, working around the issue of blur being applied to shadows. This is not an easy implementation for applications to just include, so this extension exists as a convenient way for applications to simply request blur with a property.

![screenshot of blur-provider](https://raw.githubusercontent.com/CorvetteCole/blur-provider/master/2020-09-19_16-22.png)

# Installation
## Install from extensions.gnome.org 
[<img src="https://raw.githubusercontent.com/andyholmes/gnome-shell-extensions-badge/master/get-it-on-ego.svg?sanitize=true" alt="Get it on GNOME Extensions" height="100" align="middle">][ego]
## Install from GitHub
- Clone this repository
- Change your directory to the root of this repository
- Run "make install"
- Restart shell with Alt+F2
- Activate extension

# Try it out
## Method 1
1. run xprop -f _MUTTER_HINTS 8s -set _MUTTER_HINTS blur-provider=${sigma-value}
2. click on the window you want blur applied to (it needs to have transparency for the blur to be visible)
## Method 2
1. clone https://github.com/AryToNeX/Glasstron
2. cd in to Glasstron/test
3. run ```npm install && npm test``` 

## Applications with blur-provider support built-in
- https://github.com/AryToNeX/Glasstron
- https://github.com/AryToNeX/Glasscord (available in the upcoming 1.0 release)

# Support development
https://paypal.me/CGerdemann

Will add crypto links if requested. I am quite broke right now, and money is a good motivator to keep me working on my projects while I study :)

# Integrate blur in to your application
To request your application to be blurred, you simply need to add a property to your window.

add below keypair to property _MUTTER_HINTS

keypair template: blur-provider=${sigma-value} apply blur with given sigma value
(note, ${sigma-value} is a value between 0 and 111)

You can test this with xprop: "xprop -f _MUTTER_HINTS 8s -set _MUTTER_HINTS blur-provider=${sigma-value}"

info about _MUTTER_HINTS property:
The purpose of the hints is to allow fine-tuning of the Window Manager and
Compositor behaviour on per-window basis, and is intended primarily for
hints that are plugin-specific.

The property is a list of colon-separated key=value pairs. The key names for
any plugin-specific hints must be suitably namespaced to allow for shared
use; 'mutter-' key prefix is reserved for internal use, and must not be used
by plugins.

## Behavior (simply by modifiying the \_MUTTER_HINTS property you can adjust blur instantly)
- If your \_MUTTER_HINTS is null, or no longer includes blur-provider, blur is removed
- If you do not pass a sigma value at all, or the sigma value is outside the range 0-111 (inclusive), the blur sigma will default to the extension settings sigma value set by the user
- If you pass a sigma value of 0, blur will be removed

# TODO
- after switching to a workspace and back, blur appears on top of the target actor until something is interacted with
- blur is not present behind actor in overview
- when switching focus rapidly between windows, blur can blink in front of all windows briefly
- rewrite prefs.js file, don't use the gnome-shell-extensions gettext domain, use the extension UUID

- ~a listener needs to be set to update all bluractors using default extension values when they are changed. we probably need to hold a set of pids that refer to bluractors using default values to do this.~ **FIXED**
- ~code needs to be broken up, object orientated. refactoring comes when this thing is working right~ **FIXED**
- ~blur sticks around when windows are minimized, need to listen for that~ **FIXED**
- ~blur sticks around when switching workspaces, might be able to be fixed in the same way as the above issue~ **FIXED**
- ~when actors are removed there is a delay in removing the blur actor due to the way I am iterating with a loop~ **FIXED**
- ~blur actor is not put behind the target actor properly on focus changes~ **FIXED**
- ~30ms "blink" of blur actor when focus changes (needed to keep from obscuring content, but weird workaround)~ **FIXED**
- ~blur actor still appears on top after picking a window from overview~ **FIXED**

[ego]: https://extensions.gnome.org/extension/3607/blur-provider/
