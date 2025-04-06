---
title: "How to copy projects for Claude"
categories:
  - Blog
tags:
  - Programming
  - Tech
  - Python
  - MacOS
  - Claude
  - AI
---

*This is meant to be an explanation of my workflow for providing Claude with context about specific Python packages so that I can ask questions about it. Note that this workflow is best suited for smaller packages because not all packages will fit into Claude's context window. I wrote it for my brother.*

## The Toolkit

The main thing I want to get across is that using `files-to-prompt` is a great way to quickly grab context about a bunch of files in a folder and provide it to an LLM. I also pair this with `ttok` because that tells you the number of tokens that that's going to take when it's provided to the chatbot, and there are limits to how much they can actually read.

Usually, I'll make a query with `files-to-prompt` and see how many tokens that takes before I pipe it into `pbcopy` instead, so that I get it copied, and then I can take it over to Claude and paste it into its context.

## Basic File Selection with `files-to-prompt`

If I need to be more granular about what files I'm selecting, I have two courses of action. The first is to use the built-in file selection mechanisms that come with `files-to-prompt`.

Typically, if you want to include all the files, you might do something like:

```bash
files-to-prompt .
files-to-prompt --include-hidden .
```

The first one selects all the non-hidden files in the current directory. The second also includes the hidden files.

If I'm working with Python files specifically and I just want to select the Python files in a directory, then I would specify the extension that I want to select for:

```bash
files-to-prompt -e py .
```

## A Complete Workflow Example

Let me break down step-by-step how I might get access to and then get a copy-paste version of a Python package that I need to ask Claude questions about:

```bash
$ git clone https://github.com/Westwood-Robotics/PyBEAR.git
Cloning into 'PyBEAR'...
remote: Enumerating objects: 244, done.
remote: Counting objects: 100% (48/48), done.
remote: Compressing objects: 100% (44/44), done.
remote: Total 244 (delta 10), reused 18 (delta 2), pack-reused 196 (from 1)
Receiving objects: 100% (244/244), 66.22 KiB | 105.00 KiB/s, done.
Resolving deltas: 100% (137/137), done.

$ files-to-prompt -e py PyBEAR | ttok
19885

$ files-to-prompt -e py PyBEAR | pbcopy
```

In the first step, I clone the project I'm interested in. In the next step, I use `files-to-prompt` and select for the Python file extension and pipe that into `ttok` to see how many tokens it would take up in the context window. 20k is less than Claude's limit, so I know that it's safe to move forward with this. The final command, `pbcopy`, copies the output so that I can paste it later.

## Advanced File Selection with `fd`

There are a number of tools that you can use to improve this workflow. For example, instead of using the file selection tools provided by `files-to-prompt`, which are pretty rudimentary, you can use a slightly more sophisticated file selection tool called `fd`. The output from `fd` can be piped into `files-to-prompt` because `files-to-prompt` is designed to be able to do that.

Here is a working example:

```bash
$ fd --type file "\.py" PyBEAR
PyBEAR/examples/set_position.py
PyBEAR/examples/spring2damper.py
PyBEAR/pybear/CONTROL_TABLE.py
PyBEAR/pybear/CRC.py
PyBEAR/pybear/Manager.py
PyBEAR/pybear/Packet.py
PyBEAR/pybear/TIMING_TABLE.py
PyBEAR/pybear/__init__.py
PyBEAR/pybear/__main__.py
PyBEAR/setup.py
PyBEAR/useful_tools/ID_Setup.py
PyBEAR/useful_tools/Search_N_Change_ID.py
PyBEAR/useful_tools/bulk_comm_freq_test.py
PyBEAR/useful_tools/ping.py

$ fd --type file "\.py" PyBEAR | files-to-prompt | ttok
19885

$ fd --type file "\.py" PyBEAR | files-to-prompt | pbcopy
```

In this example, I use `fd` so that I can see the files I'm selecting before I go and read their contents with `files-to-prompt`, and the advantage of this is that I can scan the list and see if there's something that I want to exclude beforehand. This example doesn't showcase some of the more advanced selection tools that `fd` has available to it, but I'm not going to get into that in this write-up.

## Installing the Tools

I'm going to cover how to do this on the Mac, and I think you can figure out with assistance from Claude how to adapt this for other platforms as necessary.

The core tools in this workflow, `files-to-prompt` and `ttok`, are distributed as Python packages and can be installed with the Python package manager (and much more), `uv`.

To install `uv`, look up how to install `uv` because I don't want to include instructions in here that might become outdated.

Once you have `uv` installed, you can install the tools with `uv tool install <toolname>`:

```bash
uv tool install files-to-prompt
uv tool install ttok
```

`pbcopy` is what you can use on Mac (and I think on Unix more generally) to take something and copy it. I'm not sure what the equivalent is on Windows.

`fd` is an optional portion of this guide, so I won't be including download instructions. In my setup, I install it with homebrew (command `brew`), the package manager for macOS (and Linux).

## Example

[Claude chat where I ask questions about PyBEAR](https://claude.ai/share/5efbdc62-ab54-4e8d-b117-5f86b0d7c509)

*If you encounter any issues with this workflow, I recommend troubleshooting with Claude.*
