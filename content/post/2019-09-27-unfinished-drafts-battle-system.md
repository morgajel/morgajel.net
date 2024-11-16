---
author: Jesse Morgan
categories:
- Uncategorized
date: "2019-09-27T20:07:57Z"
guid: http://morgajel.net/?p=237
id: 237
title: 'Unfinished Drafts: Battle system'
url: /2019/09/27/237
---

***This article is from sometime in 2008. I was kicking around the algorithms for combat. While it didn’t go anywhere, it’s interesting to see where my mind was.***

Battle mechanics are always fun… but how to calculate battle and/or damage…

## Base characters

| stats | Fighter | Snapper | Snake | Worg | Fighter (lvel 2) | Fighter (level 20) |
|---|---|---|---|---|---|---|
| Lvl | 1 | 1 | 11 | 19 | 2 | 20 |
| atk | 12 | 5 | 15 | 28 | 16 | 28 |
| def | 10 | 12 | 5 | 18 | 10 | 18 |
| str | 12 | 12 | 5 | 18 | 16 | 28 |
| eva | 9 | 5 | 15 | 28 | 9 | 16 |
| maj | 4 | 2 | 2 | 4 | 4 | 7 |
| res | 6 | 10 | 10 | 18 | 6 | 10 |
| con | 10 | 8 | 5 | 18 | 11 | 18 |
| hp | 50 | 40 | 25 | 90 | 55 | 90 |
| total | 63 | 54 | 57 | 63 |  |  |

## Levelling

lvl 1: main stats(str,atk) +2, +5 points 27  
lvl 2: main and std stats(str,atk,def,con) +1, +5 points 38  
lvl 3: main A,std A, secondary A(str,def,eva) +1, +5 points 49  
lvl 4: main B,std B, secondary B(atk,con,res) +1, +5 points 50  
lvl 5: maj,eva +1, +5 points 61

## Weaknesses

stab/slash/crush/mag

## Base Equations

Chance to Hit = (atk + str\*.1)/(def + eva\*.1)\*.5  
chance for crit = atk/eva\*.1  
damage = rand(weapon-dmg) \* str/def \* ifcrit(1+str/def)

### lvl 1 Fighter Vs. Snapper

#### Snapper attack:

(5 + 12\*.1)/(10 + 9\*.1)\*.5 = 28% Chance to hit  
5/9\*.1= 5% Chance for crit  
**Jaws**  
(3 to 4) \* 12/10 = 3.6 min  
(3 to 4) \* 12/10 = 4.8 avg  
(3 to 4) \* 12/10 = 4.8 max  
(3 to 4) \* 12/10 \* (1+12/10) = 7.92 min crit  
(3 to 4) \* 12/10 \* (1+12/10) = 10.56 avg crit  
(3 to 4) \* 12/10 \* (1+12/10) = 10.56 max crit

#### Fighter attack:

(12 + 12\*.1)/(12 + 9\*.1)\*.5 = 51% Chance to hit  
12/5\*.1= 24% Chance for crit  
**Fist**  
(1 to 3) \* 12/12 = 1 min  
(1 to 3) \* 12/12 = 2 avg  
(1 to 3) \* 12/12 = 3 max  
(1 to 3) \* 12/12 \* (1+12/12) = 2 min crit  
(1 to 3) \* 12/12 \* (1+12/12) = 4 avg crit  
(1 to 3) \* 12/12 \* (1+12/12) = 6 max crit  
**short sword**  
(2 to 6) \* 12/12 = 2 min  
(2 to 6) \* 12/12 = 4 avg  
(2 to 6) \* 12/12 = 6 max  
(2 to 6) \* 12/12 \* (1+12/12) = 4 min crit  
(2 to 6) \* 12/12 \* (1+12/12) = 8 avg crit  
(2 to 6) \* 12/12 \* (1+12/12) = 12 max crit  
**Long sword**  
(4 to 8 ) \* 12/12 = 4 min  
(4 to 8 ) \* 12/12 = 6 avg  
(4 to 8 ) \* 12/12 = 8 max  
(4 to 8 ) \* 12/12 \* (1+12/12) = 8 min crit  
(4 to 8 ) \* 12/12 \* (1+12/12) = 10 avg crit  
(4 to 8 ) \* 12/12 \* (1+12/12) = 16 max crit