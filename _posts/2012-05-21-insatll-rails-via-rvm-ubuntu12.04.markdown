---
title : Super Easy Rails Installation
layout: post
---

=============================

Are you new to rails? Or did you had to struggle a lot to install rails
the previous time? Rejoice! RVM is now more awesome and it let install
rails. 

I installed Ubuntu 12.04 on a 3 year old Toshiba
Satellite and went installing **the awesome Rails** usning RVM. I have always found RVM to be very useful and I just found its another use - installing rails in just three commands :)

    Step 1: sudo apt-get update

    Step 2: sudo apt-get install curl git build-essential nodejs


You may skip Step 1 and 2 if these are already installed. Nodejs is
javascript runtime engine required by rails >= 3.1. Mac system already comes with a V8 javascript runtime engine. No need to install this in Mac. On other systems, instead of using nodejs, one can also use the gem 'therubyracer' by adding it to the Gemfile.

    Step 3: curl -L https://get.rvm.io | bash -s stable --rails
  
This script will install the latest stable ruby along with rails. It
also install all dependencies. 

Thats all!! Start building your awesome applicaiton with rails.
