---
title: "A break with the past"
pubDate: 2010-04-27
blog: literate-programming
---
[Pretty soon](http://www.countdowntooauth.com/), Twitter is going to turn off Basic Authentication and switch entirely to OAuth. [People are upset](http://www.scripting.com/stories/2010/04/26/theToxicCoralReef.html). It’s natural. If apps aren’t updated, they’ll stop working, entirely. This could be bad.

But in the long run, it won’t be.

In any sort of long-term endeavor, things can go sour, due to accumulating cruft over time. You see this in married couples that argue every day, you see this in old-guard companies that have six layers of management, you see this in software of all kinds. You don’t want to break old stuff, so you support it. Rocking the boat may cause some problems. It could bring up old wounds. You might have to ask someone to improve. Things won’t ever be the same, and you’re gambling with the outcome, hoping that in the end, everything will be better.

Sometimes, this gamble pays off big.

Apple’s managed to do this three times in its history, in a huge way. They’ve reinvented themselves a few times, but there were three huge, backward- compatibility breaking changes that could have killed the company. The first was the transition from the 68k architecture to the PowerPC architecture, the eventual change from PowerPC to x86, and the transition from OS9 to OSX.

Apple managed to make all of these transitions successfully, and it’s been one of the greatest benefits to the company. A huge part of why Windows is so horrible is that it goes above and beyond the call of duty with backwards compatibility; Microsoft never wants to rock the boat. When they try to, they get punished. Look at how poorly the transition to Vista went. So what made it go so right for Apple, and so wrong for Microsoft?

The first thing that Apple did right with all of these transitions was being open about them. People were given ample time to move over to the new platform. I was young when the 68k change happened, so my remembering of that time might be fuzzy, but the OSX announcement certainly was made with ample time. An entire year elapsed between the public beta and the release of 10.1, which was widely considered the first version of OSX that was worth using. I remember buying a copy of 10.0, and Apple gave you an upgrade to 10.1 for $30, similar to the Snow Leopard upgrade. 10.0 was just too buggy to be considered a proper replacement. Anyway, that was a year, and that’s not counting the time between the announcement and the beta release. For the transition to Intel, the initial announcement was made in 2005, and Steve Jobs said the transition would happen over the next two years. That gave everyone plenty of time to get their ducks in a row.

The second thing that Apple did correctly was provide ample tools for dealing with the transition. For the first switch, they provided a 68k emulator, and kept it going all the way up until the Intel transition. This meant that people didn’t have to re-write their apps from scratch, and gave them a big window for re-writing apps. Rosetta fulfills the same role for the PowerPC/Intel change. And during the OS9/OSX changeover, Apple not only let you emulate OS9, but also created the Carbon framework to bridge the gap between System 9 and Cocoa.

Finally, Apple made these changes boldly, and didn’t back down. With any kind of massive change like this, unless someone forces the change through, people will stay with the old system, and the transition fails. You can see this happen with both the Python move to Python 3000, the move from Ruby 1.8 to 1.9, and the move from XP to Vista. There’s a fine line between giving people leeway to make the transition and actually forcing them to do it. Apple did a poor job with this when it came to Adobe, who’s being an incredible laggard with Photoshop. It’s a massive application, sure, but Apple announced Carbon’s end of life ages ago, they really should have gotten their shit together. Generally, Apple’s not afraid to end-of-life products after the transition time is over, though.

In any case, those seem to be the three things that make a transitional period work: transparency, tools, and finality. If Twitter wants to survive the Oauthcalypse (as if they won’t), they need to follow the same path. And they have been. They announced the move to Oauth over a year ago (I think), they’ve had libraries and examples out there for ages, and they’ve picked a date and they’re sticking to it.

I’m glad they’re doing it. OAuth is an important part of Twitter’s future, and it’s good that they’re embracing it fully.
