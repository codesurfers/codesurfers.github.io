---
layout: post
title: "Using OpenBSD as a daily driver"
author: "Samuel Chase"
---

## Journey to explore the BSDs

I was an ArchLinux user for a decade between the years
2010-2020. While it was mostly without incident, during a regular
upgrade my wifi stopped working. This led me to a bit of reflection
regarding the complexity of the software I am using, and wanted to try
out something different, something simpler.

At the time, I had been intensely curious about FreeBSD (and the BSDs
in general) so decided it was time to give it a try. I installed
FreeBSD, configured [sway](https://swaywm.org/) and got things working
decently well enough. The only major annoyance was that I had some
issues running `<SUBATOMIC-PARTICLE>` apps, but none that were a huge
dealbreaker.

When I finally gathered the courage to try building my first desktop,
FreeBSD was the natural choice to install on it. I remember how
excited I was when FreeBSD first booted up and the immediate
disappointment when I realised it did not support my wireless
card. That dashed my hopes of running a NAS with OpenZFS, and I was
back to deciding what I could install.

As an aside, if we are interested in running FreeBSD, some guides
that may help are:
1. [A FreeBSD 11 Desktop How-to](https://cooltrainer.org/a-freebsd-desktop-howto/)
2. [FreeBSD on a laptop](https://www.c0ffee.net/blog/freebsd-on-a-laptop)

I am not sure whether the advice there is up to date, but I remember
them helping me a lot. Caveat emptor etc.

A bit of online searching led me to understand that my wireless card
would be supported on OpenBSD, so I proceeded to install it.

## Motivation

My original motivation to use OpenBSD was to run an offbeat OS on my
desktop which supported my wireless card. While I was aware of
OpenBSD's reputation of being secure, that was not a priority for me,
though it is nice to have.

## Installation

The OpenBSD install process was not complicated from what I
remember. Within around fifteen minutes or so we should have a
machine that boots into OpenBSD.

## Maintenance

### Hibernate

Hibernate works well on OpenBSD most times. Every once in a while, I
have noticed that attempting to unhibernate results in a fresh boot. I
have not been able to investigate or isolate what situations causes it
to do that.

I almost never turn off my desktop. I hibernate every time, because
it's nice to not have to restart all the programs we need in the way
we left them last.

I am not sure what the state of hibernation in the Linux world
is. Maybe it has improved now.

### Upgrades

Whenever there's a new version of OpenBSD released, I upgrade my
desktop. There's usually not many steps required, and over the last
three years it has worked well enough for me not to make me anxious.

However, upgrades should always be considered a risky operation, so
doing them without all the usual precautions for your data is not
recommended.

## OS Infrastructure

### Drivers

#### Graphics

We have to use AMD cards as they have open source drivers. Not sure
how NVIDIA cards will fare on OpenBSD.

#### Wireless cards

While OpenBSD supports my wireless card, it is well known that there's
a bit of a hit when it comes to network throughput. Based on my
understanding, this is due to the driver not supporting the latest and
greatest wireless specs.

Writing and testing drivers is an extremely technical and painstaking
endeavour. We cannot expect this situation to improve without proper
funding for this work. 

I am glad that my OpenBSD desktop can connect to my home wireless
network, however be prepared for yours not to have blazing network
performance.

### Filesystems

FFS can be a bit jarring for someone used to other state-of-the-art
filesystems. It works well enough.

#### Recovering from a power outage

Using an OpenBSD system for reliable storage/archival of data
(e.g. like a NAS) is not recommended. We would be better served with
the other filesystems commonly used for such purposes.

#### Integrating other filesystems

Due to licensing issues, OpenZFS cannot be integrated with the Linux
source code directly, so some gymnastics (I don't know the details) is
done to make sure ZFS works on Linux.

However, FreeBSD has no such qualms, so OpenZFS works as an integral
part of FreeBSD where we can do roll back boot environments, and such
advanced operations.

It is unlikely that OpenZFS will ever be integrated into OpenBSD. I
don't know the reasons, but remember reading somewhere that this is
not going to happen. (My ideal system would be a combination of the
best parts of FreeBSD, DragonflyBSD and OpenBSD).

There has been some discussion on porting DragonflyBSD's HAMMER
filesystem to BSD. Indeed, some effort has taken place to support
read-only support of HAMMER filesystems from OpenBSD. This is likely
to be the modern filesystem we can realistically hope to use in
OpenBSD someday.

### Ports system

There are ports for software for which official packages are not
available. That's the natural place to search in before resorting to
good old installation from source.

### Building OpenBSD itself

One of the reasons I wanted to use a simpler OS was the wish someday
that someday, I could make meaningful contributions to the community
in the form of code as well.

I heard someone once say "OpenBSD's premier use case is developing
OpenBSD itself." Many other use cases can a bit roundabout, and
occasionally frustrating as mentioned.

I attempted downloading the source code from the version control. I
had no idea how painful it would be.

#### CVS

Unfortunately, for historical reasons, OpenBSD uses CVS for version
control. CVS is deeply integrated into the OpenBSD workflows, so it is
unlikely to change any time soon.

When I tried downloading the source code using CVS, it was downloading
files one by one (instead of a downloading data and generating the
files from it like how Git does) and it took a long time, perhaps an
hour. It was not a nice feeling.

## Desktop Usage

This is one of the best things I like about OpenBSD. We can go to zero
to usable with very minimal configuration.

### Custom Keybindings

I make use of the Hyper and Super keys to help with shortening long
key chords in Emacs. So instead of having to do something like `C-M-k`
i.e. (Control+Alt+k), I can rebind that to `H-k` (`Hyper+k`) or `s-k`
(`Super+k`). This eases the strain on my already burdened fingers.

We'll need to mess around with XKB to get this working how we want it.

I DO NOT know how to use XKB, so all this was done with a lot of error
prone hackery. It just about works for my needs, so a BIG caveat
emptor.

#### Keycodes file `~/.xkb/keycodes/local`

```
xkb_keycodes {
  <SUPR> = 117;
};
```

#### Keymap files

##### `~/.xkb/keymap/hypersuper`

```
xkb_keymap {
        xkb_keycodes  { include "xfree86+aliases(qwerty)"       };
        xkb_types     { include "complete"      };
        xkb_compat    { include "complete"      };
        xkb_symbols   { include "pc+us+inet(pc105)+terminate(ctrl_alt_bksp)+hyperswitch(swap)"  };
        xkb_geometry  { include "pc(pc105)"     };
};
```

##### `~/.xkb/keymap/reset`

To reset, if ever I need to do that.

```
xkb_keymap {
        xkb_keycodes  { include "xfree86+aliases(qwerty)"       };
        xkb_types     { include "complete"      };
        xkb_compat    { include "complete"      };
        xkb_symbols   { include "pc+us+inet(pc105)+terminate(ctrl_alt_bksp)"    };
        xkb_geometry  { include "pc(pc105)"     };
};
```
#### Symbols file `~/.xkb/symbols/hyperswitch`

```
partial modifier_keys
xkb_symbols "swap" {
   key <RWIN> { [ Hyper_R, Hyper_L ] };
   key <RALT> { [ Super_L, Super_R ] };
   modifier_map Mod3 { Hyper_L, Hyper_R };
   modifier_map Mod1 { Alt_L, Meta_L };
};
```

### Display Server

OpenBSD has its own fork of X called Xenocara. It works fine, however
this might be disappointing for those who want to use Wayland. Wayland
is not fully supported as yet, but seems some action is taking place
[here](https://github.com/openbsd/ports/tree/master/wayland).

### Window Manager

The first thing to configure will be
[cwm](https://man.openbsd.org/cwm) which is one of the window managers
available in base. [fvwm](https://man.openbsd.org/fvwm) also exists,
but I liked cwm better.

Here's my cwm config. It's a slightly modified version of the cwm
config found
[here](https://www.c0ffee.net/blog/openbsd-on-a-laptop#cwm).

```
sticky yes

snapdist 4

unbind-key all

command alacritty "/home/samuel/.cargo/bin/alacritty"
command treesheets "/usr/local/bin/treesheets"
command lagrange "/home/samuel/src/lagrange-build/lagrange"
command firefox firefox
command emacs emacs

bind-key 4-Return "/home/samuel/.cargo/bin/alacritty"
# bind-key M-Backspace terminal

# bind-key 4-Backspace window-hide

bind-key 4-1 group-only-1
bind-key 4-2 group-only-2
bind-key 4-3 group-only-3
bind-key 4-4 group-only-4
bind-key 4-5 group-only-5
bind-key 4-6 group-only-6
bind-key 4-7 group-only-7
bind-key 4-8 group-only-8
bind-key 4-9 group-only-9

# mod + shift +$N = move window to group $N
bind-key 4S-1 window-movetogroup-1
bind-key 4S-2 window-movetogroup-2
bind-key 4S-3 window-movetogroup-3
bind-key 4S-4 window-movetogroup-4
bind-key 4S-5 window-movetogroup-5
bind-key 4S-6 window-movetogroup-6
bind-key 4S-7 window-movetogroup-7
bind-key 4S-8 window-movetogroup-8
bind-key 4S-9 window-movetogroup-9

bind-key 4-f window-fullscreen
bind-key 4-w menu-window
bind-key 4-r menu-cmd
bind-key 4-e menu-exec
# bind-key 4-s menu-ssh

bind-key 4S-r restart
bind-key 4S-e quit

bind-key CM-x window-close

bind-key 4-Tab window-cycle
bind-key 4S-Tab window-rcycle

unbind-mouse CM-1
unbind-mouse M-2
unbind-mouse M-3
unbind-mouse CMS-3

# mod + left click drag = move window
bind-mouse 4-1 window-move
bind-mouse M-1 window-move
# mod + right click drag = resize window
bind-mouse 4-2 window-resize
bind-mouse M-2 window-resize
# mod + middle click = lower window's focus
bind-mouse 4-3 window-lower
bind-mouse M-3 window-lower
# mod + shift + middle click = hide window
bind-mouse 4S-2 window-hide
```

### Terminal Emulator

[Alacritty](https://alacritty.org/) works well.

### Web Browsers

Firefox and Chrome work fine.

One notable difference in how they work is, that by default Firefox
will only have access to the Downloads directory and no other
directory on your system. It's a useful security feature, but
something to keep in mind.

### Text Editors

#### Emacs

A package is available for vanilla Emacs. If we want the
bleeding-edge JIT compiled Emacs, we're in for a rough ride. I've
perceived some slowness with things like magit. Not sure what could be
the cause of that.

##### JIT Compiled Emacs

Any 'serious' emacs user now, would want to squeeze every bit of
performance available by using the [JIT compiled
Emacs](https://akrl.sdf.org/gccemacs.html).

JIT compiled emacs does not work without an extremely complicated set
of steps that I have unfortunately **not** succeeded in executing.

The issue comes because we need `libgccjit`. We may think that we
can **just** download the source code of GCC and try and build
it. When I attempted that I realised that the 'GCC' that runs on
OpenBSD has some extra patches on it and the vanilla GCC code does not
build the same way it would build on a Linux machine.

[Omar Polo](https://www.omarpolo.com/) was an absolute champ and
figured this out as we can see on this
[thread](https://fantastic.earth/@samebchase/111340712771679079). If
we're feeling brave, we can try it out.

#### NeoVim

The [NeoVim](https://neovim.io/) package is available and works well.

#### Helix

I've had some issues building [Helix](https://helix-editor.com/) from
source, but the one installed from the packages works well.

### Development

#### Programming Languages

I am going to be mentioning languages that I have tried out. Apologies
for not mentioning any language that you like. (Feel free to contribute).

##### Raku

The 'rakudo' package is outdated, so would recommend installing a more
recent one using [rakubrew](https://rakubrew.org/). `rakubrew` is
fortunately a Perl program, and Perl is well supported on OpenBSD as
many of the system tools are written in it.

I did run into issues building Rakudo, because of the default
configuration of `login.conf` as [this
article](https://blog.lambda.cx/posts/openbsd-compiling-rakudo-star/)
helped me with what I needed to do.

##### Common Lisp

We can install [SBCL](https://www.sbcl.org/) from the packages,
install [Quicklisp](https://www.quicklisp.org/beta/), and we should
be good to go.

##### Clojure

Clojure works well.

##### Elixir

I was able to install Elixir (and Erlang) using
[asdf](https://asdf-vm.com/).

##### Go

Go works well. I have done proper Go development on OpenBSD. The
notable exception is that the
[delve](https://github.com/go-delve/delve/issues/1477) debugger does
not work on OpenBSD. I got around that by setting up a Linux VM,
rsync'ing the code there every time I needed to run the debugger. It's
a bit ugly, but there's no other option.

##### Rust

Trying to setup a Rust programming environment turned out to be the
biggest disappointment.

`rustc` and `cargo` can be installed from the packages, but for
someone doing serious Rust development, they would require all the
tooling which in many cases depends on nightly toolchain. The nightly
toolchain requires `rustup` which [does not
work](https://github.com/rust-lang/rustup/issues/2168) on
OpenBSD. This is a massive pain point.

Now, I'm not sure what can be done here, because it requires some work
including changes to
[llvm/libc](https://github.com/rust-lang/rustup/issues/2168).

It will help OpenBSD's adoption if Rust aficionados can reasonably
develop Rust software on machines running OpenBSD.

Over the past few months, this has actively come in the way of me
enthusiastically wanting to learn Rust. It might seem dumb, but I put
off learning Rust because it does not work well on my machine, and I
don't have another fast enough computer where I could install Linux or
whatever.

The obvious workaround here is to set up a working Rust environment in
a Linux VM, but with the limitation of a single core, compiling crates
is going to be slower than it should be, leading to a frustrating
second-class experience.

It's extremely disappointing that I will essentially need to be on a
different OS if I wanna do serious development on Rust.

##### C/C++

Clang++ and GCC work well enough. By default `cc` and `c++` point to
the LLVM compilers. When GCC is installed, they will need to be called
by `egcc` and `eg++`.

#### Containerization

Containerization and/or container orchestration tools (like Docker,
Podman, Kubernetes) will not work. You must work around this by
getting **very** familiar with building and running software from
source when official packages are not available.

One way we can work around this is to have Linux VMs running, but as
mentioned earlier, each of these VMs have the restriction of only
being able to use one CPU core.

Unfortunately, this also affects OpenBSD's ability to be used as an OS
for doing mainstream commercial software development where things like
Docker, Kubernetes are pre-requisites.

#### Databases

##### PostgreSQL

This is present in the official packages and works well.

###### Running your own version

I have managed to follow the steps given in [Building PostgreSQL 11
From Source
Code](https://dev.to/nabbisen/building-postgresql-11-from-source-code-53g3)
can be followed to build PG from source and to start the DB service.

##### SQLite

SQLite should work well enough because I use Fossil regularly. Not
sure how the DB performance compares, but it works.

### Document Preparation

#### [Kile](https://kile.sourceforge.io/)

I've used [AUCTeX](https://www.gnu.org/software/auctex/) in the past,
but for whatever reason I couldn't be bothered figuring it out why it
wasn't working. Ended up using Kile which worked beautifully.

### Multimedia

#### Image Editing

The [GIMP](https://www.gimp.org/) and
[Inkscape](https://inkscape.org/) work well.

#### Audio

Audio playback suffers from artifacts. Depending on your audio card
and the driver in use, weird things happen.

When I use my 3.5mm jack, one driver is in use and the audio works for
about ten minutes, and then there's just silence. I can see the video,
but I cannot hear anything.

To work around this, I got a 3.5mm to USB interface and connected that
into the USB port. I still do get the weird artifacts/stutters which
nixes the possibility of enjoying music because it messes with the
timing of the song and the audible artifact itself is unpleasant
sounding. Watching tech talks is just about bearable, so that's okay.

##### Full Duplex Audio

OpenBSD does not support full duplex audio. This essentially means,
we cannot mix a stream of audio where one device is recording and
another is playing audio. We can **only** do one at a time.

This is one of my biggest gripes with OpenBSD.

#### Video

I have an AMD GPU. There is tearing when I watch a video, which I get
around by setting the `TearFree` option.

```
xrandr --output DisplayPort-1 --set TearFree on
```

Please note run `xrandr` which will list out all the available video
outputs (`DisplayPort-1` in my case).


[ffmpeg](https://ffmpeg.org/) and [VLC](https://www.videolan.org/vlc/)
work well.

##### Video conferencing

Webcams work after enabling some video recording flags.

Video calls do not work because of the missing support for full duplex
audio mentioned above.

On occasions where I have to share my screen, I share the screen from
OpenBSD, and connect to the video call with audio and a webcam from a
different laptop. It's annoying, but hey, you're an OpenBSD user. ;-)

### Virtualization

There is a homegrown virtualization solution called
[vmm](https://man.openbsd.org/vmd.8)/[vmd](https://man.openbsd.org/vmd.8). This
can be used to run OpenBSD or Linux VMs. I think FreeBSD should work
as well, but I have not tried it.

Linux VMs can be used assuming the bootable ISO/image supports a
serial console (the debian installer notably does not), so in my
experience, Alpine Linux, Arch Linux and Nix OS work well enough.

Some have run old versions of Ubuntu as well, but I wouldn't recommend
running old versions of an OS.

## VPS (VM) Usage

### OpenBSD Amsterdam

I run an OpenBSD VM on (OpenBSD Amsterdam](https://openbsd.amsterdam/)
which is the premier OpenBSD hosting service. They offer OpenBSD VMs
using
[vmm](https://man.openbsd.org/vmd.8)/[vmd](https://man.openbsd.org/vmd.8). 

Their communication is prompt and their service is excellent. There
are other providers, but not sure who else dogfoods and contributes to
OpenBSD at the same degree. I've been a happy customer for years, and
heartily endorse them.

### VPS operations

There are some advantages to be had where one's daily driver OS is
also the OS of a remotely running machine. We don't need to worry
about mild paper cut differences in command line syntax, available
base tools and it's easy to experiment with changes on the local
system first instead of having to run yet another VM.

### Self hosting

#### Webpages

I had originally set up webpages using [relayd]() and [httpd]()
following this the guide [How to setup a web server with
OpenBSD](https://www.bsdhowto.ch/webserver.html).

It covers setting up HTTPS using Let's Encrypt which is not a
straightforward topic/process. TLS termination happens at relayd (the
loadbalancer), and it forwards the request to the underlying services.

However, when I started self hosting an IRC client (see below) on the
VPS, I needed the equivalent of NGINX's `proxy_pass` which was not
supported on httpd. I started using NGINX.

#### Gemini Capsule

I chose [vger](https://tildegit.org/solene/vger) which was a
lightweight Gemini server. I want to thank
[Solène](https://dataswamp.org/~solene/) for assisting me with
debugging my issues and setting up my Gemini capsule. There was a
situation where she needed to make a small modification to vger which
she was kind enough to do. 

The setup instructions can be found in the vger docs. I didn't do
anything different.

#### IRC

I've been using irssi and weechat on remote machines for
years. However, after getting used to modern IRC knockoffs, I wanted
the GUI conveniences of being able to easily type emoji, copy paste.

I self host [The Lounge](https://thelounge.chat/) and it has worked
well enough for an extended period of time. I was concerned that maybe
this would not build/run on OpenBSD, but was pleasantly surprised when
it worked.

#### Fossil Wiki

I had a difficult time setting this up on the VPS because I was new to
**both** OpenBSD **and** [Fossil](https://fossil-scm.org/).

After a lot of frustrating attempts, the final magic incantation
turned out to be:

```
fossil server <FOSSIL-FILE> --scgi --localhost --port <PORT> --https --jsmode bundled &
```

There is some NGINX configuration as well which looks something like:

```
location ^~ /wiki/ {
        include scgi_params;
        scgi_param SCRIPT_NAME "/wiki";
        scgi_pass localhost:<HOST>;
}
```

#### Future plans

Source code hosting, calendar,
[email](https://www.c0ffee.net/blog/mail-server-guide), Wireguard VPN
etc.

Will add those sections as I get around to doing them.

## Conclusion

### My ideal computing setup

I list some desirable qualities of my fantasy desktop, and the OSes
that are known for it.

- Fast redundant storage using OpenZFS (Linux, FreeBSD).
- Good overall performance (Linux, FreeBSD)
- The ability to perform multimedia tasks like video calls,
  live-streaming, editing video, mixing/editing audio. (Linux,
  proprietary OSes).
- Light weight base system and excellent documentation (OpenBSD)
- Extremely stable across upgrades, reliable hibernate. (OpenBSD)
- Good gaming support. (Linux, proprietary OSes)
- Secure and conservative by default. (OpenBSD)
- A good development environment. (Linux, FreeBSD)
- Declarative dependency management. (NixOS, Guix)
- The ability to feel smugly superior. (OpenBSD, ArchLinux, FreeBSD, NixOS,
  <YOUR-OS-HERE> 😂)

It seems that none of the OSes listed above can entirely
ignored. Every OS is good at a few things and suboptimal at
others. It's a matter of preference and what we want to optimize for.

It seems I would need three computers to fulfill all of the above.

1. OpenBSD for general purpose use on desktop and remote machines.
2. NixOS/Guix machine for the things Linux is good at, and exploring
   declarative dependency management.
3. A redundant OpenZFS NAS running on FreeBSD.

### Conclusion's conclusion

It might seem that all this is a bit too much pain for a working
desktop. For anyone that feels that way, I can strongly recommend a
mainstream Linux distribution which will mostly Just Work™, however if
you want to sidestep the monoculture and explore some offbeat paths,
OpenBSD is a good starting point.

Having multiple solid desktop OSes with thriving communities is a good
thing.

I would like to add a note of heartfelt gratitude to all the OpenBSD
developers and community members who have built all this and helped me
along the journey. Thank you!

## References 

Some absolutely awesome OpenBSD resources.

1. [Roman Zolotarev's website](https://romanzolotarev.com/)
2. [Bruno Flückiger's massively helpful BSD How To](https://www.bsdhowto.ch/)
3. [Solène Rapenne's website](https://dataswamp.org/~solene/)

## P.S. You can hire me!

I've been laid off since Oct 2023. If you think I would be a good fit
for an opportunity, would be glad to hear from you. You can email me
at samebchase@gmail.com


