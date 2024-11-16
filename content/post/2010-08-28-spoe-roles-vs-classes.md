---
author: Jesse Morgan
categories:
- Projects
- SPoE
date: "2010-08-28T10:14:37Z"
guid: http://morgajel.net/?p=935
id: 935
title: SPoE Roles Vs. Classes
url: /2010/08/28/935
views:
- "75"
---

One thing I knew going into creating SPoE was that there were two types of users, Reviewers and Authors. Authors are essentially Reviewers with an expanded view. Since I’ll be using Spring and MVC, it made sense to make Reviewer a model class and Author a child class. Then I could simply add the functionality to Author as needed.

I’m starting to rethink that; Spring Security has the concept of Roles, which I’d originally planned on reserving for regular users and admins, but now I’m thinking that it might be a superficial distinction. Perhaps I’d be better off with the following:

- **Person Model:** Everyone who visits the site belongs to this model. There are no roles by default
- **Reviewer Role:** Anyone logged in would receive this role. can view snippets and
- **Author Role:** Anyone who has submitted a snippet would receive this role in addition to Reviewer role
- **Moderator Role:** Given to a select few to make sure that people are behaving. can remove snippets and critiques.
- **Administrator Role:** assigns moderators, bans users, etc.

Knowing now how spring security works, this seems the better option. Any thoughts?