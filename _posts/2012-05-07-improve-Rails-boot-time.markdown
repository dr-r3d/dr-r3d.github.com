---
title : Rails boot time may be slowing you down !!!
layout: post
---

I love to refactor my code, but for some not-so-good reasons, I was not writing the tests. And since I was breaking my existing functionality with ease while refactoring a method, I was forced to do TDD. The first frustration I had, that rake test was taking lot of time initializing. And guess where I found the culprit - **rails boot time!** 

It may be argued that boot time is a one time cost while starting the application in production and hence it does not matter. I may have completely bought into this argument, had I not started doing TDD. Each time test cases are run, the app is booted. At every run of rake task, migration or web server restart, rails app is booted. So a higher boot time adversely affect the development time of the app and does matters.

And the most trivial part of rails that does affect the boot time is - **Number of gems installed**. Wonder why?

*    When a file is required, rubygems tried to find it in the load path and if not found, it searched through the entire installed gems. To do this, it loads all gemspecs and searches for the file, which  means all gemspecs are evaluated and so the more gems are installed, the longer it takes. These [rubygems comments](https://github.com/rubygems/rubygems/blob/master/lib/rubygems/custom_require.rb#L20-32) explain this.

*    A second problem arises when the bundler is run. For each gem, rubygems reads the gemspec file, convert it into Gem::Specification objects and caches them. But sometimes bundler needs to modify GEM_HOME and GEM_PATH environment variables and if these are modified after the cache is built, the cache must be cleared. Bundler ver <= 1.0, clears this cache every time it runs. Since rubygems can load gemspecs before bundler is activated or while it is being activated, all gemspecs may be evaluated twice! Fortunately, @tenderlove [committed a patch](https://github.com/tenderlove/bundler/commit/a132e4720cadf290edd8754e87fa63ad9db1b2f7) to clear the cache only when the environment variables are modified and apps using bundler v >= 1.1 can take advantage of this.

    
So the boot time can be reduced, simply by
    
- Using gemsets ( another reason to start using RVM :P )
- Cleaning unused gems often ( just run **gem cleanup** )
- Upgrade bundler to version >= 1.1

