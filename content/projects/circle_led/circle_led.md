---
title: "Circle LED"
draft: false
date: 2025-01-07
description: "Playing a game on a LED strip"
showTableOfContents: true
---

For Christmas 2024 I thought I'd create a game for my kids and my nephews.
I wanted to create something where they could use the phones to play a 
game together.
As display I chose a LED strip, driven by an ESP32, in the form of
an [Atom Lite](https://shop.m5stack.com/products/atom-lite-esp32-development-kit).
These mini-computers are in fact quite powerful, being able to connect to
a WiFi, and drive a LED strip at the same time.
So I created a little game where the LED-strip is formed into a circle, and then the
users can move their figure along the LED-strip.

![Start screen](../screen_start.png)
Fig: starting the game and choosing the color

## Rules

To make the game interesting, the following applies:
- a random 'killer' token appears which moves in either direction and decreases the
  life of the user if they collide
- a user can 'jump' to avoid the 'killer' tokens, but will have to wait for some time
  before being able to jump again
- a rare 'levelup' token appears which will increase the life of the user if they collect
  it
- the speed of the users is higher than the speed of the tokens

![Countdown to play](../screen_countdown.png)
Fig: countdown with the chosen colors

## Improvements

While playing with my nephews, I upgraded the game with some new features:
- removed a bug where a player with life=0 can resurrect
- players cannot move through each other, but are blocked
- brightness on screen and on the LEDs are difficult to accomodate
- keep the display clean - I thought it's funny if there is some blur from time to time,
  but it really makes playing impossible

![Playing with killers](../screen_play_killers.png)
Fig: red and green playing, and three killers on their way

## Technical Details

### With a little help from my friends

It was interesting to see how good (or bad) Claude was in helping me create the game.
I used it mainly to create the displays in the browser, and then to make it also viable
on mobile devices.
As always, Claude was quite fast to create a 90% working interface - be it for the choice
of the colors, or for the display of the board.
Getting the 10% working was more difficult.
Some of the code was wrong in calculation of the distances, there was some wrong logic in
one place, and the tips to make it work on the mobile devices were not really useful.

In the end it's always the same: for some short code snippets, the tools work quite OK and
actually make programming faster.
For the bigger questions, or for helping with new libraries, LLMs are useless.

### Dioxus

For my last project, [Livequiz](https://livequiz.fledg.re), I created a backend with rust,
using the rocket library.
For the frontend, I used Angular, as I was already familiar to this stack.
During the project, I thought how nice it would be to have a single codebase for both the
backend and the frontend.
So I was pleasantly surprised when I discovered [Dioxus](https://github.com/DioxusLabs/dioxus),
which does exactly this:

With Dioxus, it's possible to write one codebase, which is then compiled to either the backend,
or the frontend.
It is very nice, allows for creating API endpoints which are then callable directly from the
frontend part.
All in all a very nice experience, I hope I'm able to use it in another project!

### Communication with the server

During the game I also observed that the communication between the Atom Lite and the server
was not very good.
This made the display flicker quite often.
After a lot of experiments - as long as it took me to create the original game - it turned
out that the TLS library used by the Atom Lite is very slow.
To fix this, I added a pass-through to traefik, which was difficult, until I wrote the
[Traefik 101](../traefik-101) to understand how it actually works.

#### Communication tests

As the Atom Lite needs to get 25 updates per second of the colors of its LEDs, the communication
is very important.
I tried different protocols, and only in the end realized that there are even more differences:

- A single HTTP get for every update
- Using UDP for the updates
- Opening a channel for Server Side Events (SSE), and then updating the LEDs with every new event

I tested these in my LAN and was very surprised to see that the SSEs are more regular than using
UDP.
But when I tested it on the real server, the regularity was inversed: SSE was very unreliable, and
often failed and needed to reconnect.
As it turns out, the TLS library in the Atom Lite has difficulties with a long connection.
I didn't dig into it, as the library is written in C, and precompiled.
Perhaps even an update of the library would've helped, but I was using the Arduino IDE, and didn't want
to go into all the trouble of updating it.