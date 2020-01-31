---
layout: post
---
<img src="/images/fulls/two_desktop.jpg" class="fit image"> 

I use Arch linux in my personal rig, naturally the first statement came out of me during my first date will unironically be 'btw do you know I use Arch?'. Meme aside, there are actually many valid and sound reason why many people developed the sense of superiority towards running Arch, a quick google search will bring out thousands of page explaining various benefit that arch brings to the table. I'm not going to reiterate the reasons, but rather detailing my personal experience
on how Arch Linux is changing my life. (cliche alert)

After couple of weeks away, Last night I finally got to sit down infront my desktop and give mr.pac the long awaited `syu`, i.e. updating my Arch, the unthinkable things finally happens. After what must have been 1 year of blessed stable system, it finally break. The mouse and keyboard drop dead during the login screen. Fine, I told myself, let's investigate the system logs by going to TTY through `ctrl` + `alt` + [Num], and immediately realized how stupid I am. So, we've officially came
to the dead end. The younger me would definitely shit my pants, but luckily I'm not the person I used to be anymore. I've ascended you see.

I'll still need to get access to the log one way or the another, so the correct step will be boot into arch iso, mount the drive that contain the OS and inspect the log. Since our mouse and keyboard does not response in the login page, it must have something to do with the X server (you'll need to install and configure X server during arch installation), and sure enough X server log detail the process of disabling the mouse and keyboard. But I know for certain X server couldn't be the
culprit since the config never change.

The next very important log to inspect is the journalctl (or systemd units). To get the actual log, I disable the login page (display manager) auto start, so that I'll not launch X server when it's booted up. After restarting and it boot straight to TTY with keyboard access, we can then inspect which units is failing. I discover that `systemd-modules-load` failed, and it usually caused by invalid kernel installation process, and that exact time the dawn of realization came upon
me. Couple of weeks ago I've meddled with the /etc/fstab (responsible to auto mount the drive), and accidentally use `>` instead of `>>` in my terminal, which will removed the old fstab content, and in the process of recreating the fstab, I forgot to add the poor /boot partition. This will cause the kernel image to not install in the correct partition when updating the linux kernel, which essentially will cause a kernel mismatch. I can verify by running `uname -a` to see
which kernel image are the system using.

The story does seems like it doesn't worth a blog post, but I'm damn proud of myself. I'll never be able to properly debug the problem if I'm sticking to the boomer distro (e.g. ubuntu). That's where I think Arch truly shine, all the knowledge of configuring X server, fstab, bootloader came into play nicely to save my ass. If your work required linux, do yourself a favour and head on to arch wiki installation guide and start your journey. Alternatives like *gentoo* and *LFS* also work
if you don't have a social life.
