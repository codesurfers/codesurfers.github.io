# [WIP]: Using OpenBSD as a daily driver

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

As an aside, if you are interested in running FreeBSD, some guides
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
remember. Within around fifteen minutes or so you should have a
machine that boots into OpenBSD.

## Maintenance

### Hibernate

Hibernate works well on OpenBSD most times. Every once in a while, I
have noticed that attempting to unhibernate results in a fresh boot. I
have not been able to investigate or isolate what situations causes it
to do that.

I almost never turn off my desktop. I hibernate every time, because
it's nice to not have to restart all the programs you need in the way
you left them last.

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

FWIU, you have to use AMD cards as they have open source drivers. Not
sure how NVIDIA cards will fare on OpenBSD.

#### Wireless

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
(e.g. like a NAS) is not recommended. You would be better served with
the other filesystems commonly used for such purposes.

#### Integrating other filesystems

Due to licensing issues, OpenZFS cannot be integrated with the Linux
source code directly, so some gymnastics (I don't know the details) is
done to make sure ZFS works on Linux.

However, FreeBSD has no such qualms, so OpenZFS works as an integral
part of FreeBSD where you can do roll back boot environments, and such
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

## Desktop Usage

From the base install, we can quickly make it into a usable state
without much configuration.

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

### Web Browsers

### Text Editors

#### Emacs

A package is available for vanilla Emacs. If you want the
bleeding-edge JIT compiled Emacs, you're in for a rough ride.

##### JIT Compiled Emacs

JIT compiled emacs does not work without an extremely complicated set
of steps that I have unfortunately not succeeded in executing.

The issue comes because we need `libgccjit`. You may think that you
can **just** download the source code of GCC and try and build
it. When I attempted that I realised that the 'GCC' that runs on
OpenBSD has some extra patches on it and the vanilla GCC code does not
build the same way it would build on a Linux machine.

[Omar Polo](https://www.omarpolo.com/) was an absolute champ and
figured this out as you can see on this
[thread](https://fantastic.earth/@samebchase/111340712771679079). If
you're feeling brave, you can try it out.

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
recent one using [rakubrew](https://rakubrew.org/). `rakubrew` is a
Perl program, and fortunately, Perl is an important language on an
OpenBSD system as many of the system tools are written in it.

I did run into issues building Rakudo, because of the default
configuration of `login.conf` as [this
article](https://blog.lambda.cx/posts/openbsd-compiling-rakudo-star/)
helped me with what I needed to do.

##### Common Lisp

You can install [SBCL](https://www.sbcl.org/) from the packages,
install [Quicklisp](https://www.quicklisp.org/beta/), and you should
be good to go.

##### Clojure

Once you have Java installed, you can get
[Leiningen](https://leiningen.org/) and it should work okay.

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

`rustc` can be installed in the packages, but for someone doing
'serious' Rust development, they would require all the tooling which
in many cases depends on nightly which requires `rustup` which [does
not work](https://github.com/rust-lang/rustup/issues/2168) on OpenBSD.

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

##### C/C++

Clang++ and GCC work well enough. By default `cc` and `c++` point to
the LLVM compilers. When GCC is installed, they will need to be called
by `egcc` and `eg++`.

#### Containerization

Containerization and/or container orchestration tools (like Docker,
Podman, Kubernetes) will not work. You must work around this by
getting **very** familiar with building and running software from
source when official packages are not available.

One way you can work around this is to have Linux VMs running, but as
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

SQLite should work 

### Document Preparation

#### Kile

### Multimedia

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
you cannot mix a stream of audio where one device is recording and
another is playing audio. You can **only** do one at a time.

This is my biggest gripe with OpenBSD.

#### Video

I have an AMD GPU. There is tearing when I watch a video, which I get
around by setting the `TearFree` option.

```
xrandr --output DisplayPort-1 --set TearFree on
```

##### Video conferencing

Webcams work after enabling some video recording flags.

```

```

Video calls do not work because of the missing support for full duplex
audio mentioned above.

### Virtualization

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
also the OS of a remotely running machine. You don't need to worry
about mild paper cut differences in command line syntax, available
base tools and it's easy to experiment with changes on the local
system first instead of having to run yet another VM.

### Self hosting

#### Webpages

#### Gemini Capsule

#### IRC

#### Fossil Wiki

#### Future plans

Source code hosting, calendar,
[email](https://www.c0ffee.net/blog/mail-server-guide), Wireguard VPN
etc.

## Conclusion

It might seem that this is a bit too much pain for a working
desktop. To anyone that feels that way, I can strongly recommend a
mainstream Linux distribution which will mostly Just Work™, however if
you want to sidestep the monoculture and explore some offbeat paths,
OpenBSD is a good starting point.

## References 

Absolutely awesome OpenBSD resources.

1. [Roman Zolotarev's website](https://romanzolotarev.com/)
2. [Bruno Flückiger's massively helpful BSD How To](https://www.bsdhowto.ch/)
3. [Solène Rapenne's freaking awesome website](https://dataswamp.org/~solene/)

## P.S. You can hire me!

I've been laid off since Oct 2023. If you think I would be a good fit
for an opportunity, would be glad to hear from you. You can email me
at samebchase@gmail.com


