---
layout: default
---


## Software Archaeology

Sometimes with software - especially with software that has been running for many years - you need to figure out _how the heck this thing works_. 

This is often an important challenge. If software has been kept running for a long time, there's usually a good reason, and it's worth the effort to dig in and learn more. 

Some examples:
 - A laptop plugged in to the server room with a sticky note that says "DO NOT TOUCH".
 - A cloud VM called "arch-test" that folks believe is mission-critical. 
 - A repository for an important library that nobody knows how to build. 
 - A vendor-provided disk image that no longer works correctly. 

"Software Archaeology" is the term I like to use to describe the tools and techniques I use to learn more about abandoned software. I _love_ this sort of work, I think it's a ton of fun. There's not an enormous amount of Software Archaeology that needs doing (probably for the best that we're not all running a top-to-bottom abandonware stack). But when it's necessary, it's an exciting challenge, and is often a valuable exercise that yields technical dividends. 

## Unorganized Notes

 - provide actual examples 
 - procmon, the sysinternals suite on windows, linux tools
 - getting access to a VM in AWS (was important for digging in to some vendor tooling)
 - rg and friends to dig through code
 - willing to run ancient VMs
 - willing to read lots and lots of documentation, dig through old versions of the code
 - adding comments, logging, print statements
 - Scream test as a last resort, sometimes appropriate
