---
author: Jesse Morgan
categories:
- Linux
- Open Source
- Utility
date: "2006-03-02T15:34:32Z"
guid: http://morgajel.net/?p=98
id: 98
title: 'Useful Utility: chmod'
url: /2006/03/02/98
views:
- "37"
---

Chmod is a utility used for changing permissions. It is fairly well known, and doesn’t have a lot of obscure flags, which makes it an odd choice for this series. I’m including it because it seems like the most logical way to touch on linux file permissions, which can be the bane of new linux user. Let me cover permissions first, then we’ll move on to chmod.

### Simple Permissions

(I’m only touching on “simple” permissions because they’re difficult enough to grasp without throwing in super user and stickey bits, or attributes like immutable.)

Every file has three types of simple permissions: Read, Write and Execute. Each file also has three types of users: the (u)ser, (g)roup, and (o)thers. Each of these groups has each of these permissions, resulting in a total of 9 simple permissions (there’s also a 10th one that determines what type of file you’re dealing with at the front of the line). When you type “ls -l filename” at the command line, you can see these permissions:

```

morgajel@FCOH1W-8TJRW31 ~/docs $ ls -l foo.pl
-rwxr-xr--  1 morgajel users 984 Feb 27 13:01 foo.pl
```

The file permissions are the first 10 characters.

```

  file type     user     group     others
       -             rwx        r-x          r--
```

 Permissions will always be listed in this order- user, group,others. They’ll also be in the order or read,write,execute for each of those groups. A permission is active when the corresponding character is shown, and inactive when a dash appears. The above permissions show this is a regular file, and the user (morgajel) has read, write and execute permissions. The group that it belongs to (users) have read and execute permissions. Everyone else (others) have only read permissions.

Permissions for directories are a bit different than regular files- they control who has access to view and write to their contents. This is a bit out of the scope of what I intended to write (and I’m a tiny bit fuzy on the edges myself), so for the sake of simplicity, I won’t touch on them (for now). I encourage you to explain it if you have a firm grasp yourself and can provide sources.

So all your files have these wonderful permissions, but how do we change them? Well, there are two ways- symbolic and numeric. The simplest is with symbolic, but as you continue you’ll learn to use both.

###  Symbolic Permissions

Suppose in the file above, we want to block other users from reading the file. to do this, we need to remove the read permission from (o)thers on foo.pl

```

morgajel@FCOH1W-8TJRW31 ~/docs $ chmod o-r foo.pl
morgajel@FCOH1W-8TJRW31 ~/docs $ ls -l foo.pl
-rwxr-x---  1 morgajel users 984 Feb 27 13:01 foo.pl
```

we use the symbols o-r to signify “subtract read from others.” Suppose we also want to remove read and execute permissions from the group that this file is in?

```

morgajel@FCOH1W-8TJRW31 ~/docs $ chmod g-rx foo.pl
morgajel@FCOH1W-8TJRW31 ~/docs $ ls -l foo.pl
-rwx------  1 morgajel users 984 Feb 27 13:01 foo.pl
```

See, we can stack them up like that. We can stack user groups as well: go+r, guo+rwx, and gu-rx are all acceptable. It’s worth noting that you can also use the equals sign (example: oga=rx) to explicitly set permissions. On top of this, rather than writing “ugo”, you can use “a”, which stands for “all”.

```

morgajel@FCOH1W-8TJRW31 ~/docs $ ls -l foo.pl
----------  1 morgajel users 984 Feb 27 13:01 foo.pl
morgajel@FCOH1W-8TJRW31 ~/docs $ chmod a+rx foo.pl
morgajel@FCOH1W-8TJRW31 ~/docs $ ls -l foo.pl
-r-xr-xr-x  1 morgajel users 984 Feb 27 13:01 foo.pl
```

The problem you run in to during all of this is setting complex permissions. Suppose you want to change from -r-xr-xr-x to -rw-r-x-r– ? I don’t know of a way to do it symbolically (although there might be and I just don’t know it).

### Numeric Permissions

Think about those 9 original permissions again:

```

-rwxr-xr--  1 morgajel users 984 Feb 27 13:01 foo.pl
```

Ignore the file type in the beginning of that line, you have 3 groups of 3.

```

 rwx      r-x      r--
```

Now, the fact that they use the letters r, w and x are just visual cues- Instead of letters, imagine them being placeholders in contrast to nothing- permissions are either on, or off.

```

 ###      #_#      #__
```

Now we’re going to make a bit of a logical jump. What we end up with are 3 three-digit binary numbers; all the digists are either On (#) or Off (\_). The highest you can count in binary with three digits is 7(if you start at 0). The bad news is you/re about to learn to count to 7 in binary. The good news is it’s very easy.

```

0   ___     000
1   __#     001
2   _#_     010
3   _##     011
4   #__     100
5   #_#     101
6   ##_     110
7   ###     111
```

Now lets look at those permissions again and convert them from symbols to placeholders to binary to regular digits:

```

 rwx      r-x      r--
 ###      #_#      #__
 111      101      100
  7        5        4
```

Now lets try setting a file with now permissions to have the permissions above.

```

morgajel@FCOH1W-8TJRW31 ~/docs $ ls -l foo.pl
----------  1 morgajel users 984 Feb 27 13:01 foo.pl
morgajel@FCOH1W-8TJRW31 ~/docs $ chmod 754 foo.pl
morgajel@FCOH1W-8TJRW31 ~/docs $ ls -l foo.pl
-rwxr-xr--  1 morgajel users 984 Feb 27 13:01 foo.pl
```

The most common numbers you’ll use are the following:

 7 most permissions you can give: read, write and execute  
 7 read and write, used on personal documents  
 5 read an execute, but no write access- often given to scripts so other users can run them  
 4 read only, used on system documents  
 0 nothing: you get no access

###  Using Chmod

We’ve seen the basis of how chmod works, but what fun things can we do with it? Well, there’s the -R flag which changes permissions recursively. The -c flag lists the changes that are actually made for each file:

```

morgajel@FCOH1W-8TJRW31 ~/docs $ chmod 750 -c foo.pl
mode of `foo.pl' changed to 0750 (rwxr-x---)
```

The -v flag lists ALL changes that are *attempted* for each file. The -f flag hides all mesages, which can also be done with the more obvious –silent or –quiet.

It should be noted that Users can only set the permissions they have access to; for example, Bob can change the permissions on files that Bob owns, but not on files that Joe owns. Root can of course change any permissions and even write to unwritable files.

An interesting note is that a file set to 007 will allow others to read, write and execute, but not the user or group that it belongs to- in other words, permissions don’t stack.

### The 10th Permission

Remember that leading – on our permissions? I referred to it as the file type. Well, here’s a more complete explination. There are several types of files:

- – = regular file
- d = directory
- l = symbolic link
- s = socket
- p = named pipe
- c = character (unbuffered) device file special
- b = block (buffered) device file special

Regular files and directories are the most common types you’ll run into, followed by symbolic. These are all beyond the scope of this document (yet again), but I will mention this- Symbolic link files will always have their permissions set to 777 (rwxrwxrwx).

### Advanced Permissions

Remember how I said the r, w and x in “rwxr-xr–” were just placeholders, and didn’t hold any meaning? well, that was partly true. there are 3 special “optional” flags that can be used. The first one is the Stickey bit, often showing up as a T in the (o)ther’s execute space. This tells the kernel to keep a copy of this executable in swap space so it’ll run without incurring the delay of loading it from the harddrive the next time around. It’s rare to see it, but when you do, it’ll look like this:

```

morgajel@FCOH1W-8TJRW31 ~/docs $ ls -l foo.pl
---------T  1 morgajel users 984 Feb 27 13:01 foo.pl
```

Next up are the setuserid (SETUID) and setgroupid (SETGID) bits. There are actually two- one for groups and one for users. When the group SETGID bit is set, anyone who runs this program will execute it as if they’re a member of whatever group the file belongs to. If the user SETUID bit is set, the program will run as if it’s being run by the user it belongs to.

```

morgajel@FCOH1W-8TJRW31 ~/docs $ ls -l myprogram*
---x--Sr-x  1 morgajel webdevs 984 Feb 27 13:01 myprogram1
---S--xr-x  1 morgajel webdevs 984 Feb 27 13:01 myprogram2
```

For example, if bob was to execute the myprogram1 file, it would execute with webdevs’ group permissions, perhaps granting it access to files bob otherwise might not be able to get to. If he were to run myprogram2, it would execute as if morgajel was running it. These bits can be useful with applications like cdrecord, where the program needs low level access to hardware. They do pose a security risk however- an insecure program can result in a malicious user getting permission to files they otherwise wouldn’t be able to.

Be wary when using the SETUID and SETGID bits, and use them sparingly.

I think that covers a good majority of what you need to know to use chmod. Later on I’ll go into umask, chown and file attributes. If you have anything to add, feel free to include it below.