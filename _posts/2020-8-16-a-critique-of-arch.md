---
layout: post
title: Installing Arch, a critique of new-user-experience
---

## Disclaimer

This blog posts, like all my others, will likely only contain partial or incomplete thoughts.
It's going to be built purely off of my (SUBJECTIVE) experiences getting arch set up and working.

Any references which do get posted are likely going to be outdated in short order, considering the pace 
at which arch updates.

---

## Preface

First of all, Arch is not this impossible-to-approach monstrosity. Following the guide and the wiki, one can get 
a basic, first pass system up and running with arch quite quickly... Assuming you stick with... KDE/GNOME as your
default desk environments, or TTY.

Arch is actually pretty simple. There are, however, a number of critiques I have for it. Arch is, what I would 
consider, pure, raw linux. Really, only Alpine Linux or building the kernel and making your own distribution of 
Linux is comparable. 

Learning and installing Arch and configuring it is immensely valuable for any person looking to gain knowledge
and expertise in Linux. Someone who has installed Arch and uses it as their daily driver, is certainly a Cut 
Above the Rest, with respect to knowing the various systems in play and being able to navigate deep and winding
wikis, manpages, and sometimes, the boards or communities surrounding it, which can be very daunting.

I found taking my spare time to learn and install and configure Arch a fantastic, though incredibly frustrating
learning experience, and admonish any person wanting to become a sysadmin or kernel or driver developer, to really
dive into it and figure out the various nasties at interplay.

---

## Additional Context - Why were things so hard?

I've primarily used Ubuntu for that last year and a half, as my daily Desktop OS. It has served admirably. I've used
the prepackaged XUbuntu, which uses XFCE as the desktop environment. I've admired how slim it was on CPU and RAM. For
my Tempera project, a mod for Halo Custom Edition, I needed/wanted to pull out some old tools. Originally, I was
running the game in a VirtualBox VM, but ran into some issues with the vGPU features. 

Along the way, while seeking better virtualization support, I grew frustrated and wanted to run a newer kernel and use
VFIO GPU passthrough. I also wanted a new file system to back some mission-critical files, but also avoid the costs
that come from that $10/month sacrificed to the Dropbox Gods. Their linux support is pretty shit, all things considered.

### The Rig: 

- Threadripper 2920X (? 2#20X, 12C, 24T)
- ASROCK x399 Taichi Motherboard
- 32 GB Ram
- 1TB M.2 SSD, probably Samsung
- 2x Vega 64 GPUs. 

I tried Fedora, but continually ran into the limitations of the GNOME DE. Not only did they hide and remove 
new features, but the latest version of GNOME also limits a key thing for me: "Discoverability"

### Required Features:

- GPU Passthrough
- Filesystem built for regular backups

### Desired Feature:

- Emphasis on supporting Wayland and the move to Wayland 

### Caveat:

- I only have finite patience. If I get frustrated, I'm liable to give up and move back to Windows until my 
spoons replenish.

---

## On Discoverability

(In my definition) 
 - Discoverability means: "If the user needs to do a thing, X, how hard is it for them to figure it out on their 
 own?"
 
 When I say GNOME is a regression, I'm referring specifically to this issue. Discoverability can rely on a few 
 things:

 - User's baked-in knowledge of how other systems behave "If I jump to linux from mac/windows, what stays the same?"
 - How good are the UI elements at indicating what a particular action does or will do?
 - How does the user figure out how to perform more complicated actions? 
 - How does the user "graduate" or learn how to learn? Is searching for answers from StackExchange the only way?
 
These are all failings of the Arch Wiki, the GNOME 3+ UX/UI, and the true defining failure of the Linux User 
Experience. 

Are you motivated to make a new Distro? Are you motivated to "Fix" the UI or problems with Linux in general? That
has already been tried. I don't think Linux will ever have a "proper" user experience or that passes that fine 
threshold of security and usability. If it leads to more GNOME 3+... I don't think you should try. No. 

Instead, consider revamping the arch wiki or making a derivative of the Arch wiki that's more _approachable_. For 
many thing you might want to do in linux or in Arch, there is no place to resort except Google, or even other people, 
and that's a crying shame. And that's because there's a lack of discoverability. 

### How the hell do we _use_ a tool? How the hell am I supposed to know that?

The real, major problem of Linux, isn't User Friendliness in the UI, it's not that we need to dumb things down. No.
It's not that we need to section everything off. No, it's that:

 - There's no way to find out some of this information without already knowing it or asking someone who knows.
 - I need to find a file and open it. How do I use the find tool? When does that tool accept `-h` vs `--help` ?
 - What is the script initialization order for my machine? How do I find out?
 - How do I use systemctl, how do I find out?
 - How do I get LightDM to start and then for Cinnamon (or any other DE/WM to load?)? 
 - How do I find out what's broken, _without already knowing_ what's broken?
 
These examples are all one-off examples, but if you go through the Arch wiki, the wiki will give you hints, but 
sometimes it won't tell you if your configuration is valid. I don't want a hand to hold me through all the steps,
I just want to know that what I did was correct.

Other distro's documents and guides will almost NEVER tell you to edit mkinitcpio.conf, but doing so managed to net 
me some big wins on boot times. What can users do with it, and how do they find out? Is it comprehensive enough that 
they can fill in the gaps?

---

This post was going to launch on a whole detailing of my travails and processes, and failures and annoyances, but 
I didn't wan't to focus too much on specifics, because this isn't about specifics. It's about...

## Lego's

The Arch wiki, bless those maintainers, along with most other linux documentation, is an _old_ manual for a box of
 lego's. It tells you how to plug a couple of pieces of the Lego's together, but for one reason or other, it may wind 
 up dropping some critical details that are required in order to to go from point A to point C.3. 
 
Unsure where that issue stems from, but it can be helped. If the Nintendo 3DS homebrew community can make a [kickass](https://3ds.hacks.guide/)
guide that covers almost all edge-cases and is user-friendly to use, the exponentially-larger Linux community can 
cobble together some extra documentation on how to go from point A to that point C.3.


## What's stopping *nix? 'User' research, maybe.

I think the issue is getting user experience feedback. Where are people stumbling the most, and how are we making it 
easy for them to provide extra feedback? When people are visiting the wiki, they're not particularly committed to
Arch and the deeper linux, and that's fine, but when someone gives a good college try at installing arch and getting
set up, but winds up frustrated, what are their concerns? How do they provide that feedback, and how does the community
address that feedback?

Those maintaining the Arch wiki are already self-selected to be incredibly knowledgeable about the areas they're
documenting, and will often make assumptions about pre-existing knowledge, having already visited a particular wiki
page, or even just knowing the underlying systems.

I don't want to dumb down the Linux Experience. that's not why I use it. No, I want to expand it and make it easier 
to navigate and get to my goals.

I shouldn't have to guess whether my `/efi` partition is set up correctly. I should know what patterns and methods and
 tricks people making the wiki use to verify their setups. 

## The Operating System as a means of User, Creative freedom and Expression
 
Arch and the Linux ecosystem is Experimentation, it is Configurability. It is giving the user ability to express 
themselves by their operating system. Distributions and environments that hide from users the complexity underneath 
harm this expression and make defaulting to that state of a lack of knowledge too easy. 

I want to see more attempts at user configuration, more attempts at making how the operating systems work, KNOWN
to the user, during regular use of the OS, by way of good user experience and good documentation and good logging.

I also want to see better online documentation.
