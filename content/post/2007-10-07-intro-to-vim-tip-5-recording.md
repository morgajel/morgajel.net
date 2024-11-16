---
author: Jesse Morgan
categories:
- Open Source
- Utility
- Vim
date: "2007-10-07T01:47:37Z"
guid: http://morgajel.net/2007/10/07/220/
id: 220
title: 'Intro to Vim Tip #5 (Recording)'
url: /2007/10/07/220
views:
- "78"
---

Search and replace is a great feature in most text editors, but what happens when you want to do more? Vim has a solution- recording macros. Suppose you have the following output from some ancient program that needs to be tweaked:

`<br></br>X1222 22323 2A22 3303 0000 3334esss test 123<br></br>X2222 22353 2A22 3303 0001 3334esss tacd 456<br></br>X3222 22383 2A22 3303 0010 3334esss fals 789<br></br>X4222 22393 2A22 3303 0011 3334esss true 012<br></br>`

It is doesn’t really matter what it is, this example is somewhat contrived. Suppose you needed to make the following changes for each line that starts with X:  
\* change the ID from X\_222 to Y\_223  
\* reverse the 4th and 5th fields  
\* copy the second character from the beginning and insert it before the last character of the line

If it were only 4 lines, you could handle this yourself, but it would be very tedious. Suppose rather than 4 lines, you had 400- it’d be much easier to automate it. The best way to take care of it would be with a macro:  
`<br></br>[esc]qa<br></br>/^X[enter] i[delete]Y[right][right][right][delete]3<br></br>[esc]wwwdww[left]p<br></br>0[right]d[ctrl+v]y$[shift+p]<br></br>q<br></br>`  
That right there is a MESS, but gets the job done- it’s not something you want to repeat for fear of a typo. Notice that the first characters you typed were qa: ‘q’ starts recording, and ‘a’ is the slot we’re using to store the macro. From here we record how \*we\* would make the changes, making sure to keep our keystrokes to a minimum. When we’re done, we stop the macro by pressing ‘q’ again.

To run our macro on the next line, press ‘@a’ to run the newly created ‘a’ macro- it should find the next line that starts with an X (notice the /^X in our first command) and run those commands to massage our text.

Remember how we were talking about 400 lines like this? Even at 2 characters each, that’s 800 characters to type which is still annoying. Here’s where the magic comes in- you can record macros of macros:  
`<br></br>qb<br></br>@a@a@a@a@a@a@a@a@a@a<br></br>q<br></br>`  
Now each time you run @b, you’ll run the a macro 10 times. A more efficient way to handle this would be to use  
`<br></br>@a<br></br>398@@<br></br>`  
the first one was done manually to record the macro, the second to play the macro, and the third to say run it 398 more times.

And there you go- a quick tour of recording macros. I’m sure there’s much more than what I’ve shown, but that’s enough to keep you busy.