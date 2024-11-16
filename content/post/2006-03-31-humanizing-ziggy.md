---
author: Jesse Morgan
categories:
- Hobbies
- IRC
- Open Source
- Ziggy
date: "2006-03-31T16:37:26Z"
guid: http://morgajel.net/2006/03/31/120/
id: 120
title: Humanizing Ziggy
url: /2006/03/31/120
views:
- "66"
---

For those that are not familiar with Ziggy, he is a character based on a DnD character from a few years back and has taken on a life of his own. He’s made an appearance in my Willis module for Neverwinter Nights (based off the same campaign), a book that I’m writing, several sketches as to what he looks like, and an IRC bot written in perl.

The IRC bot is what has really shaped his personality, and he’s become pretty much another member of the group. The first iteration of ziggy had noticable issues- mainly the instantaneous full sentence reponses.

Fortunately, the initial workaround for this problem was easy- Ziggy is based off the PoCo-IRC module, and there’s an ability to delay() calls to session hooks. This has made ziggy much more realistic and given me the ability to make him… bizarre.

As I’ve added more and more functionality to Ziggy, I’ve had successes and failures. The current code rewrite I’m working on is massive, but well worth the effort. One of the big pushes I’m working on is modularizing his code; that in combination with the personality xml file would allow others to easily create bots and pick and choose features to add. This would also allow me to create an API for modules and let others develop for ziggy.

The problem I’m running into is the code I wrote in the previous version of ziggy is dependent on features available in the PoCo-IRC scripts, but not the plugin class. Lets look at a sample case:

Bar\_brawl.pm  
Bar\_brawl is triggered when someone performs a ctcp action that involves punching ziggy.

\* morgajel punches ziggy in the liver  
1-3 second delay  
\[ziggy\] BAR BRAAAWL!!!!!  
1-5 second delay  
\*ziggy hits morgajel with a chair

repeat the last two sets 4-8 times, changing the delay, the target, and it’s an action or a battle cry.

In the original version, I created a bar\_brawl handler which recursively called itself and decremented a variable. Each round of bar brawl randomly selected a “brawl\_option” from the personality xml file, immedately performed a post(‘privmsg’) or action to the channel, then performed a delay() on itself for a random 1-5 seconds.

The result was exactly what I was looking for, except that it was a big mass of ugly code in the middle of the program. This, if anything, should be moved to a module. The problem then became, “how do I declare the delays?”

BinGOs, the guy who maintains PoCo-IRC is working on a delay for plugins, which will almost fix my problem- he says that it’ll only work for main session hooks like privmsg, kick, etc- which means no plugin-level delay on a plugin-level bar\_brawl hook.

So where should I go from here?  
**UPDATE**

So it looks like the fix was using an internal function in $irc called \_send\_events  
with this I could send a ‘say’ event, and had say recursively call itself if an extra parameter was given to it (a delay in seconds). on top of this, delay\_add fixed the disappearing delays issue. Moral of the story: it’s all working now.