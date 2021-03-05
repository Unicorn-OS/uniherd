# UNIHERD Xirtus Microkernel (powered by UNICORN)
UNIHERD: GNU HERD TokenRTOS FORK WITH BOINC INTEGRATION and Xirtus Exodriver management system

GNU Hurd is the multiserver microkernel written as part of GNU. It has been under development since 1990 by the GNU Project of the Free Software Foundation, designed as a replacement for the Unix kernel and released as free software under the GNU General Public License. When the Linux kernel proved to be a viable solution, development of GNU Hurd slowed, at times having slipped intermittently between stasis and renewed activity and interest.

The Hurd's design consists of a set of protocols and server processes (or daemons, in Unix terminology) that run on the GNU Mach microkernel. The Hurd aims to surpass the Unix kernel in functionality, security, and stability, while remaining largely compatible with it. The GNU Project chose the multiserver microkerne for the operating system, due to perceived advantages over the traditional Unix monolithic kernel architecture, a view that had been advocated by some developers in the 1980s

In December 1991 the primary architect of the Hurd described the name as a mutually recursive acronym:

It's time [to] explain the meaning of "Hurd". "Hurd" stands for "Hird of Unix-Replacing Daemons". And, then, "Hird" stands for "Hurd of Interfaces Representing Depth". We have here, to my knowledge, the first software to be named by a pair of mutually recursive acronyms.

— Thomas (then Michael) Bushnell
As both hurd and hird are homophones of the English word herd, the full name GNU Hurd is also a play on the words herd of gnus, reflecting how the kernel works.

The logo is called the Hurd boxes and it also reflects on architecture. The logo is a graph where nodes represent the Hurd kernel's servers and directed edges are IPC messages.

Debian Resources
Official page about the Debian GNU/Hurd port: Debian GNU/Hurd
Debian FAQ — Frequently Asked Questions
QEMU Image
There is a QEMU image with Debian GNU/Hurd pre-installed available as https://cdimage.debian.org/cdimage/ports/latest/hurd-i386/debian-hurd.img.tar.gz.

Usage:

$ wget https://cdimage.debian.org/cdimage/ports/latest/hurd-i386/debian-hurd.img.tar.gz
$ tar -xz < debian-hurd.img.tar.gz
$ kvm -m 1G -drive cache=writeback,file=$(echo debian-hurd-*.img)
Please also read the README file: https://cdimage.debian.org/cdimage/ports/latest/hurd-i386/README

If you have troubles extracting the image, you can use the gz version, the zip version, or even the plain version (5GiB!)

See the discussion about writeback caching.

Just in case you were wondering: the root password is empty.

For more detailed instructions, please see the QEMU page.

Installing
Installation Instructions
Upgrading K11 or K14 based systems to unstable
After install — Do this to get networking, new console and X
Contributing
Porting — Helping with porting packages
Patch submission — How to submit patches for build failures
Creating image tarball
Additional Information
Presentation Debian GNU/Hurd, ?MichaelBanck, LinuxTag 2004 Karlsruhe
Status
Archive Qualification
/etc/mtab -> /proc/mounts
IRC, freenode, #hurd, 2014-02-12
<braunr> hm, there is something weird
<braunr> after successfully installing (with the new installer cd), and
  rebooting, system init fails because fsck can't be run on /home (a
  separate partition)
<braunr> it can't fsck because at that point, /home is already mounted, and
  indeed the translator is running
<braunr> teythoon: any idea what might cause that ?
<teythoon> me ?
<teythoon> no
<braunr> ok
<braunr> ah no, actually /home isn't mounted oO
<braunr> but fsck still refuses to check it, stating that reason
<braunr> hm, /etc/mtab isn't a link to /proc/mounts here, might explain
IRC, freenode, #hurd, 2014-02-12
<braunr> yes, better with a proper symlink :)
<teythoon> good
<youpi> Mmm, what is supposed to create that symlink?
<teythoon> one debian init script did that at one time
<teythoon> i believe they dropped that
<youpi> err, but something must be creating it for newer systems
<teythoon> good point
<braunr> well, except for these small details, everything went pretty
  smooth
<braunr> both on ide and ahci
<youpi> it seems /etc/mtab gets created at boot
<youpi> (on Linux I mean)
<teythoon> youpi: i cannot find the init script, but i'm sure that it was
  there
<youpi> I can't find it either on the installed system...
<azeem> maybe pere or rleigh in #debian-hurd can help
IRC, freenode, #hurd, 2014-02-13
<braunr>   6<--60(pid1698)->dir_lookup ("var/run/mtab" 4194305 0) = 0 3
  "/run/mtab"  (null)
<braunr> looks like /etc/mtab isn't actually used anymore
<teythoon> it never was on hurd
<tomodach1> braunr: well it is generated i believe from mounted filesystems
<tomodach1> if its still around there is a reason for it, like posix
  compatiblity perhaps?
<braunr> well the problem is that, as mentioned in pere's thread on
  bug-hurd, some tools now expect /var/run/mtab instead of /etc/mtab
<braunr> and since nothing currently creates this file, these tools, such
  as df, are lost
<braunr> they can't find the info they're looking for
IRC, freenode, #hurd, 2014-02-17
<braunr> i still don't have mtab at the proper location on darnassus
<pere> is there something missing with sysvinit on hurd?
<braunr> is that normal ?
<pere> yes.  I recommended fixing it in the hurd package. (BTS #737759)
<braunr> yes i saw but was there any action taken ?
<pere> did not check
<teythoon> i thought youpi mentioned that it is fixed in the libc and we
  just need to rebuild coreutils or something
<pere> yes
<braunr> oh ok
<braunr> but doesn't that mean it will use /etc/mtab ?
<pere> if I was a hurd porter, I would fix it in hurd while waiting for a
  fix in coreutils, just to save people for wondering about the breakage,
  but I am not the most patient of developers. :)
