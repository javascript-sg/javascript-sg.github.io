---
published: true
title: Bash on Ubuntu on Windows
layout: post
---
So Microsoft added Ubuntu into Windows which is quite nice (besides the rather mouthful name). Let's dig a little beneath the surface of things.

## What it is not

* It is not a Desktop. Duh. It says "Bash" after all. It didn't occur to me until I installed it.
* It doesn't integrate with Git for Windows or SSH agent in Windows
* It doesn't mean you can open Linux programs outside of Bash on Ubuntu on Windows.

## Where is it located in Windows?

Here: `C:\Users\<YourUserName>\AppData\Local\lxss`

For example, you usually `/etc` directory is mapped here: `C:\Users\KahWee\AppData\Local\lxss\rootfs\etc`.

I tried to manually edit the files here in Windows and it glitched out. Somehow the file disappeared from Bash on Ubuntu. 

## How do I get to my Windows drives

It's mounted in `/mnt`. In the case for my `F:` drive, it's mounted here `/mnt/f`. You have access to all your drives.

## How do I restart Bash on Ubuntu

You can restart only when you restart your Windows.

## Can I open files with Windows applications?

Yes but it doesn't really sync properly. I wanted a setup where I use Bash on Ubuntu for command line and the Windows file system to do web development. I couldn't quite get used to this workflow. 

## Does Bash on Ubuntu support tabs?

Nope, much to my disappointment. I tend to open a lot of terminal tabs and I really miss that. Managing 10 of these terminal sessions with `ALT+Tab` isn't ideal.

## Can I install zsh?

Yes! I did that. I'm not sure if it would glitch out.

## Verdict

I like the idea but the moments when I lost files scare me a little. Until my confidence regains I will not be using this with serious work too soon. I'll definitely be trying this again.