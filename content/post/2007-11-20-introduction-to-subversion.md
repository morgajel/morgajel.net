---
author: Jesse Morgan
categories:
- Development
- Hobbies
- Open Source
- Utility
date: "2007-11-20T16:03:25Z"
guid: http://morgajel.net/2007/11/20/223/
id: 223
title: Introduction to Subversion
url: /2007/11/20/223
views:
- "51"
---

I was planning on simply republishing my previous svn article, but realized that it sucked compared to what I know now.

### Prerequisites

I’ll presume you have the following things.  
– a Linux machine  
– subversion already installed

###  Terminology to Know 

There are a few terms that get mangled if you’re coming from other types of source control. This is just to clear things up.  
– **Repository:** the central storage place on the subversion server where your data is kept.  
– **Checked-out copy:** Unlike VSS, saying something is checked out does not imply that the file is locked. Also referred to as a local copy; but bear in mind that it doesn’t contain \*all\* of the data of the actual Repository.  
– **commit:** save the changes you made locally to the repository.

###  Mental Hurdles 

All of the files stored in a subversion repository are stored in a meta-filesystem. Much like an ISO image sitting on your desktop is not simply a folder full of files, a subversion repository is not directly accessible when you open it. Instead, you’ll see the guts of the repository- DB files, hooks, locks, etc. Don’t go digging through there to manually change your files- it’ll break things.

Another important one is the meta-filesystem. The “inside” of your repository is just a big filesystem. Much like /bin and /home exist on a linux machine by convention, there are certain base-level directories you should create for convention’s sake. The first thing you should do with a new repository is create three new base level directories: /tags, /trunk and /branches. Your main development will take place in /trunk. Don’t worry about the other two at the moment.

When I refer to the root of the repository, I’ll often refer to it as / or root. This \*IS NOT\* your server’s root directory or the physical location of the repository- it’s the part of the meta-filesystem where /trunk, /tags and /branches reside.

###  Step 1: Creating the Repository

Creating a repository is fairly simple. Anyone can create a repository where ever they have write access. All they must do is run  
`<br></br>  $ svnadmin create ~/project1/<br></br>`

This should create an empty repository. This will demonstrate what I’m talking about when I say that your repository is a meta-filesystem:  
`<br></br>  morgajel@unicron ~ $ svn ls file:////home/morgajel/project1<br></br>  morgajel@unicron ~ $ ls /home/morgajel/project1<br></br>  conf  dav  db  format  hooks  locks  README.txt<br></br>  morgajel@unicron ~ $ svn mkdir file:////home/morgajel/project1/trunk -m 'creating trunk'`

 Committed revision 1.  
 morgajel@unicron ~ $ svn ls file:////home/morgajel/project1  
 trunk/  
 morgajel@unicron ~ $ ls /home/morgajel/project1  
 conf dav db format hooks locks README.txt

Here you can see the repository was empty, then we created a directory called trunk (using a commit message to describe the change we made), then showing that the directory was in fact created. Do the same thing for /tags and /branches.

We now have a working repository!

### Checking out the Repository

Creating a repository is fine, but using it would be much more… useful. Next you should check out a copy of your project. Under normal conditions you’ll only be checking out the /trunk, but you mileage may vary in different situations. Since this is on our local machine, we can use the file:// protocol. Other protocols exist, like http:// (webdav), svn:// (svnserve), and svn+ssh:// (svn over ssh), but you don’t have to worry about them right now.

`<br></br>  morgajel@unicron ~ $ svn co file:////home/morgajel/project1/trunk my_project1<br></br>  Checked out revision 3.<br></br>`

This should check out the empty /trunk directory to a local folder called my\_project1. The only thing in this directory is a hidden .svn directory which holds the guts of your local copy repository info. It’s similar in function to the CVS directory in CVS. Unfortunately a nearly-empty directory isn’t much use, so let’s add some content.

### Adding Content

So let’s add some content.  
`<br></br>morgajel@unicron ~ $ cd my_project1/<br></br>morgajel@unicron ~/my_project1 $ mkdir -p lib bin share/docs<br></br>morgajel@unicron ~/my_project1 $ touch configure Makefile share/docs/README lib/foo.pm bin/widget.pl<br></br>morgajel@unicron ~/my_project1 $ svn status<br></br>?      configure<br></br>?      share<br></br>?      lib<br></br>?      bin<br></br>?      Makefile<br></br>`

Here I created a bunch of directories and created some empty files. When I ran svn status, svn told me that there were 5 things it wasn’t versioning. Let’s add them.  
`<br></br>morgajel@unicron ~/my_project1 $ svn add configure share lib bin Makefile<br></br>A         configure<br></br>A         share<br></br>A         share/docs<br></br>A         share/docs/README<br></br>A         lib<br></br>A         lib/foo.pm<br></br>A         bin<br></br>A         bin/widget.pl<br></br>A         Makefile<br></br>morgajel@unicron ~/my_project1 $ svn status<br></br>A      configure<br></br>A      share<br></br>A      share/docs<br></br>A      share/docs/README<br></br>A      lib<br></br>A      lib/foo.pm<br></br>A      bin<br></br>A      bin/widget.pl<br></br>A      Makefile<br></br>`  
As you can see, it recursed down into share, bin and lib and added all the goodies inside of each directory. You can also see svn status shows these as well. Keep in mind they’re just slated to be added to the repository- they’re not added yet. Let’s go ahead and commit them.

`<br></br>morgajel@unicron ~/my_project1 $ svn commit -m "a bunch of empty files and directories"<br></br>Adding         Makefile<br></br>Adding         bin<br></br>Adding         bin/widget.pl<br></br>Adding         configure<br></br>Adding         lib<br></br>Adding         lib/foo.pm<br></br>Adding         share<br></br>Adding         share/docs<br></br>Adding         share/docs/README<br></br>Transmitting file data .....<br></br>Committed revision 4.<br></br>`

### Modifying Data

So suppose you’d like to modify these files, you you decide to move the README to the root of your local copy (~/my\_project1/):

`<br></br>morgajel@unicron ~/my_project1 $ svn mv share/docs/README README<br></br>A         README<br></br>D         share/docs/README<br></br>morgajel@unicron ~/my_project1 $ svn stat<br></br>D      share/docs/README<br></br>M      bin/widget.pl<br></br>A  +   README<br></br>`

Notice that I used svn mv to move files rather than regular old mv- That’s to make sure svn is aware of the move and keeps the file history associated with the new file. You can also see bin/widget.pl now include some new info as well, and displays an M\[odified\] next to it. The + next to README shows that it copied the history over from it’s previous position. So what happens if we move a file without svn mv?  
`<br></br>morgajel@unicron ~/my_project1 $ mv configure config<br></br>morgajel@unicron ~/my_project1 $ svn status<br></br>?      config<br></br>!      configure<br></br>D      share/docs/README<br></br>M      bin/widget.pl<br></br>A  +   README<br></br>morgajel@unicron ~/my_project1 $ mv config configure<br></br>`  
You can see that svn panics(!) that configure has gone missing, and sees this new file called config that it’s currently not revisioning. It doesn’t know that they’re the same file.

###  Commit Messages

You’ve seen me use the -m flag a couple of times now- I’m using it to keep things flowing. If you don’t use it, you’re prompted in your favorite $EDITOR to create a commit statement, which includes the list of modified files. Using the -m flag is useful if you’re scripting commits (I use this when dumping and committing a nightly config file from our load-balancer).

Most of the time however, you’ll use your Editor. Make sure to keep your commit messages sweet and to the point- other’s will see them.

`<br></br>morgajel@unicron ~/my_project1 $ svn commit<br></br>[vim shows up, I enter the following text]<br></br>Small changes to demonstrate movements<br></br>- moved the README<br></br>- added shebang to widget.pl<br></br>[save and exit vim]<br></br>"svn-commit.tmp" 8L, 190C written<br></br>Adding         README<br></br>Sending        bin/widget.pl<br></br>Deleting       share/docs/README<br></br>Transmitting file data .<br></br>Committed revision 5.<br></br>morgajel@unicron ~/my_project1 $ svn update<br></br>At revision 5.<br></br>morgajel@unicron ~/my_project1 $ svn log<br></br>------------------------------------------------------------------------<br></br>r5 | morgajel | 2007-11-20 15:55:21 -0500 (Tue, 20 Nov 2007) | 4 lines`

Small changes to demonstrate movements  
\- moved the README  
\- added shebang to widget.pl

\------------------------------------------------------------------------  
r4 | morgajel | 2007-11-20 14:17:04 -0500 (Tue, 20 Nov 2007) | 1 line

a bunch of empty files and directories  
\------------------------------------------------------------------------  
r1 | morgajel | 2007-11-20 13:35:28 -0500 (Tue, 20 Nov 2007) | 1 line

creating trunk  
\------------------------------------------------------------------------

You’ll notice that revisions 2 and 3 aren’t listed- if you’ll remember correctly, they were used to commit the /tags and /branches directories. svn log only reports changes that affect the current target (in this case, ~/my\_project1 which is a local copy of /trunk).

There’s a couple more tips and tricks I could go on about- if there’s any interest in this post maybe I’ll write some more about more advanced topics.