---
author: Jesse Morgan
categories:
- Work
date: "2019-09-27T19:56:34Z"
guid: http://morgajel.net/?p=833
id: 833
title: 'Unfinished Drafts: The Importance of Documentation'
url: /2019/09/27/833
---

***This article was originally written on July 19th, 2010, but never published.***

Documentation is another topic where there appears to be disagreement in the sysadmin world. When to document, what to document, who do document for, and where to store that documentation always seem to be subjects of contention. Everyone likes documentation, but no one has the time to document, and the rules for documentation often feel arbitrary. I’d like to open this up for discussion and figure out some baselines.

## Should I Document?

If you have to ask then probably; but it’s much more complex than that. Documentation is time-consuming and rarely of value at first, so few want to invest the effort into it unless it’s needed. There are several questions here that need to be answered:

- **Why should I Document?** What is the purpose of the documentation? Are you documenting a one-off process that you’ll have to do 10 months from now? Are you providing instructions for non-technical users? Perhaps you’re defining procedures for your team to follow. Whatever the reason, focus on it, and state it up front. There are few things worse than reading pages of documentation only to find out that it’s useless. Documentation for the sake of documentation is a waste of time.
- **What should I Document?** It’s very easy to ramble when writing documentation (as many of my articles prove). Step back and review what you’ve written, then remove any unneeded content. Find your focus and document only what needs to be explained, leave the rest for footnotes and hyper links.
- **When should I Document?** As soon as possible. Ideally you’d document as you worked, creating a perfect step-by-step record. Realistically, pressure to move quickly causes procrastination, but the truth of the matter is that the longer you wait, the less detail you’ll remember. Write down copious notes as you go, and massage it into a coherent plan after the fact.
- **Who should I Document for?** Write for your audience- a non-technical customer requires a much lighter touch compared to a seasoned techie. The boss may need things simplified that a coworker would instinctively understand. Pick your target audience and stick to it. Anything that falls outside of the audience interests should be flagged as “\[Group B\] should take note that…” Also remember that the person who requests the documentation may not be the target audience.
- **Where should I Document?** Where you keep documentation is often more important than the quality of your document. You can write the most compelling documentation in the company, but if it’s stored in a powerpoint slide on a shared drive, it’s of no use to someone searching a corporate wiki. Whatever your documentation repository may be, be it Alfresco, Sharepoint, Confluence or even Mediawiki, everyone has to be in agreement on a definitive source. The format should be searchable, track revisions, prevent unwanted access, and be inter-linkable.

Now that we’ve set some boundaries, let’s delve a little bit deeper into the types of documentation.

## Types of Documentation

Documentation can take many forms. Over the course of any given day, you’ll see proposals, overviews, tutorials, standards, even in-depth topical arguments.

. Each type of documentation has its own rules and conventions- what’s required for one set may not be needed for another. That said, here are a few general rules to follow.

- **Be Concise** – 
    - **NO**: thoughtfully contemplate the reduction of flowery adjectives and adverbs for clarification;
    - **YES**: remove unneeded words. Over-explaining will confuse the reader.
- **Be Clear** – Make sure your subject is obvious in each sentence. Ambiguity will destroy reader comprehension.
- **Be Accurate** – Incorrect documentation is worse that no documentation.
- **Keep it Bite-sized** – Large chunks of data are hard to process, so keep the content in small, digestible chunks that can be processed one at a time.
- **Stay Focused** – Keep a TODO list. Whenever you think of an improvement, make a note of it and move on.
- **Refactor** – The original structure may not make sense after a few revisions, so don’t be afraid to reorganize.
- **Edit for Content** -Make sure your topics are factually correct and the content flows properly.
- **Edit for Grammar** – Make sure your punctuation is correct and your structure is technically sound.
- **Edit for Language** – Make sure the text is actually interesting to read.
- **Link to Further Information** – If someone else has explained it well, link to it rather than rewrite it.
- **Get Feedback** – Feedback finds mistakes and adds value. The more trusted sources, the better off you are.

### Proposal/RFCs

Proposals can be immensely rewarding (or mind-numbingly frustrating), depending on if they’re accepted or not. That’s not to say you shouldn’t write them; even a failed proposal has value. The point of a proposal is to communicate an idea, a way to tell your team or supervisor “this is what I think we should do.” If you’re successful, the idea will be implemented. If you’re unsuccessful, you may find out a better way to do it. The overall goal should be to improve team performance. Here’s what a proposal should include:

- **The Problem** – What problem are you trying to solve? Why is it a problem?
- **The Solution** – A simple overview of the solution
- **The Benefits** – what benefits it will provide?
- **The Implementation** – How to implement it.
- **The Results** – Explain the intended results
- **The Flaws** – What issues are expected, and if there is currently a solution
- **The Timeframe** – When should this project be started and completed? How long and how much effort will it take?

Lets presume you write a knockout proposal. Everything is perfect, and with 2 days of effort you’ll reduce a 2 hour daily task to a 15 minute weekly task. Regardless of the benefits, the response will be one of these:

- **Complete Apathy** – the worst response, because it shows how little you are valued. No response, approval, or denial. If this happens, run your idea past an uninvested third party. Perhaps a critical set of eyes may reveal the problem.
- **Denied** – perhaps the benefit isn’t worth the cost, the risk is to high, there’s not enough resources, or some other issues not addressed. Try to get specific reasoning as to why it won’t work, and rework your proposal taking that into account.
- **Feigned Interest, no Support** – Be it plausible deniability or lack of interest, the response is weak. Push for a yes or no answer, ask what the concerns are with it.
- **Delay** – It’s a good idea, but not right now. There might be hesitance due to a minor issue. Find a way to calm their fears, then push for an implementation date, create a checklist of conditions that need to be met.
- **Conditional Agreement** – It is a good idea, but conditions must be met first. Create a checklist and verify that it’s complete.
- **Full Agreement** – This should be your end goal. Full agreement means support from the boss and the team on implementation. Without support, your efforts may be wasted.

You can’t account for everything in your proposal, so make sure not to paint yourself into a corner. A method for dealing with problems is more valuable than individual solutions. It doesn’t need to be perfect, but does need to be flexible.

The most important thing a proposal needs is buy-in. If your team and management aren’t behind an idea, implementation will be a struggle. The final thing to keep in mind is that not all proposals are good. If there is universal apathy for your idea, it might just be bad and you’re oblivious to it.

### Introductions and Overviews

Introductions are the first exposure someone may have to whatever you’ve been working on, be it a JBoss implementation, Apache configuration, or new software package. A clear understanding of what “it” is can help with acceptance. A bad introduction can taint the experience and prevent adaptation. So, how can you ensure a good introduction to a technology?

- **Explain the Purpose** – Why is the user reading this introduction? A new Authentication system? Messaging system? Explain why the reader should care.
- **Define your Terms** – Include a glossary of any new terms that the user needs to understand. Remember, this may be their first exposure to the topic. Don’t overwhelm them, but at the same time don’t leave them in the dark.
- **Don’t Drown in Detail** – An introduction should not cover everything in perfect detail, but it should give you references to follow up on.

The tone should be conversational- you need to draw the reader in, befriend them, and convince them that this new thing is not scary. This can be a tough task if the subject is replacing something that the reader if

### Document a Process (Installation, Upgrade, Tutorials, How-to, Walk Through)

Documenting a process serves three purposes- it trains new employees in proper technique, ensures consistency, and covers your rear should something go wrong. That last point may sound a bit cynical, but you never know when you’ll need it. The process itself should be clear enough that any qualified user can follow it. Process documentation should have the following traits:

- **Steps** – Well defined tasks that need to be performed.
- **Subtasks** – any moderately complex task should be divided up.
- **Document Common Problems** – Surprises can derail a new user. Acknowledgement and fixes for issues can help ease new users into the process.

Dry runs are essential in documenting a process- test the process yourself and have others test it as well. Continual runs will expose flaws and allow you to address deficiencies. Keep testing and refining the process until a sample user can follow it without issue.

### Topical guide (Feature-based)

Topical guides are both the most useful and yet the hardest documentation to write. They need to be thorough, both fully covering the material but not burying the user in frivolous details. So what should you cover in a topical guide?

- Be specific on the topic – Document a feature and all related material. If it’s not related, don’t include it.
- Cover Relevant Tangents –
- Be comprehensive – Cover everything a user needs to know, but remember it’s not intended to be a reference book.

### Document a Standard (How Something Should be Done)

Inconsistency is the bane of system administration, and consistency can only be had when everyone is in agreement on how things should be done. There must be agreement not only on theory, but also in practice. As such, standards should be documented. What should a standard entail?

- **Dynamic** – Not the first word when you think of standards, but something you have to face; your standard will become out of date quickly. Document it and give it a revision number. Soon enough you’ll realize
- **Audit** – It’s not enough to document a standard, you also need to enforce it. Periodic verification can spot issues before they become problems. If configuration files are identical, md5sums can be used to find inconsistencies.

### Annotation (Config Commenting)

One of the most common types of documentation is never published, yet often the most crucial in day-to-day operations. Comments within configuration files can explain what steps were taken and why.

- **Explain Why** – When you make changes, explain why you made the change.
- **Keep it Simple** – Comments should not overshadow the configuration. Leave over-documentation to sample configs.
- **Consider Versioning** – The best configuration documentation is a history of changes. Configurations that are both critical and fluid (for example, Bind zone files) are perfect candidates for versioning.
- **Sign and Date Changes** – When you make a change, leave your name and a datestamp. While versioning comments may be more permanent, inline comments provide instant context This is important when the change is revisited and no one remembers making it.