---
author: Jesse Morgan
categories:
- Open Source
date: "2006-08-27T10:09:57Z"
guid: http://morgajel.net/2006/08/27/164/
id: 164
title: QtRuby QApplication &#8220;limit of one&#8221; workaround for Unit Testing
url: /2006/08/27/164
views:
- "51"
---

So I’ve been working with QtRuby a lot the last few days. I’ve also been playing with unit testing thanks to [drutro’s](http://morgajel.com/images/drutro.jpg) quick runthrough of it. The project I’m working on is a top-down view videogame, which at it’s base level requires loading up some images. The problem is using Qt::Pixmap requires that a QApplication exists. Now this in and of itself is annoying, but not a showstopper.

What becomes a showstopper is building a test suite when each test file has it’s own QApplication- you see, you can only have one QApplication at a time. So I thought “well, I’ll just build and destory the QApplication in the setup and teardown functions of the Unit Tests- then I found out that [oh you can’t destroy a QApplication](http://lists.kde.org/?l=pykde&m=114658756411698&w=3). They stick around and it’s like you never removed it. So I tried the Qt-4 ruby bindings and got the same result.

At this point I was getting damn frustrated. You couldn’t just put the QApplication in the suite or the test won’t work. you can’t put the QApplication in the tests or the suite won’t work… Then it occured to me that perhaps I should do both- so I found A LEGITIMATE USE FOR GLOBAL VARIABLES:

```

if ( ! defined?($qApp) )
    # Qt needs an application object to exist before loading a pixmap.
    $qApp = Qt::Application.new(ARGV)
end
```

I put that in the bottom of each test and in the suite as well and now it’s all happy. I hope people find this reference on google if they’re trying to do unit testing with QtRuby and find this problem. You can thank me by sending lots of money to paypal@ this domain 🙂

**Update**  
after talking with rdale, it turns out that qtruby has a variable set aside for this called $qApp- I’ve changed the variable above to reflect this.