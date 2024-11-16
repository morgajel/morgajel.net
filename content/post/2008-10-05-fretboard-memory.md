---
author: Jesse Morgan
categories:
- Uncategorized
date: "2008-10-05T13:24:09Z"
guid: http://morgajel.net/?p=331
id: 331
title: Fretboard Memory
url: /2008/10/05/331
views:
- "63"
---

[![fullfretboard](http://farm4.static.flickr.com/3033/2915799088_f15633775e.jpg)](http://www.flickr.com/photos/7626534@N03/2915799088/ "fullfretboard by morgajel, on Flickr")

So I’d like to create a simple memory game in Flex, however my brain is stuck in music/writing mode rather than code mode, so I’m going to ask some people I know who are learning flex if they’d like to help. It will start off with 2 modes and a couple options- relatively simple, but in the future might move on to more modes (once I read their code, I may be able to tweak and append on myself).

### Goal

The goal is to help teach myself and other musicians to better understand the fretboard and recognize the relationship of the strings, chords, intervals and scales on it.

### Premise

- **Mode 1: Guess this note:** A note will light up on the fretboard and ask “what note is this?” Users Will fill in the blank. All halfsteps will be presumed sharp for now. future difficulties include “sharp, natural or flat?” Options include fill in the blank and multiple choice answers.
- **Mode 2: Find the note:** a user will be given a note and asked to find a note on the board. difficulties include: entire fretboard (default), per string, per box, per random selection of notes

From here down is wishful future thinking: - **Mode 3:What’s the Interval?** a user will be given 2 notes and asked to select which interval it is. Options include the ability to limit intervals to certain types. difficulty includes per string, per box, per fretboard
- **Mode 4: Chordal stuff** could be a lot of things, but it gets complex fast
- **Mode 5: Recognize the tone** Will record strings and play, and user will try to recongize by ear
- **Mode 6: Tune my string** trains tuning a guitar by ear- raising or lowering the pitch of a tone until it sounds like the note on the fretboard.
- **Mode 7: Show my chord** User select a type of chord (mdim7), then select a note on the fretboard and it shows how that chord is created from that root note (finds the min.3rd, then 5th, then dim 7th)
- **Mode 8: Recognize a scale/mode**What’s being played? C Major?, G pentatonic? options include per string, per box, or per arpeggio.

### Implementation thoughts

Since I don’t know much about flex/actionscript, I’m not sure how much of this would actually be applicable. One thing I’ve been thinking about is what type of data structures would be needed- it would be simple enough to represent the fretboard with a matrix of notes, presuming you only allow sharps.  
However if it gets much more advanced, you’re gonna need to have some notes have multiple values (the note above G could be either G# or Ab). So you could define a Note Datatype and overload the equivalency type functions (&amp;lt’,&gt; ,=). You’d also need to define a Scale since it’s a circular array. Then again you may want to represent each individual String as an object and represent it’s relationship with all of the above as well as the other strings… Lots of directions to run.

### Terminology 

- **Box:** A section of the fretboard 4 or 5 frets wide and spanning all 6 strings. Usually used in the context of a mode
- **Mode:** Not to be confused with game modes, a mode in music terminology is a segment of a scale starting at a given note. The Pentatonic scale only has 5 notes, and therefore only has 5 possible modes.
- **Interval:** an Interval a series of two notes- point X to point y. There are 12 possible intervals, corresponding with the 12 notes in a chromatic scale:  
    From Root note: 
    - Minor 2nd
    - Major 2nd
    - Minor 3rd
    - Major 3rd
    - Perfect 4th
    - Tritone
    - Perfect 5th
    - Minor 6th
    - Major 6th
    - Minor 7th
    - Major 7th

(let me know if I need to define any more.)

Anyone have any thoughts or other input?