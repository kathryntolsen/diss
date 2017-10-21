# DISS - Debian Integrated Support System

Project with the ambitious aim to synergistically integrate all Debian's support resources and offer a simple and intuitive interface to well-tested diagnostic procedures.

This is by no means meant to replace, and in fact very much depends upon, all our existing resources. The rationale here is that our system is growing exponentially and we ARE "the universal OS", and that we have limited support resources and not everyone knows how and in what order to use them all properly which results in many well known and easily observable issues.

# Issues we want to address

1) Support team burnout through handling repetitively, the known issues we've already solved, having to explain our procedures and policies, and gather (at times pry out) information relevant to the issue at hand, etc

2) User alienation through lack of understanding of the systems, and unproductive interaction with supporters and such. It's been a well documented mindset among some of our best supporters, myself included that we don't really want those kinds of users who are inexperienced because they're just going to become a draw on our resources without contributing to our project. It is also an easily observable FACT that the developers and more experienced users are often the last people to find bugs and issues because they not only tend to be less adventurous in trying different software because they already know what they like and use, and they use it in the way it was intended to be used. It takes someone inexperienced to try out all sorts of options and use things in ways that will uncover obscure errors. This is a valuable commodity to have masses of inexperienced users scouring over our ever growing software base, uncovering issues we would otherwise miss. However we need to ensure that the feedback we get from this valuable resource is meaningful and winds up in the right place without the aforementioned issue occuring, thus we need a filter of sorts that caters to their needs as well as ours.

3) We also need to make sure that all the effort toward those two issues gets utilized. That is we cannot keep having users coming to us with self-interest of solving an issue, and that effort being underutilized because it was not properly documented. Some users come back and share solutions to problems, sometimes we wind up creating a factoid about it, sometimes there ends up being a wiki page, bug report, etc.. but most of the time that is not the case. Furthermore, we don't always know this has been done when the issue comes up again. Our current system relies on our supporters to remember these things, and when those people who remember it are not looking in at the time, we can wind up falling back to issue #2 and alienating our users, or not getting the issue documented or resolved at all, even ineffectively.

# Software components 

Client/Frontend (readline, curses, GTK, Qt), Diagnostics (Signed diagnostic tree files), Bot (IRC), Server (Issue Tracker)

# Client

The client will be a reportbug style wizard that will allow a user to select a program (on lower skill levels, use generic names like "filemanager" and have it automatically detect the actual program name or use a grab fuction where the user can click on a window and get the command) and enter a description of their issue and should have various classes of issues (Network, Sound, Crashes, Build errors, Package system problems, etc.) and optionally a CC address (email can be masked by the tracker with a issue ID and email bounced back to the user for privacy, with ability for the user to then opt out of further CC by emailing the tracker with the ID telling it to stop).

The first tier will then use Diagnostics to perform simple tests and ask further quetions, and gather information and compile a report/log on the issue. 

The logs should most likely be parsed/serialized/sterilized to remove or substitute personal data like IPs, MAC IDs, Usernames, maybe even path/filenames and replace them with generic ones like 1.2.3.4 or 12:34:56:78:90 or such. 

Then if the problem is not able to be resolved through automatic diagnostic processes that identify the problem based on well-known, worn, and battle-tested simple solutions, the client will in 2nd tier do exactly as reportbug does and lookup bug reports (and/or forum/wiki posts) and display them to the user from existing knowledge databases we already have.

If the issue remains unsolved it will then in the 3rd tier facilitate forwarding the issue to the issue tracker and giving it an ID number, then forwarding it to our support tools (IRC/Mailing lists) that we already have, from within the client itself, and if IRC is used this will be facilitated by the bot issuing a "<dissbot> Issue #98153: no sound in pianobar" in the channel notifying our supporters of an open issue, and if the user doesn't recieve a solution or decides to leave, the issue can then remain open and/or be forwarded to mailing lists and they can follow up by visiting the tracker website with their ID number or by reciving email notifications on the issue from the tracker.

If the issue is still not solved, it can be forwarded to 4th tier which would be things like the BTS (filing the report on the BTS, as it is determined to be a software issue by the supporters), or upstream possibly.

# Diagnostics

The diagnostics tree files can possibly be some sort of XML or such, and will need to be signed and verified, the core diagnostic tree will be much like what reportbug does, it'll just gather some preliminary information about the system and verify that it's Debian, what version, arch, and what sources apt prefers, etc.  The others which will be developed and maintained over time will be specific to the categories mentioned earlier, a sound issue diagnostic tree file for example will do things like perform a sound test and ask the user if they heard the sound, check their mixer, ask them to check their connections, etc. 

These diagnostic tree files will also facilitate further information gathering by running commands that gather more information specific to the type of issue in question. These commands will need to be shown and explained to, and verified by the user, as well as the reports/logs/output be shown and (optionally) parsed/serialized/sterlized to remove any personal information. These diagnostics will need to be signed and rated, and the issue tracker will facilitate doing so in the same manner any good forum's rating system does, only using gpg signatures, and rating a solution will not only cause the client to sign that diagnostic increasing its trust, but also increasing the trust of the supporters who contributed to those portions of diagnostic. 

Whenever possible all security mechanisms available from things like chroots to kernel-based security mechanisms should be employed to lock down the things diagnostics do, and they should be simple and unintrusive as possible. We're not looking to create a sentient diagnostic tool, just a few simple checks for known configuration issues, simple tests, and compile data for further support.

# Bot

The IRC bot should be not only a conduit/proxy for the user to the IRC support channels (up for much debate) perhaps speaking on the user's behalf in the channel with a generated user ID or issue number, which will not only allow us to ensure the user only sees stuff related to their issue, but that the issue tracker knows what support responses belong to the issue for later documentation of the issue and solution back to our existing tools like the wiki, forums, mailinglists and such. This can be done various ways, and IRC client scripts can be written or client features used to tab-complete these IDs like a normal nick, or perhaps a supporter can message the bot to "sign on" to an issue so that talking to the bot sends info back to the user you're signed on to. 

The bot will also facilitate access to the compiled report with information gathered by the diagnostic and submitted by the user, elminating all the chatter about running very common commands and using pastebins and such. Furthermore the bot could possibly behave as a client interface for opening a new issue in the tracker (possibly even if deemed necessary, only by a known and registered supporter) by someone in a normal standalone IRC client. 

In short the bot is the glue that binds DISS to the IRC support channels, and care should be taken to make sure that the info winds up in the right channel depending on the user's preferred language, branch of Debian, and perhaps even what package or issue catergory they have, as we have specific channels for different things (thus eliminating need to refer people to other channels and deal with any reluctance they may have or them having negative feelings about us doing so).

# Server

The tracker will contain metadata regarding the issue, it will generate an issue identifier, keep track of any CC address the user supplied, and where the reports are (paste.debian.net most likely), and the status of the issue, as well as any forum, mailing list, BTS, or other things that the client or supporters cause to be linked to this issue. It should not be some sort of new wiki or forum in and of itself, just a frontend linking and gluing it all together with metadata about the issue. It should have a web interface similar to the BTS. 

Issues and bugs are different; bugs are actual problems in the software, where issues are most often just PEBCAK or such. This is why it is necessary to create a new tracker, because this one is only serving to short-term track the issue and make sure it gets to the right final resting place. The tracker will provide the bot and client with the information needed to make a factoid in our existing info bots, file a bug report, or issue an email to mailing lists, and serve as a place where any interested party can figure out which of these things has occurred and where to find them. It is not a replacement for, but a wrapper of, all our existing systems. It is the glue that binds all the components of the DISS, including all the ones we already have.

# Further considerations

It has been suggested many times that we simply improve existing systems, and that is part of this, but it's not instead of this. That would still have the problem of issue #2 Alienating users because they'd need to know about and how to use those things. This would be a piece of software in the OS itself that integrates and facilitates using all that in an intuitive way that doesn't require a year or more of learning policy and practice. 

In regard to improving existing systems, this project and its contributors will seek to unify registration across any and all Debian support systems requiring registration, and working with the current teams of those systems to integrate them together in a synergistic fashion.

Furthermore, existing systems can be used/adapted at the descretion of the current developers working on other areas, for example the BTS and Tracker can be one and the same, and these "Issues" can just be a much lower class of bug that the maintainer is not bothered with, and the reportbug can just be extended to include these other features, and an existing IRC bot can be forked and modified to add the needed functionality. This is not just a project aiming to create a single new piece of software, but to adapt all we have now to better serve us going forward. 

# What we need

We need programmers. Those skilled with python as it seems well suited to these tasks, easy to code and powerful and flexible enough to develop the things we need with less dependencies outside the base system. Those skilled with trust based systems and services, GPG signatures, etc. Those with GUI/frontend programming experience. Those familiar with the Debian development process and all the concerns of users and developers alike. Those who can program client/server network stacks using sockets, HTTP, email protocols, etc.  Those who can develop a robust API for Debian support systems to communicate effectively, which will require knowledge of integrating applications on and off the web.

We need input and planning around our existing systems, effort by the existing teams and aid for them to incorporate a uniform Debian login credential system that will work across all Debian sites and services.

We need people who will work on the documenting and interfacing with the web presence of this project, keeping the status information and project(s) goals and such clearly defined.

# Roadmap

This project just started in the early morning hours of Friday October 13, 2017, around 2am US/EST time. As of this writing we're not even 24hrs in, and we already have half a dozen or so people hanging out in the channel, and responses on various forums. We are all just noodling at this point, tossing around ideas and trying to carefully make preliminary decisions that will shape the project and its design.

The first goal here is to establish a firm web presence with a wiki and such that will diagram out the anatomy of this integrated system and the progress of it, so people can understand where we're coming from, where we're going, and how far along we are.

The second goal here is to flesh out an API that will define the features and communications of this system, and I am not a very experienced programmer but I've seen it done for decades, that sometimes you have to make something (a tool) to make something else, and in this case I think starting work on the client interface, particularly the GUI frontend will inherently flesh out the API. And at first without an issue tracker, issues will not be persistent, it'll be simply a client talking to a rudimentary bot likely living in our development channel. 

It should be stressed that this is not something we want to push out and deploy rapidly, we want to get some working framework and do extensive alpha-testing outside normal channels, initially without any modification to existing services to help facilitate integration, as the API isn't there yet. Once we have working components and interest of a maintainer and those already within the Debian Developer circle, we'd want to start a beta-testing phase for use only on non-production testing/unstable systems. Once there is confidence in the implementation of secure trust mechanisms for the diagnostics, the system can be actually packaged for Sid and hopefully rolled into a future Debian stable release. The diagnostics files will most likely use some kind of repository that can allow them to be developed over time and implemented into the client without waiting for a new Debian release cycle, based upon a seperate rigid testing and signing/verifying process.

Long term, we'd like to see Debian installer have a more robust skill determination as its first step, with more than just a normal/expert install mode, and this support client being automatically installed by default on systems not selecting advanced or expert levels. We'd like to see all our supporters participating not just in free-range support, but registering into a trust-based system and using GPG signatures, so that our knowledge base can be higher quality and more trusted.

# Links

[DISS Wiki](https://github.com/kathryntolsen/diss/wiki) 
  
[Reddit Thread](https://www.reddit.com/r/debian/comments/762lg9/call_for_volunteers_for_a_new_debian_support/) 
  
[Debian Forums Tread](http://forums.debian.net/viewtopic.php?f=20&t=134959&p=656143#p656143)  
  
[Debian-Project Mailing List Thread](https://lists.debian.org/debian-project/2017/10/msg00025.html)  
  
[Debian-Devel Mailing List Thread](https://lists.debian.org/debian-devel/2017/10/msg00364.html)  
  
[Debian-User Mailing List Thread](https://lists.debian.org/debian-user/2017/10/msg00754.html)

