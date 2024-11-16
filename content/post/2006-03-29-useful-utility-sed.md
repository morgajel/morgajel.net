---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Rant
- Reviews
- Utility
date: "2006-03-29T10:25:06Z"
guid: http://morgajel.net/2006/03/29/60/
id: 60
title: 'Useful Utility: sed'
url: /2006/03/29/60
views:
- "137"
---

Sed is a powerful utility for going regexes on the fly. Regular Expressions (regex) are beyond the scope of this artcle, but I’ll try to write one later. As I go, I’ll explain the regexes I use, but you really should learn about them because they’re handy as hell in many different utilities.

First up, we’ll use a simple example of a regular expression. Suppose for some reason, you want a list of the Input Device names used by Xorg, and plan on piping it into another script later on. We can look at the Xorg log and find a lot of data, but locating the Input Devices is like searching for a needle in a haystack.  
Our first step is to cat the file, then grep for the phrase “Input Device”. This provides us with

```

morgajel@FCOH1W-8TJRW31 ~/docs $ cat /var/log/Xorg.0.log \ 
| grep "Input Device"
(**) |-->Input Device "Mouse1"
(**) |-->Input Device "Keyboard1"
```

Which tells us what we need to know, but your script is more picky about the format. This is where sed comes in to play. we can tell sed to throw away everything before and including Input Device

```

morgajel@FCOH1W-8TJRW31 ~/docs $ cat /var/log/Xorg.0.log \
| grep "Input Device"|sed -e "s/.*Input Device //"
"Mouse1"
"Keyboard1"
```

Lets break this down. Sed can accept regular expressions one of two ways, either through the -e flag, as shown above or through a file using the -f flag. The advantage of using the -e flag is it’s quick and simple, whereas the advantage of using the -f flag is you can build compound regular expressions executing again one at a time, in order.

Now, what’s the regex doing? it’s saying switch(s) the first parameter(/.\*Input Device /) with the second parameter (//, or nothing) the first time you see it in every line. the “.\*” means “match every character” before “Input Device”. Note that we’re replacing “every character before Input Device” AND “Input Device” and the space after “Input Device”, and replacing it with nothing.

The good news is we’ve gotten it down to the base information we need- the bad news is it’s still surrounded by double quotes, which your script doesn’t want. How can we fix this? by using another sed to remove the quotes!

```

morgajel@FCOH1W-8TJRW31 ~/docs $ cat /var/log/Xorg.0.log \
| grep "Input Device" |   sed -e "s/.*Input Device //" |   sed -e "s/\"//g"
Mouse1
Keyboard1
```

Lets bread this one down. There are two important things to learn here- ” is a special character and needs to be escaped since we’re writing this on the command line, so it has a “\\” in front of it so the regex isn’t closed prematurely. one of the drawbacks of using the -e flag is properly escaping characters not only for regex, but for bash as well. The next thing to note is the trailing “g” at the end of the regex. the g stands for global, and says to replace all instances of the match with the replacement term (which is again, nothing).

Alright, that was the simple example, ready for the more difficult ones? Too bad, I’m tired of explaining regex. Instead I’ll focus back on what sed can do. If this is a command we’re going to be running often, it might benefit us to move the regexes to a file- I’ll call mine “myscript”, because I’m original like that:

```

morgajel@FCOH1W-8TJRW31 ~ $ cat myscript
s/.*Input Device //
s/"//g
```

Note that the double quote is not escaped. Our command to use this file goes like this:

```

cat /var/log/Xorg.0.log|grep "Input Device"|sed -f myscript
```

Much cleaner, if you ask me- but then again, you need to create a file to do so, so it’s a trade off.

Here’s an ugly ugly regex, but it has some useful things in it. Suppose we wanted a list of shells and the users who used those shells, in alphabetical order. One way to do that (although there are much better tools for this sorta thing,) is with sed.

```

 cat /etc/passwd|sed -e "s/^\([^:]*\).*:\([^:]*\)$/\2\t\t\1/"|sort
[edited for length and security]
/bin/false              squid
/bin/false              sshd
/bin/false              uucp
/bin/false              vpopmail
/bin/false              xfs
/bin/sync               sync
/sbin/halt              halt
/sbin/shutdown          shutdown
```

There are some more important things in this one as well- this is a good lesson in building expressions.

- “^” tells sed that it must start at the beginning of the line.
- “\\(” and “\\)” are bash-escaped parenthesis that tells sed “save this bit for later
- “\[” to “\]” represents a range- normally, you would see it in the context of “\[a-z\]” or “\[13579\]” matching a single lowercase a-z and/or a single odd digit, respectively.
- “^:” the carrot has a different function when it a range- it negates it, basically saying “any character but what follows”, in this case, a colon( : )
- “^\\(\[^:\]\*\\)” put together means “match zero or more characters starting at the beginning of the line until you reach a colon- oh, and save that glob for later.”
- “$” is similar to the first use of “^”- it matches the end of the line.
- “\\2” represents a glob that we set aside with the parenthesis- notice they start counting in order: “\\1” picks up the first glob (username) and \\2 picks up the second glob (shell name). Notice that most of the line was ignored- “.\*:” hiding in the center of the regex matches everything else, but since it wasn’t surrounded by parenthesis, it doesn’t get saved.
- “\\t” is last, but not least- it places a tab character between \\2 and \\1, spacing them out a bit. I put 2 in there to make it easier to read.

Argh, I said I wasn’t going to explain any more regex! Oh well, screw it. I’d also suggest checking out the info page for sed as well as the man page- running “info sed” will also give you a lot of information on sed’s regex.