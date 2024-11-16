---
author: Jesse Morgan
categories:
- Projects
- SPoE
date: "2010-08-17T23:05:15Z"
guid: http://morgajel.net/?p=908
id: 908
tags:
- agile
- SPoE
- Stories
- User
title: 'SPoE: User Stories'
url: /2010/08/17/908
views:
- "109"
---

This is intended to be a set of user stories/use cases for the author beta-reader site I intend to build (the initial article can be found [here](http://morgajel.net/2010/08/17/904)).

- - - - - -

The purpose of this project is to create a site where Authors can go to have their content beta-read. Site is meant as an author-to-author tool for critiqing snippets.

## **User Stories**

**User Account theme**

- As a user 
    - I want to be able to log into the site \[<span style="color: #00ff00;">done</span>\]
    - I want to be able to create an account \[<span style="color: #00ff00;">done</span>\]
    - I want to be able to change passwords \[<span style="color: #00ff00;">done</span>\]
    - I want to configure profile information \[<span style="color: #00ff00;">done</span>\]
    - I want to be able to recover passwords via email \[<span style="color: #00ff00;">done</span>\]

**General User stories**

- As a user, once logged in I want to see snippets that can be critiqued
- Users should be able to log into to see an interface where they can submit a snippet or critique a snippet.
- There should be two classes of users; Authors and Reviewers. All users start out as Reviewers and see a Reviewer interface, but can be upgraded to author status by submitting a snippet.

**Authors**

- As an Author, I should be able to upload a doc file or paste plain text snippets. 
    - Authors should have a simple interface for pasting plain text to submit a snippet.
    - There should be a separate field for the Authors to explain their content.
    - Authors should be able to tag the snippet similar to del.ic.io.us
    - Authors can use a “word replacer” to replace characters names if they wish to keep things secret. 
        - Must be capitalization agnostic
    - Must rate content: mature audiences(default), young adult, everyone
- Authors should see a list of their uploaded snippets as well as critiques for each one 
    - Authors can mark snippets as public or private
    - Authors see new critiques along with the user
- Authors have the ability to create psuedonyms
- Authors need to be able to give feedback on reviewers on how helpful it was  
    > *useless — non-useful — Acceptable-feedback — above useful — extremely useful*
- Authors can request reviews via the buddy system- They post their snippet and are presented with a group of snippets of a similar size and can buddy with another author
- Each uploaded snippet can have multiple critiques

**Reviewers**

- Reviewers set their default review preferences- i.e. “what they’re willing to review”
- Reviewers interested in short stories can select a posted snippet via a set of filters, which default to their preferences 
    - Snippets appear anonymous to the reviewer.
- Reviewers can select a story based on a one-sentence substring example
- Reviewer reputation based on author feedback of critique
- Reviewers can become authors when they wish to post their own snippet
- Reviewers can rate a snippet on the following scale

> *horrible read — boring read — Average read — good read — exciting read*

**Snippets**

- Multiple length categories: 
    - Paragraph
    - Scene
    - Chapter
    - Short Story
    - Novella

**Reviewer Tool**

- Reviewer should be able to insert comments inline, highlight text, or cross off words
- Reviewer needs to be able to save/autosave their comments as they work before submitting them
- Should have a googledocs-collaboration feel

**Tech Requirements**

- Should authenticate with OpenID credentials

- - - - - -

**Why would someone want to be a reviewer?**

- Become a better author themselves, even if they don’t submit a snippet
- assist new authors
- have your voice heard on what’s wrong with modern writing
- read something new without the commitment of reading a whole book
- taste test new authors and genres

**Why would someone want to be an Author?**

- Anonymous feedback on your book
- Help fellow authors
- Network with other authors
- May help alleviate writer’s block

If you have any suggestions or feedback, please let me know.