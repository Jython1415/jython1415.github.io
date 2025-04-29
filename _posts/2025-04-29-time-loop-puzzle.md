---
title: "Time Loop Survival Puzzle"
categories:
  - Blog
tags:
  - Puzzle
  - Logic
  - Information Theory
---

## The Scenario

The Time God has trapped you in a time loop with an unusual condition for escape! At the end of each loop (24 hours), you must submit a unique word that you've never used before. If you ever repeat a word, you die and the loop ends! You escape the time loop once you have provided the required number of unique words.

To give you a chance at survival, the Time God allows you to submit a code along with the unique word. The Time God tells you that the code will be passed along to the "you" of the next loop.

The code is the ONLY information that carries over between loops—your memory is reset each time. The "you" of the next loop must use this information to submit a word that you have not used in any previous loops.

The Time God tells you that there was no code from a previous loop. You guess this means this is the first loop.

How would you encode information to survive the required number of loops?

## Rules and Constraints

- Your memory is completely reset at the start of each new loop.
- The only information transferred between loops is your code. There is no other causal mechanism between loops.
- The code does not have to be the same between loops. You can receive code `A` and tell the Time God to relay code `B` for the next loop.
- The Time God tells you how many loops you must survive, but the Time God does not tell you how many loops you have already survived, or how many you have left.
- You do not have to worry about comfort or survival for duration of the time loop.
- You may not consult other humans, directly or indirectly, for advice.
- You have access to all human writing and knowledge (e.g. Wikipedia, online dictionaries, books, libraries, etc.). The Time God will deliver this writing/knowledge to you provided you ask for it, and it does not violate the other rules.
 - The Time God will only accept English words it considers valid.
	 - You may assume that this has significant overlap with major English dictionaries (e.g. Merriam-Webster, Oxford English, etc.) but no particular list is specified.
	 - At any point, you may ask the Time God whether it would accept a particular word.
	 - At any point, you may ask the Time God whether it considered two words to be distinct.
	 - Capitalization does not impact word uniqueness.
	- Words that are spelled differently are considered distinct: "check" and "cheque" are distinct, but not "can" (the modal auxiliary verb; "X can do Y") and "can" (noun; "soda can").

### Code Specification

There are two variations on this puzzle based on the code format.

1. Text message with the permitted characters being lowercase letters, uppercase letters, and digits.
2. Any binary sequence.

(1) is simpler and avoids the problem of decoding format, but (2) may allow for better information density. With (1), you should know that each additional character transmitted corresponds to ~6 bits of information. This should allow you to compare the efficacy of solutions for variations (1) and (2).

## Challenge Levels

### Standard Challenge

Design a strategy for each of the following scenarios.

| Code Length             | Target # of Loops |
| ----------------------- | ----------------- |
| 3 characters (16 bits)  | 10                |
| 6 characters (32 bits)  | 100               |
| 12 characters (64 bits) | 1,000             |

*The binary-based option in parentheses is meant to be similarly challenging, not precisely match the amount of information conveyed with the character-based option.*

If that was too easy, consider aiming for 10x as many loops for each of the code length restriction.

### Theoretical Challenge

At what code length do you become limited by the number of unique words in English, rather than by your information transfer capacity and strategy re-discovery reliability? Assume that the list of words that the Time God uses has more than 100,000 words but less than 1,000,000 words.

## Hints

### Questions to prompt thinking

1. How would you ensure your future self recognizes the encoding strategy?
2. What word-generation algorithm would be consistently discoverable?
3. What is the limiting factor in your strategy? Information transfer? Strategy discovery reliability? Something else?
4. How might the optimal strategy differ depending on the person?

### Sample Solution

With a budget of 12 characters and a target of 10 loops, the following solution would probably work for most people:

#### Initial Loop

- Word: `zero` or any word that is not also a number
- Code: `By1CountUp01`

#### Second Loop

You see that the hint is `By1CountUp01` and you know you only need to survive 10 loops, so you figure that you decided to count up the words for numbers: `one`, `two`, `three`, and so on.

The hint tells you what the number is and how much to count up by. Pretty straightforward.

You decide to submit the following:

- Word: `one`
- Code: `By1CountUp02`

#### Subsequent Loops

For the remainder of the loops you continue the pattern, counting up until you submit `nine` and successfully exit the time loop.

