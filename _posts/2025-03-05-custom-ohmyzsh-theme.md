---
title: "Creating a Copy-Paste Friendly Oh-My-Zsh Theme"
categories:
  - Blog
tags:
  - Programming
  - Something I Learned
  - Tech
  - Terminal
  - Oh-My-Zsh
---

My existing Terminal theme looked nice but didn't play well with copy-pasting output for sharing. Those fancy characters (`╭─` and `╰─`) in the "bira" theme I was using made for messy pastes, and right-aligned elements weren't helping either.

After combing through the [ohmyzsh theme page](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes), I identified a few promising candidates that had the some of the elements I wanted:

- A clean, minimal prompt
- Command prompt (`$`) on a new line
- Good information display (username, hostname, directory, git status)
- Time display

Rather than use an existing theme directly, I decided to create my custom theme that combines the best elements from several themes.

## Setting up the custom theme

1. First, I created a theme file in my dotfiles directory:

  ```bash
  touch ~/.dotfiles/joshua.zsh-theme
  ```

2. Claude wrote the theme based on the code for the sample themes

  ```bash
  # joshua.zsh-theme - a minimal, informative theme
  
  # Get return code - show on error
  local return_code="%(?..%{$fg[red]%}[%?]%{$reset_color%} )"
  
  # Username and hostname
  local user_host="%{$fg[cyan]%}%n%{$reset_color%} @ %{$fg[green]%}%m%{$reset_color%}"
  
  # Current directory
  local current_dir="%{$fg[yellow]%}%~%{$reset_color%}"
  
  # Git status info
  local git_info='$(git_prompt_info)'
  
  # Time display
  local time_display="%{$fg[white]%}[%*]%{$reset_color%}"
  
  # Define the prompt format
  PROMPT="
  ${user_host} in ${current_dir} ${git_info} ${time_display} ${return_code}
  %{$fg[red]%}$%{$reset_color%} "
  
  # Remove right prompt
  RPROMPT=""
  
  # Git prompt styling
  ZSH_THEME_GIT_PROMPT_PREFIX="%{$fg[blue]%}git:%{$fg[magenta]%}"
  ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%}"
  ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[red]%} ●%{$reset_color%}"
  ZSH_THEME_GIT_PROMPT_CLEAN="%{$fg[green]%} ✓%{$reset_color%}"
  ```

3. I linked the theme to oh-my-zsh's custom themes directory:

  ```bash
  ln -s ~/.dotfiles/joshua.zsh-theme ~/.oh-my-zsh/custom/themes/joshua.zsh-theme
  ```

4. Updated my `.zshrc` to use the new theme:

  ```bash
  # Changed ZSH_THEME in .zshrc
  ZSH_THEME="joshua"
  ```

5. Activated the theme with:

  ```bash
  source ~/.zshrc
  ```

The prompt now looks like this:

```plaintext
username @ hostname in ~/current/directory git:branch ✓ [time] [error_code_if_present]
$
```

Also, working with Claude made this super easy. Admittedly, this means I did not *really* learn how to make a custom theme if I didn't have internet access, but at least I'm happy.
