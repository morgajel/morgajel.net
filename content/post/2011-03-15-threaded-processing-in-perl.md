---
author: Jesse Morgan
categories:
- Uncategorized
date: "2011-03-15T19:04:08Z"
guid: http://morgajel.net/?p=1063
id: 1063
title: Threaded Processing in Perl
url: /2011/03/15/1063
views:
- "1277"
---

Suppose you have a list of 1000 IP addresses that you wanted to scan and inventory. If each IP address took 10 seconds to properly inventory, scanning serially would take 166 minutes… That’s a long time for a server wth 16 gig of memory, gig ethernet and 8 processors.

So rather than that, I’ve decided to parallellize my scan into 32 threads and feed in the IPs as threads free up. In an ideal scenario, this should cut the run time down to 5 minutes. Here’s the code that I plan on using:

```
<pre class="brush:perl">
#!/usr/bin/perl -w 

use strict;
use Data::Dumper;
use threads;

# Pretend these numbers are IP addresses.
my @ips = (1 .. 10);
my @threads;
my @results;
my $concurrent_threads=5;

#creates and scans 5 threads/ips
for (my $i=0 ; $i0 ; $i++){
    my $ip= shift(@ips);
    push @threads, threads->create(\&scan_systems, $ip );
    print "initial push of $ip on thread $i;\n";
}

# Pull an IP off the stack with each new thread until there is nothing left
while (scalar(@ips) >0){
    #See if any threads are complete
    foreach my $thread (threads->list(threads::joinable)){
        print "thread ".$thread->tid()." has closed\n";
        # Close that thread
        push @results, $thread->join(); 
        # Make sure we haven't exhausted our spare IPs
        if (scalar(@ips) >0){
            my $ip=shift(@ips);
            #since we just closed a thread and have another IP to use, create a new thread.
            $thread=threads->create(\&scan_systems, $ip);
            print "thread ".$thread->tid()." has opened\n";
            push @threads, $thread;
        }
    }
    # sure, why not? is it needed?
    threads->yield();
    #Keeps us from running like crazy, probably a more elegant way to do that
    sleep 1;
}


foreach my $thread (threads->list(threads::all)){
    push @results, $thread->join(); 
}

print "Done\n";
print Dumper @results;

# This is a replacement for my real scanner
sub scan_systems{
    my ($ip)=@_;
    my $random=int(rand(10));
    sleep $random;
    print "sleeping $random \n";
    my @result=(0..($random+1));
    return @result;
}
```

Please let me know if you can think of ways to improve/simplify this.