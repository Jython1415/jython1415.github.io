---
title: "How to bind a dynamic menu items to a keyboard shortcut"
categories:
  - Blog
tags:
  - Programming
  - Tech
  - MacOS
  - Something I Learned
---

1. Create a "service" in [Automator](https://en.wikipedia.org/wiki/Automator_(macOS)) that accepts no input and is linked to the app that you want to have the shortcut for. In my case, this was Finder.
2. Add a "Run AppleScript" block[^1][^2] that clicks the menu item you want. The AppleScript can dynamically select the correct menu item based on the parts of the menu item that don't change. If no part of the menu item stays that same, this may be more difficult. See the script that allowed me to select the "Copy \<filename\> as Pathname" menu item.

```AppleScript
on run {input, parameters}
    tell application "System Events"
        tell process "Finder"
            set menuList to name of every menu item of menu "Edit" of menu bar 1
            repeat with itemName in menuList
                if itemName contains "Copy" and itemName contains "as Pathname" then
                    click menu item itemName of menu "Edit" of menu bar 1
                    return
                end if
            end repeat
        end tell
    end tell
end run
```

3. Save the workflow as a "service". It should end up in `~/Library/Services`. I called mine "Copy Pathname".
4. Navigate to the "Services" tab in System Settings[^3] and set a keyboard shortcut for the service. I set mine to ⌃⌘C.

How I figured out how to do this:

1. DuckDuckGo search for "macos rebind dynamic menu item keyboard shortcut"
2. [fabián cañas - Custom Keyboard Shortcuts to Dynamic Menu Items](https://fabiancanas.com/blog/dynamic-menu-item-shortcut)

[^1]: I'm actually not sure what the correct term for this in Automator workflows is, because this is my first interaction with it.

[^2]: I asked Claude for help with this because I don't work with AppleScript often enough to feel comfortable writing a script from scratch.

[^3]: System Settings > Keyboard > Keyboard Shortcuts > Services. And in my case, my service was in "General" within the options under "Services".
