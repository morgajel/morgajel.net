---
author: Jesse Morgan
categories:
- Hobbies
- IRC
- Jackie
- Jerks
- Personal
- Rant
- The House
- Work
date: "2008-07-10T11:33:40Z"
guid: http://morgajel.net/?p=255
id: 255
title: What&#8217;s blue and white and still not working?
url: /2008/07/10/255
views:
- "94"
---

My internet connection.

SO here’s the scoop

**5 days until cutover:**  
I call AT&amp;T, tell them I’m moving and need to transfer my Static IP DSL service on the 30th(Monday). Tech says no problem it’s all set. I am pleasantly surprised at how little of a hassle it was and that it was way smoother than any other interaction I’ve had with them.

**Saturday, 2 days until cutover:**  
We’re planning on doing the actual moving Sunday morning and plan to spend Saturday packing and planning. However at 3am Saturday morning, the internet connection drops, leaving me unable to contact many of the people who may be able to help us move. It sucks, but ok, we can work around it. I still have enough people to get by with and have ways to contact most of them. Since we asked to be connected on Monday, maybe they had to disconnect the old line the day before in order to get their stuff in place. Maybe they cut it on Saturday rather than Sunday because nobody works on Sunday. I get that, I can understand it. While annoying, it’s still better than my previous interactions with them.

**Sunday,move day, day before cutover:**  
We move on Sunday and realize that we never actually checked to see if the house had any phone cables in it. It didn’t. Fortunately my father-in-law knows a bit about phone installation and was able to help me wire up a stub for the AT&amp;T guy to connect to.

**Monday, 1 day after move:**  
AT&amp;T shows up, runs cable, says service will be enabled withing X hours. yippie.

**Tuesday, 2 days after move:**  
Connection is there, but my ip address has changed. “crap,” I think, “now I gotta update dns entries for our sites.” But I can understand this, perhaps my old static ip was tied to the network near my old apartment and didn’t reach this area. I can buy that. So I change my DNS entries… and they don’t work. I look again and I apparently mistyped the IP because the new DNS entry doesn’t match the external IP on the router. So I change it again. and 20 minutes later the external IP has changed again.

They had me on a fricking dynamic IP. For the non techies out there, large ISPs only have a limited number of ip addresses, and more often than not don’t have one for every customer. Since few customers stay online 100% of the time, they can take addresses away from people not using them and redistribute them as needed. This is called a Dynamic IP account. For people who run servers from their homes, keeping the same IP is important, so when your computer goes to connect to morgajel.com, it needs to be able to find the right IP address. That’s why I pay extra for AT&amp;T to guarantee me the same IP address. That is why I am pissed. While there are ways to get around this (dyndns) but they’re a pain in the ass an not an option for me since I run an IRC server as well.

So I tinker around, thinking maybe \*I\* did something wrong- maybe my router was reset and it cleared the static info. I dig around with Jackie’s help and find the original documentation and try to set up the networking listed manually. No dice. Then I remember that yes, they did manage the info via the PPOE settings, and that just required a user name and password, which is what I was originally using. I switch it back and get yet another dynamic IP. I should point out that my static IP range was 75.x.x.x, while the dynamic stayed in the 66.x.x.x range- this made it easy to keep track of what was going on.

So I call them up and surprise surprise, they screwed up. See, they don’t really transfer accounts so much as shut off the old one and create a new one. The tech didn’t bother to notice I had a static account and replaced it with a dynamic account. I’m livid at this point, and tell them that it needs to be switched back. “Ok, I’ll put in the order. It’ll be ready in 10 days.” Now, this should NOT take 10 days from a technical point of view, this is all red tape causing the delay. But WTF can I do, so I say hell with it and go along with it.

At some point my father-in-law comes back over to help with the baby gate and notes that the technician illegally ran the line through the neighbor’s yard. While I’m half tempted to yell at them to fix it, I just wanna get a connection up and running again so I can actually write about the house.

**Saturday, 5 days after cutover (timeline gets a little fuzzy here)**  
Connection is still flaky, but generally working. I call to check on the status of the static IP order, and find out it was never placed. They’ll get right on that.

**Sunday, 6 days after cutover**  
Connection goes down at 7:37am. Completely. It does not come back. Jackie calls tech support this time. Flames, brimstone cries of the undead ensue. Eventually I take the phone and find out there’s still no mention of a static order of any sort for our account. Guess what? They can’t do anything about it because “orders” isn’t open on weekends. They agree to send out a tech to look at the line since they can’t see the modem from their end. He should be out between 8am and noon on monday

**Monday, 7 days after cutover:**  
Connection begins working again around 7am- I think to myself “great, maybe they just took it down to switch over to the static IP- finally I can get my stuff up.” Nope, still a dynamic IP address. I call AT&amp;T to get the static IP address set up and let them know the connection is up. They say hold off until the technician confirms it’s not an issue. ok. I’ll call back later. I spend my time waiting for the technician looking for any other ISPs in the area on dslreports.com

Technician comes out, nice guy, doesn’t see anything wrong, says he’s seen this behavior before when switching from dynamic to static, but the business won’t fess up to it. Whatever. At least the wiring was good, presuming that both the installer and the inspecting tech were both competent. While he was tooling around, I found out that Cyberonic, my ISP from DC, covers this area (they didn’t in grand rapids or rochester hills). They resell business class Covad lines to residential customers. I contemplate switching over to them, but figure it would be too much effort since I’ve gotten this far. I’m not even sure they’d have a decent plan in this area.

So he leaves and I call AT&amp;T back and get the static all set up. She also said the static IP would be in place tomorrow. Just as we’re finishing she informs me that since I don’t have a contract, my payment will go up to $70 a month from $55. “WTF, this isn’t my screwup- you guys said you could transfer service, then you pooch it, then you want to charge me for it??”

“Oh, no,” she says, “When we transfer service, we don’t transfer contracts. If you want the original rate, you’ll have to sign up for another year of service.”

This is where Jesse snaps.

“You know what? Fine, make it the month to month price, because it’ll take me about 3 weeks to get covad in here.” She was a bit shocked by that statement, and the conversation ended awkwardly. I think she was supposed to ask if I was please with my experience but she knew the answer.

I then spent 10 minutes looking through DSL reports for ISPs in the area and narrowing down their plans- turns out that Cyberonic offers the same plan I had in DC for $60. Lets compare the plans side by side:

|  | AT&amp;T | Cyberonic |
|---|---|---|
| Download speed | 3meg | 6meg |
| Upload speed | 386k | 768k |
| IP address | 5 static | 5 static |
| Stability | False | True |
| Cost | $55/mo | $60/mo |

I call up cyberonic, phone is picked up on the 3rd ring. I tell the technician that I’m interested in their plan, I get signed up, cc infos taken, etc. The entire call lasted 22 minutes and 28 seconds. I was never transferred once, my call was never dropped, the technician never once said “I don’t know,” and they were going to do a hotswap on the line and cancel the AT&amp;T DSL for us since we obviously can’t have 2 DSL services on the same line. The transfer should take place in the next 7-14 business days.

I’d like to point out that AT&amp;T still hasn’t got their act together as of this morning (Thursday), and dropped my connection while I was beginning a deployment for work. That was real awesome btw. Thankfully my neighbor is allowing us to use his wireless connection until we get it straightened out. If the issues aren’t resolved by switching to cyberonic, I’ll have the neighbor report the cable crossing his yard and they’ll have to come out and redo it (this is my backup plan).

The good news is we’ve moved our blogs to [gopedro.net](http://gopedro.net). I’m still in the process of converting them, but expect to be done by next Monday. The only site that will still point to my static IP is morgajel.com, for my streaming music, IRC server, etc. We’ve also decided to move all of our pictures to flickr, so expect to see broken images for a while.

I really want to thank gopedro.net for in all of this. I highly recommend them for any domain name purchases or hosting. They’ve been handling our domain names for years now, and their service is outstanding. I’d also like to thank our new neighbor Bobby for being one hell of a cool guy.

I’ll keep you updated on how things go. Hopefully I’ll start writing about the house soon.

**\*UPDATE 2008-07-14\***  
Cybronic called and told me they’d be sending a technician out tomorrow to verify the lines. Hopefully I should have a working connection soon.

**\*UPDATE 2008-07-16\***  
My bad, it was wednesday. Connection is up now and I’m back online with a static IP!