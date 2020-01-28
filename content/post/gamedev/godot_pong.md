---
title: "Godot Pong Demo"
thumbnailImagePosition: left
thumbnailImage: /img/thumb_guards.jpg
metaAlignment: center
coverMeta: out
date: 2020-01-27
categories:
- Gaming
tags:
- random
- programming
- gamedev
---

I've played too many video games over the years, going back to the NES as the first system I recall owning, and cutting my teeth on the SNES for most of the 90s. Then moved onto nights of Goldeneye 64 and into the *future* of PC gaming.  And for the longest time through most of college, I wanted to work on games in some way. However, turns out that is harder than it looks.  At the beginning of college, I was majoring in computer science.  But since I was a burnout dumbass at the time, I didn't do particularly well.  Why I was bad is a separate novel of its own. I can only say it would have been best for me to go to school about 5 years later, instead, if not longer.  

In the end, I got a degree in Geology, also another story.  Despite that, I work in IT/touch computers. And do quite a bit of programming now. Primarily in Python.  I consider myself decent with Python and not a professional coder by any means.  Python is incredibly nice to write code in, though.  It has clear syntax, and modules for just about anything, and its fast enough for my purposes. And over the years I've tried to dabble in making tiny games to learn more.  Each time I hit a similar barrier, which was all the boilerplate you need to do basic things. 

And those basic things are not interesting.  Take Pygame. Its a nice module with plenty of documentation.  Making a game in it is not terribly hard.  It is tedious.  With a module like Pygame (or lets say Raylib in Clang), you are given the tools to make things fly across the screen without any of the game mechanic logic.  Basic stuff like preset vector functions/classes, handling sprites and so on are sometimes there, and not easy to use.  Want to make a tilemap ? WELL learn how that works barebones and implement it. 

Now there is no problem with doing all that labor if you are into that sort of thing. I'd rather spend time making a game, not building the computer to play it on.  So after making a basic Pong clone in Python, I started to shop around for game engines/simple 2D boilerplate that would accomplish a few things:

1. Have great documentation
2. Eliminate the tedium of coding out blits/draws to screen and other tasks I consider *solved* problems from 30+ years of video games existing
3. Let me quickly focus on prototype gameplay mechanics instead of worrying about minutia.

Unity almost fits the bill. Since ~2009 Unity has taken off and I own several games on Steam made with Unity. A thorough exploration of their 2D support, and the clunky feeling I got from the editor left me feeling empty inside. To be honest, the Unity editor gives me the same annoying feeling using any JetBrains IDE.  In both cases I've exhaustingly read about how powerful the tools are and the UI turns me off so much I can't see past it.

Enter [Godot Engine](https://godotengine.org/). I stumbled across it looking around.  And read through the documentation. The first pass I ended up in some old doc archive and wasn't impressed, especially with an older tutorial that didn't make any sense.  Then I found the newer stuff, and a decent course on Udemy. Plus various Youtube videos extolling the virtues of Godot.  

After getting through the first quarter of the course, I was very impressed with the Godot editor.  Godot ships a with a cross-platform editor for making games. Within, everything is structured into scenes, and scenes are made of nodes. Nodes do various things, such as animated sprites and so on.  And they have a heirarchy so you can mix and match scenes and nodes to achieve the effects you want.  

Overall, it makes getting quick gameplay a breeze.  It took me several days of tinkering to get Pong working in Pygame.  With Godot I made a Pong clone that looks 1000x better in only a couple hours. And the editor works great in Linux Mint, my preferred OS at the moment. So I can play around without any hiccups and release things that work on Windows if I desire.

The tiny project is up on [Github](https://github.com/martysohio/godot_pong). I need to wrap up the 1 player AI functionality, maybe a few more QoL editions, but its perfectly playable right now with two people.  Except on Windows as I found a Windows size bug when I tried to launch it, so I guess I'll fix that :)