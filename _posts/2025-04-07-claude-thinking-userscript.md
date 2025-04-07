---
title: "Auto-Enabling Extending Thinking Mode for Claude"
categories:
  - Blog
tags:
  - Programming
  - Something I Learned
  - JavaScript
  - Claude
  - Safari
  - Userscript
---

# How I Auto-Enable Extended Thinking Mode for Claude

I got tired of manually enabling extended thinking mode for Claude. I want it on for most of my conversations, but since the default is that it's off, I find myself having to click that toggle constantly. It's even worse when I start a conversation and realize that I wanted it on from the start, and now I have to go back and create a new chat with the same prompt.

I would much prefer if it was on by default and I would have to manually turn it off when I wanted it off.

To solve this issue, I first asked Claude if it could search online to see if someone has found a way to make sure that extended thinking is on by default. It had some trouble searching, so I decided to ask if they could find some way to automate turning it on so that I didn't have to bother with it.

Over the course of about five minutes, we worked on a solution. It happens to be the first custom userscript that I've used in any browser.

* [Gist with the userscript](https://gist.github.com/Jython1415/ceb3aa8ac75d48799f036bd610a41da8)
* [Claude chat where we worked on the userscript](https://claude.ai/share/f194dd57-33e1-46f3-b204-33d6298ad50d)

I now have a custom userscript using the open-source ["Userscripts"](https://github.com/quoid/userscripts) extension for Safari that automatically enables extended thinking when I open up a Claude webpage. The script works by detecting when I'm in a Claude conversation, clicking the settings button, finding the extended thinking toggle, and enabling it if it's not already on.

It doesn't work on the macOS desktop app, but it doesn't really have to. Most of my Claude usage is through the webpage anyway.

