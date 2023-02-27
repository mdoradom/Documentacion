---
title: SDL Documentation
author: mario
date: 2022-02-27 11:56:00 +0800
categories: [SDL]
tags: [doc, sdl, C, C++]
render_with_liquid: false
---

## Inputs in SDL 

We register evenets: 

- **KEY_DOWN** 
- **KEY_REPEAT** (SDL doesn't give you this automatically) 
- **KEY_UP** 

That works for keyboard, mouse and buttons.

We can request the list of events to SDL anytime. This will produce an ordered list of events following the timeline. Normally this list is pulled at the start of every frame. The core of the process starts with SDL_PollEvent()