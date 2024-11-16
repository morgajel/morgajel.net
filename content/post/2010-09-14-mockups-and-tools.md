---
author: Jesse Morgan
categories:
- IRC
- Rant
date: "2010-09-14T21:49:14Z"
guid: http://morgajel.net/?p=960
id: 960
title: Mockups and Tools
url: /2010/09/14/960
views:
- "100"
---

IRC can be an excellent source of information, especially on the right networks, but every once in a while you have a conversation that is so disappointing you have to share it.

- - - - - -

`<br></br>22:15 < morgajel> I'm using mockito to mock up a Dao, but I've run into an issue: What is the proper way to mock up chained getters, i.e. sessionFactory.getCurrentSession().createCriteria(Account.class).list()? a mock with a mock with a mock is considered bad form, so I'm not really sure how I *should* handle this.<br></br>22:15 <@toolA> Stop using mocks?<br></br>22:15 <@toolA> They are damned pointless anyways<br></br>22:16 <@toolA> Use somthing like Junit or spring-db tests, setup a whole cycle with a in memory db, then run that against your queries.<br></br>22:16 <@toolB> ~mock toolA<br></br>22:16  * javabot points at toolA and laughs<br></br>22:17 < morgajel> toolA... really? that's the advice you're gonna give me? "Don't use mocks when unit testing?"<br></br>22:17 <@toolB> yeah.  i prefer that route over mocks, personally, even though it means testing takes slightly longer.<br></br>`

He continued to rant against mocks and declare them useless. A couple of things disappointed me about this:

1. I asked a well formed, non-inflammatory question, and the response was to completely ignore the question and say what I was doing was worthless, despite being a well accepted development practice.
2. Not only did one oper state that mocks were useless, but another one didn’t disagree with his blanket statement. While they \*may\* have a point that mock objects do not cover everything, to call them useless is unreal, especially in a programming channel.
3. Rather than say “I don’t use mocks” and let someone who **could** answer my question respond, the conversation was then dominated by a diatribe about all of the faults with mocks.
4. The thing I took away from this was “don’t bother asking questions in this channel because people who don’t use the tech you’re asking about will pipe up and shoot you down regardless of how nicely you ask.”

It’d be one thing if I were off topic, or it was not a programming channel, but damnit I expected better. So I don’t think I’ll be going back there anymore.

(And no, the irony isn’t lost on me.)