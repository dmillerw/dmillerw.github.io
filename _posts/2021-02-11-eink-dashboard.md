---
published: false
---
---
published: true
---
## E-Ink Dashboard

**DISCLAIMER: I DID MOST OF THIS AT WAY-TOO-LATE-O'CLOCK**

**If you can think of a better way to do something, please let me know**

Ever since I started working from home I wanted a small unobtrusive display that I could put my calendar and todo list on. I didn't want to use a proper lit screen because my desk is bright enough already, and I wanted something I could move around and not have to keep plugged in constantly. I eventually settled on using an eInk screen, but struggled to find a solution that was cheap and didn't require a complex or difficult setup. Eventually I found a solution that at least met one of those requirements, cheap. It was a bit of a challange getting everything working, but I'm satisfied with the end result.

I did some initial research and discovered that the Barnes and Noble Nook ereader was incredibly easy to root, so initially this seemed like a perfect solution. It runs Android, has a decent battery (for an ereader) and is a complete hardware package for a low price. I picked up one of these off eBay for about $20 US including shipping.

_However..._

Upon actually receving the thing, I discovered an issue. While it does run Android, it runs Android 2.1, which was released in October of 2009! I could develop apps for it (and have, unfortunately) but it's lack of modern browser support, and not wanting to maintain my own dashboard app, was a bit discouraging.

I eventually settled on the Magic Mirror software running on a Raspberry Pi, with the Nook serving as a glorified screen. It connects via VNC to a seperate display on the Pi running an instance of Chrome. Since the Nook already has a terrible refresh rate, any sort of latency added by VNC doesn't matter.

I now offer up a guide on how I did this, if for some strange reason you want to go down this road. This guide is broken up into several steps, and I'll try to offer up standard solutions I found for my uses, as well as the round-about things I did for my super specific uses.

### Rooting the Nook

Rooting the Nook is as I said earlier, surprisingly painless. Turns out it supports booting off a Micro SD with no additional configuration, and people much smarter than me have already developed several useful utilities for this. I've included all the files required for this in a repository on my GitHub as the download links seem to fade into obscurity over time, and I had a hell of a time finding them in the first place.

**Before starting on this process, I recommend setting up the Nook completely, which involves configuring WiFi and  registering it with Barnes and Noble.**

First step is to determine the firmware version of your Nook. I've included files for both version 1.2.1 and 1.2.2, which are the latest versions ever released.

Download the appropriate file for your version, and using a tool like [Win32DiskImager](https://sourceforge.net/projects/win32diskimager/) or [Etcher](https://www.balena.io/etcher/) flash it to your Micro SD. Then turn off the Nook, insert the card, and turn it back on.

_The following images aren't mine, so the firmware version doesn't match, but the steps are the same_

You should see the NookManager loading screen, before being asked to select whether or not to enable wireless. Choose yes on this to ensure ADB is enabled.

![nook_rooting_01.jpg]({{site.baseurl}}/_posts/nook_rooting_01.jpg)
![nook_rooting_02.jpg]({{site.baseurl}}/_posts/nook_rooting_02.jpg)

From here go to Root, Root my Device, and wait an exceptionally long time...

![nook_rooting_04.jpg]({{site.baseurl}}/_posts/nook_rooting_04.jpg)
![nook_rooting_06.jpg]({{site.baseurl}}/_posts/nook_rooting_06.jpg)
![nook_rooting_07.jpg]({{site.baseurl}}/_posts/nook_rooting_07.jpg)

Eventually you should arrive at the screen below

![nook_rooting_08.jpg]({{site.baseurl}}/_posts/nook_rooting_08.jpg)

Then simply go Back, Exit, follow the prompts, and allow the Nook to reboot.

Upon rebooting you will be asked to select a Launcher to use going forward. Make sure you select Relaunch, and check the checkbox box. This will disable the Barnes and Noble launcher.

The root process also installs the Nook Touch Mod Manager apk. This will allow you some minor customization as far as keeping back/menu buttons in the status bar, and disabling the screensaver. I recommend looking through this.

### Installing VNC Client

Assuming everything went well when rooting, you should have ADB access. ADB over WiFi is enabled by default, so determine the IP address of your Nook and then on any computer with ADB installed, connect.

{% shell %}
adb connect (ip address)
{% endhighlight %}

Then download the provided apk file, and install it to the device

{% shell %}
adb install vnc.apk
{% endhighlight %}## A New Post
