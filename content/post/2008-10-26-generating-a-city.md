---
author: Jesse Morgan
categories:
- Uncategorized
date: "2008-10-26T15:44:32Z"
guid: http://morgajel.net/?p=351
id: 351
title: Generating a city
url: /2008/10/26/351
views:
- "31"
---

So I’m really lazy when it comes to creating cities for DnD, and this book is no exception. a few years back I wrote a samll utility to generate a city from a given xml file full of choices. After attempting to use it to generate some towns, I found it really wasn’t finished and the code was unreadable (it was perl after all, and city generation is complex). So, I’ve rewritten it in Ruby, and while it’s not done yet, here’s a good example of what it’ll produce- see if you can figure out what the madlib words are…

```

Name: Jamvita, Town (15938246)
Population: 3322 (Large Majority)
Government: Royalty                                                                                           
Alignment:                                                                                                    
         Moral: unreliable  (chaotic -26)
         Legal: undecided  (neutral -3)
Breakdown:
                70%     2350    Elf
                16%     537     Human
                8%      268     Goblin
                3%      100     Half-orc
                2%      67      Gnome
```

Storyline:  
Jamvita, population 3322.  
You reach Jamvita at nightfall (~08:00pm). It is a Town with a Large Majority population.  
The Town is run by a chaotic neutral Royalty. Law Enforcement is is loose, and law-breakers will face a Tribunal, then be sentenced to forced labor.

There is a secondary power in the city, an Ex-Blacksmith, that avoids confrontation with current leadership, while secretly is pulling strings with a trade adversary.

The first tavern you see is the Angry Bag Tavern, a regular, surprisingly dirty Tavern for the high class, where violence is ignored.

——————

```

            Name: Millland Rock, Small Town (71347337)                       
            Population: 1042 (Majority)
            Government: Cult Leader
            Alignment:
                     Moral: dependable  (lawful 23)
                     Legal: unconcerned  (neutral 14)
            Breakdown:
                55%     574     Elf
                19%     198     Human
                12%     125     Gnome
                10%     104     Dwarf
                3%      31      Goblin
                1%      10      Half-elf
```

Storyline:  
Millland Rock, population 1042.  
You reach Millland Rock at mid-day (~04:00pm). It is a Small Town with a Majority population.  
The Small Town is run by a lawful neutral Cult Leader. Law Enforcement is is loose, and law-breakers will face a Magistrate, then be sentenced to community service.

There is a secondary power in the city, a thieves guild, that openly denounces current leadership, while secretly moving to assert black market dominance.

The first tavern you see is the Salty Monkey Taproom, a large, surprisingly filthy Taproom for the middle class, where violence is not tolerated. Local law enforcement protects the establishment, although craps and prostitution can be found in the back rooms.

If you dig deep enough, you can find the following Black Markets:  
 – good Thieves guild  
 – neutral Adventurers guild  
 – powerful magic item trade  
 – stolen goods gem trade  
 – rare art trade