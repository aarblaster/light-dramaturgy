---
layout: single
title:  "Interview Preparation"
header:
 image: 
 teaser: "Unbecomming-0006-banner@0.5x-2.jpg"
date: 2025-11-13
categories: 
  - phd
tags:
  - Keyboard-Maestro
  - AppleScript
  - MacOS
  - interviews
---

As part of my preparation for interviewing, I wanted to make sure I had an easy way of taking digital notes.
I had previously heard from [Jason Snell](https://zeppelin.flights/@jsnell) about how he uses a script combined with Keyboard Maestro to get notes that are timestamped. 
Jason's post [about his approach can be found on Six Colors](https://sixcolors.com/post/2025/03/checking-in-on-podcast-notes/) and the full script is a [GiHub Gist.](https://gist.github.com/jasonsnell/9b95468474d6cc864d4feb6e2a5ca9cf)
While I'm doing an interview rather than a podcast, I do want audio that is easy to edit. I also wanted a way to easily find material that is useful to my research. So the note taking needed to allow me to do the following:
1. Take a free text note of whatever I liked.
2. Have minimal button presses so it was both easy to remember and also easy to repeat.
3. Be time stamped relative to the **start of the recording,** so I could find the point in the track easily.

As it turned out I didn't need to make many changes to Jason's script. I work in 24 hour time, so the date-time calculations needed to use that approach, and I wanted all my date formats to be YYYY-MM-DD.

I then put this into Script into Keyboard Maestro, for my uses I think a blank entry will work best, so I used a text box to make sure I could input anything into the note, or even leave the text as 'Enter your note here' if I just wanted to return the timestamp so I could easily find a point of interest.

![The Keyboard Maestro macro showing how I trigger the AppleScript.](/images/KM-Interview-noter-input@2x.jpeg)

The AppleScript returns a `.txt` file so they are very lightweight and easy to use. I use [BBEdit]() as my text editor of choice, so doing transformations on the text is very easy if I need to in the future. 

My version of the script can be found [as a Gist](https://gist.github.com/aarblaster/771302ee304c9eb87706f8ce38794b10). You can download a [Keyboard Maestro template here to see how it works.](https://phd.anthonyarblaster.com/files/2025-11-13_Interview-Noter-Input.kmmacros.zip)

I'm going to try it out on my first interview and see how it goes.