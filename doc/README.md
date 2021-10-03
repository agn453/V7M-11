# Installation of V7M-11 V1.0 from RX50 Micro/PDP-11 distribution

## Introduction

Unix V7M-11 V1.0 has successfully been installed using SIMH pdp11 configured
as a Q-Bus PDP-11/23-Plus with RQDX1 and 2xRD51, 2xRX50 drives,
a RLV12 and 4xRL02, dual DLV11-J serial ports (one is the console),
and DZV11 quad four-line async serial ports.

## Prerequisites

Firstly you'll need to download the Micro/PDP11 RX50 distribution kit for
Unix V7M-11 V1.0 from
[bitsavers.com](http://www.bitsavers.org/bits/DEC/pdp11/floppyimages/rx50/V7M-11-V1.0_6_USR_RX50-QJ083-H3.zip)
and set-up a running version of SIMH pdp11 from https://github.com/simh/simh

The SIMH pdp11 start-up file is as follows (and available as
[v7m.ini](https://raw.githubusercontent.com/agn453/V7M-11/master/v7m.ini)
from this site).

```
; Unix V7M-11 V1.0
;
; Running on a Micro/PDP-11/23+ system
; with two RD51 disks, two RX50 floppy drives,
; four RL02 drives, a 4-port DZV11, line printer and
; second serial DL11 terminal.
;
set cpu 11/23+, 512K
set cpu idle
set nothrottle
show cpu

; Disable unused devices
set rom disabled
set ptr disabled
set ptp disabled
set rk disabled
set rp disable
set rq disabled
set tm disabled
set tq disabled
set xq disabled
set hk disabled
set rx disabled

; Line clock at 17777546 vec 100
set clk 60Hz
show clk

; Line printer at 17777514 vec 200
attach lpt printer.txt
show lpt

; RLV12 cartridges (RL02) at 17774400 vec 160
set rl enabled
set rl0 enabled
set rl0 RL02
attach rl0 rl02-disk0.dsk
set rl1 enabled
set rl1 RL02
attach rl1 rl02-disk1.dsk
set rl2 enabled
set rl2 RL02
attach rl2 rl02-disk2.dsk
set rl3 enabled
set rl3 RL02
attach rl3 rl02-disk3.dsk

; Terminals (4xVT100 on DZV11 at 9600 bps) at 17760100 vec 310
set dz lines=4
set dz 7b
attach dz 127.0.0.1:2040
show dz

; Additional Terminal (1xVT100 on DLV11) at 17776500 vec 300
set dli enable
set dli lines=1
set dlo enable
set dlo lines=1
set dlo nodataset
set dlo0 7b
attach dli 127.0.0.1:2041
show dli
show dlo

; RD51 disks and RX50 floppies on RQDX3 controller at 17772150
; (SIMH doesn't support the RQDX1 early model controller)
set rq enabled
set rq RQDX3
; System root (/dev/rd0)
set rq0 RD51
att rq0 rd51-v7msys.dsk
; Usr device (/dev/rd1)
set rq1 RD51
att rq1 rd51-v7musr.dsk
; BOOT floppy (/dev/rx2)
set rq2 RX50
att -r rq2 V7M-11_V1.0_6_USR_RX50_01of33_BOOT_BL-X306A-BC.IMG
; Installation kit on other floppy drive (/dev/rx3)
set rq3 RX50
att -r rq3 V7M-11_V1.0_6_USR_RX50_02of33_ROOT_Required_Binaries_1of6_BL-X307A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_03of33_ROOT_Required_Binaries_2of6_BL-X308A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_04of33_ROOT_Required_Binaries_3of6_BL-X309A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_05of33_ROOT_Required_Binaries_4of6_BL-X310A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_06of33_ROOT_Required_Binaries_5of6_BL-X311A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_07of33_ROOT_Required_Binaries_6of6_BL-X312A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_08of33_USEP_BL-X313A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_09of33_SYSGEN_1of2_BL-X314A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_10of33_SYSGEN_2of2_BL-X315A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_11of33_DICTIONARY_BL-X316A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_12of33_OPTIONAL_BINARIES_1of3_BL-X317A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_13of33_OPTIONAL_BINARIES_2of3_BL-X318A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_14of33_OPTIONAL_BINARIES_3of3_BL-X319A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_15of33_OPTIONAL_LIBRARIES_1of4_BL-X320A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_16of33_OPTIONAL_LIBRARIES_2of4_BL-X321A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_17of33_OPTIONAL_LIBRARIES_3of4_BL-X322A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_18of33_OPTIONAL_LIBRARIES_4of4_BL-X323A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_19of33_LEARN_1of4_BL-X324A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_20of33_LEARN_2of4_BL-X325A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_21of33_LEARN_3of4_BL-X326A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_22of33_LEARN_4of4_BL-X327A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_23of33_ON-LINE_MANUALS_1of4_BL-X328A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_24of33_ON-LINE_MANUALS_2of4_BL-X329A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_25of33_ON-LINE_MANUALS_3of4_BL-X330A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_26of33_ON-LINE_MANUALS_4of4_BL-X331A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_27of33_DOCUMENTS_1of7_BL-X332A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_28of33_DOCUMENTS_2of7_BL-X333A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_29of33_DOCUMENTS_3of7_BL-X334A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_30of33_DOCUMENTS_4of7_BL-X335A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_31of33_DOCUMENTS_5of7_BL-X336A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_32of33_DOCUMENTS_6of7_BL-X795A-BC.IMG
;att -r rq3 V7M-11_V1.0_6_USR_RX50_33of33_DOCUMENTS_7of7_BL-Y665A-BC.IMG
show rq

echo To boot the BOOT floppy
echo boot rq2
echo
echo  #boot
echo  Boot
echo  : rx(2,0)restor		to run stand-alone restor
echo  : rx(2,0)mkfs		to make a new filesystem
echo
echo To boot the RD51 (/ on rq0, /usr on rq1)
echo boot rq0
echo
echo  #boot
echo  Boot
echo  : rd(0,0)unix		to boot the Unix V7M kernel
echo
```

As you can see, the RX50 boot floppy is on rq2 and the first
volume of the Required Binaries dump file is on rq3.  The
destination volumes are the RD51 disks on rq0 and rq1.

Pre-built disk images are available for download from
[disk-images/rd51-v7msys.dsk.gz](https://raw.githubusercontent.com/agn453/V7M-11/master/disk-images/rd51-v7msys.dsk.gz)
and
[disk-images/rd51-v7musr.dsk.gz](https://raw.githubusercontent.com/agn453/V7M-11/master/disk-images/rd51-v7musr.dsk.gz)
if you would rather skip the fun of installing this yourself! Just download
them and uncompress into the same directory as the v7m.ini file.

I'll be setting up the system volume RD51 on rq0, and making the
second RD51 on rq1 be the timesharing /usr volume mount point.

This frees up space on the system volume for a small number
of home directories (or you can use the RL02 drives as user
space).

Under V7M-11, these drives correspond to

| SIMH device | Unix device | Boot loader name |
| --- | --- | --- |
| rq0 | /dev/rd0 | RD(0,0) |
| rq1 | /dev/rd1 | RD(1,0) |
| rq2 | /dev/rx2 | RX(2,0) |
| rq3 | /dev/rx3 | RX(3,0) |
| rl0 | /dev/rl0 | RL(0,0) |
| rl1 | /dev/rl1 | RL(0,1) |
| rl2 | /dev/rl2 | RL(0,2) |
| rl3 | /dev/rl3 | RL(0,3) |

## Installation

The system device has a fixed partition layout.  For an RD51
this is hard-coded in the device driver.

| | Start block # | Blocks |
| --- | --- | --- |
| / | 0 | 18840 |
| unused | 18840 | 40 |
| error | 18880 | 40 |
| swap | 18920 | 2648 |
| unused | 21568 | 32 |

For the second RD51 drive, we're using

| | Start block # | Blocks |
| --- | --- | --- |
| /usr | 0 | 21568 |
| unused | 21568 | 32 |

(where the last 32 blocks are assigned for a "maintenance area").

All four RL02 disks can use their entire 20480 blocks.

Start the SIMH pdp11 simulator with the above configuration.

```
simh tony$ pdp11 v7m.ini
```

The first time you run this you may be prompted as the disk images
for the RL02 drives are created.  Answer Y to the each of the
"Overwrite last track? [N]" prompts.

To initialise the file systems, boot the BOOT floppy and load
the standalone mkfs program -

```
sim> b rq2
#boot

Boot
: rx(2,0)mkfs
file sys size: 18840
disk type: rd51
processor type: 23
file system: rd(0,0)
isize = 6024
m/n = 1 72
Exit called

Boot
: rx(2,0)mkfs
file sys size: 21568
disk type: rd51
processor type: 23
file system: rd(1,0)
isize = 6896
m/n = 1 72
Exit called

Boot
: rx(2,0)restor
Tape? rx(3,0)
Starting volume number <1> ? 1
Disk? rd(0,0)
Last chance before scribbling on disk. 
Mount volume 2 <type return when ready>
```

Now type CTRL-E to get back to the SIMH pdp11 sim> prompt
and attach the next disk volume for the dump on rq3

```
Simulation stopped, PC: 026060 (BIT #200,@44716)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_03
```

(hit the TAB key after typing the volume 03 and the
rest of the filename will auto-complete, then press Return).
Then continue (press C and the Return key) and press the Return
key again.

You will see

```
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_03of33_ROOT_Required_Binaries_2of6_BL-X308A-BC.IMG 
RQ3: Unit is read only
sim> c

Mount volume 3 <type return when ready>
```

Press CTRL-E again and repeat for volumes 04 to 07.

(You can save typing filenames by using command-history - press
the up-arrow twice, then delete previous filename characters
back to the volume number, update the the next one and press TAB
then Return and continue)

```
Simulation stopped, PC: 026066 (BEQ 26060)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_04of33_ROOT_Required_Binaries_3of6_BL-X309A-BC.IMG 
RQ3: Unit is read only
sim> c

Mount volume 4 <type return when ready>
Simulation stopped, PC: 026066 (BEQ 26060)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_05of33_ROOT_Required_Binaries_4of6_BL-X310A-BC.IMG 
RQ3: Unit is read only
sim> c

Mount volume 5 <type return when ready>
Simulation stopped, PC: 026060 (BIT #200,@44716)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_06of33_ROOT_Required_Binaries_5of6_BL-X311A-BC.IMG 
RQ3: Unit is read only
sim> c

Mount volume 6 <type return when ready>
Simulation stopped, PC: 026066 (BEQ 26060)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_07of33_ROOT_Required_Binaries_6of6_BL-X312A-BC.IMG 
RQ3: Unit is read only
sim> c

End of tape

Boot
: 
```

You can now boot from the minimal system you've just restored the first RD51
device and write the system boot loader using -

```
: rd(0,0)unix


V7M-11 V1.0

realmem = 524288
usermem = 448256
erase = delete, kill = ^U, intr = ^C
# date 9310030140
Sun Oct  3 01:40:00 EDT 1993
# cd /dev
# make rd51
rm -f *rx0* >/dev/null
/etc/csf -r rd
rm -f swap >/dev/null
/etc/csf rd0
mv rd07 rd0
mv rrd07 rrd0
ln rd0 swap
cp /etc/fstab.rd51 /etc/fstab
dd if=/mdec/rdrxuboot of=/dev/rrd0 count=1
0+1 records in
0+1 records out
# 
```

Also create devices for the other disk drives

```
# csf rd1
# mv rd17 rd1
# mv rrd17 rrd1
# csf rd2
# mv rd27 rx2
# mv rrd27 rrx2
# csf rd3
# mv rd37 rx3
# mv rrd37 rrx3
# 
```

The minimal system has some files in the /usr heirarchy.  We're
now going to copy them across to the second RD51 drive.

```
# cd /
# mount /dev/rd1 /mnt
# cd /usr
# tar cf - . | (cd /mnt; tar xvf -)
x ./adm/.dummy, 23 bytes, 1 tape blocks
x ./adm/messages, 0 bytes, 0 tape blocks
x ./adm/msgbuf, 0 bytes, 0 tape blocks
x ./adm/wtmp, 40 bytes, 1 tape blocks
x ./adm/culog, 0 bytes, 0 tape blocks
x ./sys/.dummy, 23 bytes, 1 tape blocks
x ./tmp/.dummy, 23 bytes, 1 tape blocks
x ./bin/more, 18006 bytes, 36 tape blocks
x ./bin/lookbib, 279 bytes, 1 tape blocks
x ./bin/uuname, 13880 bytes, 28 tape blocks
x ./bin/uustat, 29066 bytes, 57 tape blocks
x ./bin/uupoll, 27380 bytes, 54 tape blocks
x ./include/sys/acct.h, 821 bytes, 2 tape blocks
x ./include/sys/buf.h, 2671 bytes, 6 tape blocks
x ./include/sys/callo.h, 395 bytes, 1 tape blocks
x ./include/sys/conf.h, 782 bytes, 2 tape blocks
x ./include/sys/dir.h, 95 bytes, 1 tape blocks
x ./include/sys/fblk.h, 63 bytes, 1 tape blocks
x ./include/sys/file.h, 596 bytes, 2 tape blocks
x ./include/sys/filsys.h, 1111 bytes, 3 tape blocks
x ./include/sys/ino.h, 618 bytes, 2 tape blocks
x ./include/sys/inode.h, 2226 bytes, 5 tape blocks
x ./include/sys/map.h, 178 bytes, 1 tape blocks
x ./include/sys/mount.h, 268 bytes, 1 tape blocks
x ./include/sys/mpx.h, 794 bytes, 2 tape blocks
x ./include/sys/mx.h, 2074 bytes, 5 tape blocks
x ./include/sys/param.h, 6390 bytes, 13 tape blocks
x ./include/sys/pk.h, 2203 bytes, 5 tape blocks
x ./include/sys/prim.h, 139 bytes, 1 tape blocks
x ./include/sys/proc.h, 2436 bytes, 5 tape blocks
x ./include/sys/reg.h, 415 bytes, 1 tape blocks
x ./include/sys/seg.h, 636 bytes, 2 tape blocks
x ./include/sys/stat.h, 903 bytes, 2 tape blocks
x ./include/sys/systm.h, 3318 bytes, 7 tape blocks
x ./include/sys/text.h, 742 bytes, 2 tape blocks
x ./include/sys/timeb.h, 140 bytes, 1 tape blocks
x ./include/sys/times.h, 226 bytes, 1 tape blocks
x ./include/sys/tty.h, 5771 bytes, 12 tape blocks
x ./include/sys/types.h, 518 bytes, 2 tape blocks
x ./include/sys/user.h, 5470 bytes, 11 tape blocks
x ./include/sys/pk.p, 1865 bytes, 4 tape blocks
x ./include/sys/fperr.h, 171 bytes, 1 tape blocks
x ./include/sys/param.h.old, 5769 bytes, 12 tape blocks
x ./include/a.out.h, 1330 bytes, 3 tape blocks
x ./include/ar.h, 132 bytes, 1 tape blocks
x ./include/assert.h, 313 bytes, 1 tape blocks
x ./include/core.h, 139 bytes, 1 tape blocks
x ./include/ctype.h, 721 bytes, 2 tape blocks
x ./include/execargs.h, 32 bytes, 1 tape blocks
x ./include/dk.h, 583 bytes, 2 tape blocks
x ./include/dumprestor.h, 564 bytes, 2 tape blocks
x ./include/errno.h, 827 bytes, 2 tape blocks
x ./include/grp.h, 103 bytes, 1 tape blocks
x ./include/ident.h, 34 bytes, 1 tape blocks
x ./include/math.h, 375 bytes, 1 tape blocks
x ./include/olddump.h, 1389 bytes, 3 tape blocks
x ./include/pack.h, 2203 bytes, 5 tape blocks
x ./include/pwd.h, 184 bytes, 1 tape blocks
x ./include/saio.h, 863 bytes, 2 tape blocks
x ./include/setjmp.h, 24 bytes, 1 tape blocks
x ./include/sgtty.h, 3089 bytes, 7 tape blocks
x ./include/signal.h, 796 bytes, 2 tape blocks
x ./include/stdio.h, 870 bytes, 2 tape blocks
x ./include/symbol.h, 112 bytes, 1 tape blocks
x ./include/time.h, 158 bytes, 1 tape blocks
x ./include/tp_defs.h, 162 bytes, 1 tape blocks
x ./include/utmp.h, 114 bytes, 1 tape blocks
x ./include/varargs.h, 189 bytes, 1 tape blocks
x ./include/mp.h, 455 bytes, 1 tape blocks
x ./include/sys.s, 667 bytes, 2 tape blocks
x ./include/whoami.h, 25 bytes, 1 tape blocks
x ./include/curses.h, 4265 bytes, 9 tape blocks
x ./include/ndir.h, 2696 bytes, 6 tape blocks
x ./lib/makekey, 2086 bytes, 5 tape blocks
x ./lib/diffh, 5976 bytes, 12 tape blocks
x ./lib/diff3, 6422 bytes, 13 tape blocks
x ./lib/calendar, 4400 bytes, 9 tape blocks
x ./lib/crontab, 146 bytes, 1 tape blocks
x ./lib/libmp.a, 20786 bytes, 41 tape blocks
x ./lib/cign, 174 bytes, 1 tape blocks
x ./lib/eign, 2287 bytes, 5 tape blocks
x ./lib/llib-lm, 1067 bytes, 3 tape blocks
x ./lib/lint1, 52678 bytes, 103 tape blocks
x ./lib/lint2, 5204 bytes, 11 tape blocks
x ./lib/llib-lc, 3779 bytes, 8 tape blocks
x ./lib/llib-port, 1576 bytes, 4 tape blocks
x ./lib/libdbm.a, 10080 bytes, 20 tape blocks
x ./lib/uucp/.XQTDIR, 0 bytes, 0 tape blocks
x ./lib/ccom, 81720 bytes, 160 tape blocks
x ./lib/lpd, 11390 bytes, 23 tape blocks
x ./lib/tabset/beehive, 164 bytes, 1 tape blocks
x ./lib/tabset/diablo, 68 bytes, 1 tape blocks
x ./lib/tabset/std, 135 bytes, 1 tape blocks
x ./lib/tabset/teleray, 57 bytes, 1 tape blocks
x ./lib/tabset/vt100, 159 bytes, 1 tape blocks
x ./lib/tabset/xerox1720, 164 bytes, 1 tape blocks
x ./lib/more.help, 1102 bytes, 3 tape blocks
x ./lib/libmpov.a, 20786 bytes, 41 tape blocks
x ./lib/libdbmov.a, 10080 bytes, 20 tape blocks
x ./lib/libndir.a, 2290 bytes, 5 tape blocks
x ./pub/ascii, 1056 bytes, 3 tape blocks
x ./pub/eqnchar, 2969 bytes, 6 tape blocks
x ./pub/greek, 475 bytes, 1 tape blocks
x ./spool/mail/.dummy, 23 bytes, 1 tape blocks
x ./spool/secretmail/bin.key, 504 bytes, 1 tape blocks
x ./spool/secretmail/root.key, 502 bytes, 1 tape blocks
x ./spool/secretmail/notice, 25 bytes, 1 tape blocks
x ./spool/secretmail/root.0, 168 bytes, 1 tape blocks
x ./spool/dpd/empty, 0 bytes, 0 tape blocks
x ./spool/at/lasttimedone, 5 bytes, 1 tape blocks
x ./spool/at/past/dummy, 22 bytes, 1 tape blocks
x ./spool/uucp/.dummy, 23 bytes, 1 tape blocks
x ./spool/uucppublic/.dummy, 23 bytes, 1 tape blocks
x ./spool/lpd/.dummy, 23 bytes, 1 tape blocks
# sync 
# umount /dev/rd1
# cd /
# mount /dev/rd1 /usr
#
```

Now the second drive is overlay mounted on the /usr mount point,
add it to the system /etc/fstab file.

```
# cat /etc/fstab
/dev/rd0:/:rw
# cat <<EOF >>/etc/fstab
> /dev/rd1:/usr:rw
> EOF
# 
```

You can now load up the rest of the RX50 volumes in interactive mode.
Press CTRL-D to exit the single user shell, then login as root

```
# ^D

Restricted rights:

        Use, duplication, or disclosure is subject
        to restrictions stated in your contract with
        Digital Equipment Corporation.

*UNIX is a Trademark of Bell Laboratories.

Attempt to mount /dev/rd1 on /usr  FAILED: Mount device busy

Sun Oct  3 01:53:07 EDT 1993

ERROR LOG has - 1 of 40 blocks used

login: root

Welcome to V7M-11 V1.0

erase = delete, kill = ^U, intr = ^C
# df
/dev/rd0 14198
/dev/rd1 19787
# 
```

Installation kit disks 08 to 11 are tar backup format volumes
and should be restored to the root partition /.

Press CTRL-E and attach the next disk (volume 08)

```
# pwd
/
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_08of33_USEP_BL-X313A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./sysxr/sysx, 22128 bytes, 44 tape blocks
x ./sysxr/sysx_!.help, 739 bytes, 2 tape blocks
x ./sysxr/sysx_b.help, 590 bytes, 2 tape blocks
x ./sysxr/sysx_brs.help, 301 bytes, 1 tape blocks
x ./sysxr/sysx_c.help, 1464 bytes, 3 tape blocks
x ./sysxr/sysx_d.help, 379 bytes, 1 tape blocks
x ./sysxr/sysx_dd.help, 428 bytes, 1 tape blocks
x ./sysxr/sysx_dfs.help, 456 bytes, 1 tape blocks
x ./sysxr/sysx_dln.help, 268 bytes, 1 tape blocks
x ./sysxr/sysx_dmm.help, 432 bytes, 1 tape blocks
x ./sysxr/sysx_dt.help, 682 bytes, 2 tape blocks
x ./sysxr/sysx_dun.help, 188 bytes, 1 tape blocks
x ./sysxr/sysx_h.help, 864 bytes, 2 tape blocks
x ./sysxr/sysx_ifs.help, 600 bytes, 2 tape blocks
x ./sysxr/sysx_ios.help, 417 bytes, 1 tape blocks
x ./sysxr/sysx_ism.help, 173 bytes, 1 tape blocks
x ./sysxr/sysx_l.help, 871 bytes, 2 tape blocks
x ./sysxr/sysx_lb.help, 728 bytes, 2 tape blocks
x ./sysxr/sysx_lcp.help, 412 bytes, 1 tape blocks
x ./sysxr/sysx_ld.help, 378 bytes, 1 tape blocks
x ./sysxr/sysx_lf.help, 539 bytes, 2 tape blocks
x ./sysxr/sysx_lnp.help, 547 bytes, 2 tape blocks
x ./sysxr/sysx_ls.help, 920 bytes, 2 tape blocks
x ./sysxr/sysx_msm.help, 218 bytes, 1 tape blocks
x ./sysxr/sysx_mtc.help, 476 bytes, 1 tape blocks
x ./sysxr/sysx_mtf.help, 288 bytes, 1 tape blocks
x ./sysxr/sysx_mun.help, 392 bytes, 1 tape blocks
x ./sysxr/sysx_n.help, 415 bytes, 1 tape blocks
x ./sysxr/sysx_ncr.help, 693 bytes, 2 tape blocks
x ./sysxr/sysx_p.help, 487 bytes, 1 tape blocks
x ./sysxr/sysx_ppt.help, 557 bytes, 2 tape blocks
x ./sysxr/sysx_r.help, 1156 bytes, 3 tape blocks
x ./sysxr/sysx_rhn.help, 910 bytes, 2 tape blocks
x ./sysxr/sysx_s.help, 1112 bytes, 3 tape blocks
x ./sysxr/sysx_sbr.help, 541 bytes, 2 tape blocks
x ./sysxr/sysx_sln.help, 525 bytes, 2 tape blocks
x ./sysxr/sysx_x.help, 484 bytes, 1 tape blocks
x ./sysxr/sysx_xn.help, 216 bytes, 1 tape blocks
x ./sysxr/cmx, 8722 bytes, 18 tape blocks
x ./sysxr/cmxr, 10130 bytes, 20 tape blocks
x ./sysxr/cpx, 13624 bytes, 27 tape blocks
x ./sysxr/hkx, 7308 bytes, 15 tape blocks
x ./sysxr/hkxr, 11348 bytes, 23 tape blocks
x ./sysxr/hpx, 8356 bytes, 17 tape blocks
x ./sysxr/hpxr, 11932 bytes, 24 tape blocks
x ./sysxr/rlx, 6376 bytes, 13 tape blocks
x ./sysxr/rlxr, 11108 bytes, 22 tape blocks
x ./sysxr/memx, 8100 bytes, 16 tape blocks
x ./sysxr/memxr, 3822 bytes, 8 tape blocks
x ./sysxr/mtx, 5910 bytes, 12 tape blocks
x ./sysxr/mtxr, 13302 bytes, 26 tape blocks
x ./sysxr/rpx, 7256 bytes, 15 tape blocks
x ./sysxr/rpxr, 11514 bytes, 23 tape blocks
x ./sysxr/rkx, 6282 bytes, 13 tape blocks
x ./sysxr/rkxr, 10790 bytes, 22 tape blocks
x ./sysxr/fpx, 12830 bytes, 26 tape blocks
x ./sysxr/lpx, 7116 bytes, 14 tape blocks
x ./sysxr/.profile, 105 bytes, 1 tape blocks
x ./sysxr/rax, 8006 bytes, 16 tape blocks
x ./sysxr/raxr, 12552 bytes, 25 tape blocks
x ./sysxr/sysx_caw.help, 690 bytes, 2 tape blocks
x ./sysxr/sysx_rxm.help, 641 bytes, 2 tape blocks
x ./sysxr/hxx, 7432 bytes, 15 tape blocks
x ./sysxr/hxxr, 11264 bytes, 22 tape blocks
# sync
```

Press CTRL-E and repeat for volumes 09 to 11

```
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_09of33_SYSGEN_1of2_BL-X314A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./sys/h/acct.h, 821 bytes, 2 tape blocks
x ./sys/h/bads.h, 2196 bytes, 5 tape blocks
x ./sys/h/buf.h, 3003 bytes, 6 tape blocks
x ./sys/h/callo.h, 415 bytes, 1 tape blocks
x ./sys/h/conf.h, 782 bytes, 2 tape blocks
x ./sys/h/devmaj.h, 1045 bytes, 3 tape blocks
x ./sys/h/dir.h, 95 bytes, 1 tape blocks
x ./sys/h/errlog.h, 4692 bytes, 10 tape blocks
x ./sys/h/fblk.h, 63 bytes, 1 tape blocks
x ./sys/h/file.h, 614 bytes, 2 tape blocks
x ./sys/h/filsys.h, 1111 bytes, 3 tape blocks
x ./sys/h/fperr.h, 171 bytes, 1 tape blocks
x ./sys/h/hkbad.h, 1361 bytes, 3 tape blocks
x ./sys/h/hmbad.h, 1362 bytes, 3 tape blocks
x ./sys/h/hpbad.h, 1363 bytes, 3 tape blocks
x ./sys/h/ino.h, 618 bytes, 2 tape blocks
x ./sys/h/inode.h, 2269 bytes, 5 tape blocks
x ./sys/h/map.h, 198 bytes, 1 tape blocks
x ./sys/h/mount.h, 459 bytes, 1 tape blocks
x ./sys/h/mpx.h, 794 bytes, 2 tape blocks
x ./sys/h/mscp.h, 10728 bytes, 21 tape blocks
x ./sys/h/mx.h, 2074 bytes, 5 tape blocks
x ./sys/h/pack.h, 2194 bytes, 5 tape blocks
x ./sys/h/param.h, 6390 bytes, 13 tape blocks
x ./sys/h/pk.h, 2876 bytes, 6 tape blocks
x ./sys/h/prim.h, 139 bytes, 1 tape blocks
x ./sys/h/proc.h, 2491 bytes, 5 tape blocks
x ./sys/h/pwd.h, 162 bytes, 1 tape blocks
x ./sys/h/reg.h, 415 bytes, 1 tape blocks
x ./sys/h/seg.h, 727 bytes, 2 tape blocks
x ./sys/h/sgtty.h, 3089 bytes, 7 tape blocks
x ./sys/h/stat.h, 903 bytes, 2 tape blocks
x ./sys/h/stdio.h, 870 bytes, 2 tape blocks
x ./sys/h/systm.h, 4354 bytes, 9 tape blocks
x ./sys/h/term.h, 4497 bytes, 9 tape blocks
x ./sys/h/text.h, 760 bytes, 2 tape blocks
x ./sys/h/timeb.h, 140 bytes, 1 tape blocks
x ./sys/h/tty.h, 5771 bytes, 12 tape blocks
x ./sys/h/types.h, 518 bytes, 2 tape blocks
x ./sys/h/user.h, 5470 bytes, 11 tape blocks
x ./sys/h/pk.p, 1865 bytes, 4 tape blocks
x ./sys/dev/u1.c, 1963 bytes, 4 tape blocks
x ./sys/dev/u2.c, 1963 bytes, 4 tape blocks
x ./sys/dev/u3.c, 1963 bytes, 4 tape blocks
x ./sys/dev/u4.c, 1963 bytes, 4 tape blocks
x ./sys/conf/makefile, 1956 bytes, 4 tape blocks
x ./sys/conf/dump.s, 17258 bytes, 34 tape blocks
x ./sys/conf/mch_id.o, 5572 bytes, 11 tape blocks
x ./sys/conf/mch_ov.o, 6920 bytes, 14 tape blocks
x ./sys/conf/mkconf, 38846 bytes, 76 tape blocks
x ./sys/conf/sysgen, 28144 bytes, 55 tape blocks
x ./sys/conf/sg_!.help, 605 bytes, 2 tape blocks
x ./sys/conf/sg_c.help, 1556 bytes, 4 tape blocks
x ./sys/conf/sg_cbsiz.help, 264 bytes, 1 tape blocks
x ./sys/conf/sg_cmn.help, 242 bytes, 1 tape blocks
x ./sys/conf/sg_cmt.help, 778 bytes, 2 tape blocks
x ./sys/conf/sg_cn.help, 189 bytes, 1 tape blocks
x ./sys/conf/sg_cpu.help, 661 bytes, 2 tape blocks
x ./sys/conf/sg_csr.help, 225 bytes, 1 tape blocks
x ./sys/conf/sg_d.help, 549 bytes, 2 tape blocks
x ./sys/conf/sg_dc.help, 996 bytes, 2 tape blocks
x ./sys/conf/sg_ddt.help, 852 bytes, 2 tape blocks
x ./sys/conf/sg_dn.help, 212 bytes, 1 tape blocks
x ./sys/conf/sg_dst.help, 258 bytes, 1 tape blocks
x ./sys/conf/sg_fp.help, 343 bytes, 1 tape blocks
x ./sys/conf/sg_h.help, 1029 bytes, 3 tape blocks
x ./sys/conf/sg_hz.help, 75 bytes, 1 tape blocks
x ./sys/conf/sg_i.help, 854 bytes, 2 tape blocks
x ./sys/conf/sg_l.help, 426 bytes, 1 tape blocks
x ./sys/conf/sg_m.help, 1340 bytes, 3 tape blocks
x ./sys/conf/sg_md.help, 627 bytes, 2 tape blocks
x ./sys/conf/sg_mpx.help, 405 bytes, 1 tape blocks
x ./sys/conf/sg_mseg.help, 488 bytes, 1 tape blocks
x ./sys/conf/sg_msgb.help, 420 bytes, 1 tape blocks
x ./sys/conf/sg_mtc.help, 646 bytes, 2 tape blocks
x ./sys/conf/sg_mtcd.help, 667 bytes, 2 tape blocks
x ./sys/conf/sg_mtn.help, 613 bytes, 2 tape blocks
x ./sys/conf/sg_muprc.help, 323 bytes, 1 tape blocks
x ./sys/conf/sg_nb.help, 186 bytes, 1 tape blocks
x ./sys/conf/sg_nbuf.help, 440 bytes, 1 tape blocks
x ./sys/conf/sg_ncall.help, 323 bytes, 1 tape blocks
x ./sys/conf/sg_ncargs.help, 387 bytes, 1 tape blocks
x ./sys/conf/sg_nclist.help, 400 bytes, 1 tape blocks
x ./sys/conf/sg_nfile.help, 231 bytes, 1 tape blocks
x ./sys/conf/sg_ninode.help, 403 bytes, 1 tape blocks
x ./sys/conf/sg_nmnt.help, 462 bytes, 1 tape blocks
x ./sys/conf/sg_nproc.help, 405 bytes, 1 tape blocks
x ./sys/conf/sg_ntext.help, 281 bytes, 1 tape blocks
x ./sys/conf/sg_p.help, 900 bytes, 2 tape blocks
x ./sys/conf/sg_pack.help, 408 bytes, 1 tape blocks
x ./sys/conf/sg_r.help, 328 bytes, 1 tape blocks
x ./sys/conf/sg_s.help, 1074 bytes, 3 tape blocks
x ./sys/conf/sg_sb.help, 301 bytes, 1 tape blocks
x ./sys/conf/sg_sd.help, 330 bytes, 1 tape blocks
x ./sys/conf/sg_sdn.help, 731 bytes, 2 tape blocks
x ./sys/conf/sg_sl.help, 1534 bytes, 3 tape blocks
x ./sys/conf/sg_sp.help, 1417 bytes, 3 tape blocks
x ./sys/conf/sg_tz.help, 262 bytes, 1 tape blocks
x ./sys/conf/sg_udev.help, 702 bytes, 2 tape blocks
x ./sys/conf/sg_vec.help, 255 bytes, 1 tape blocks
x ./sys/conf/hk_bv.cf, 257 bytes, 1 tape blocks
x ./sys/conf/hm_bv.cf, 257 bytes, 1 tape blocks
x ./sys/conf/hp_bv.cf, 257 bytes, 1 tape blocks
x ./sys/conf/rp_bv.cf, 257 bytes, 1 tape blocks
x ./sys/conf/ra_bv.cf, 257 bytes, 1 tape blocks
x ./sys/conf/rl01_bv.cf, 260 bytes, 1 tape blocks
x ./sys/conf/rl02_bv.cf, 305 bytes, 1 tape blocks
x ./sys/conf/m11_rlrd.cf, 351 bytes, 1 tape blocks
x ./sys/conf/m11_rd.cf, 336 bytes, 1 tape blocks
x ./sys/conf/m11_rlht.cf, 285 bytes, 1 tape blocks
x ./sys/conf/m11_rlts.cf, 285 bytes, 1 tape blocks
x ./sys/conf/m11_rdrl.cf, 351 bytes, 1 tape blocks
x ./sys/conf/l.s, 4347 bytes, 9 tape blocks
x ./sys/conf/c.c, 8260 bytes, 17 tape blocks
x ./sys/conf/mch0.s, 111 bytes, 1 tape blocks
x ./sys/conf/text.sizes, 2790 bytes, 6 tape blocks
x ./sys/conf/ovload, 513 bytes, 2 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_10of33_SYSGEN_2of2_BL-X315A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./sys/ovsys/LIB1_ov, 43468 bytes, 85 tape blocks
x ./sys/ovsys/mklib_ov, 119 bytes, 1 tape blocks
x ./sys/ovsys/acct.o, 2188 bytes, 5 tape blocks
x ./sys/ovsys/alloc.o, 5208 bytes, 11 tape blocks
x ./sys/ovsys/clock.o, 3144 bytes, 7 tape blocks
x ./sys/ovsys/errlog.o, 4884 bytes, 10 tape blocks
x ./sys/ovsys/fio.o, 3308 bytes, 7 tape blocks
x ./sys/ovsys/iget.o, 5440 bytes, 11 tape blocks
x ./sys/ovsys/machdep.o, 3140 bytes, 7 tape blocks
x ./sys/ovsys/main.o, 2544 bytes, 5 tape blocks
x ./sys/ovsys/malloc.o, 1528 bytes, 3 tape blocks
x ./sys/ovsys/nami.o, 3340 bytes, 7 tape blocks
x ./sys/ovsys/pipe.o, 2292 bytes, 5 tape blocks
x ./sys/ovsys/prf.o, 2240 bytes, 5 tape blocks
x ./sys/ovsys/prim.o, 4004 bytes, 8 tape blocks
x ./sys/ovsys/rdwri.o, 4076 bytes, 8 tape blocks
x ./sys/ovsys/sig.o, 6116 bytes, 12 tape blocks
x ./sys/ovsys/slp.o, 7212 bytes, 15 tape blocks
x ./sys/ovsys/subr.o, 3580 bytes, 7 tape blocks
x ./sys/ovsys/sys1.o, 9368 bytes, 19 tape blocks
x ./sys/ovsys/sys2.o, 4032 bytes, 8 tape blocks
x ./sys/ovsys/sys3.o, 3984 bytes, 8 tape blocks
x ./sys/ovsys/sys4.o, 5412 bytes, 11 tape blocks
x ./sys/ovsys/sysent.o, 1848 bytes, 4 tape blocks
x ./sys/ovsys/text.o, 4600 bytes, 9 tape blocks
x ./sys/ovsys/trap.o, 4896 bytes, 10 tape blocks
x ./sys/ovsys/ureg.o, 2704 bytes, 6 tape blocks
x ./sys/ovdev/bio.o, 7256 bytes, 15 tape blocks
x ./sys/ovdev/ct.o, 876 bytes, 2 tape blocks
x ./sys/ovdev/dh.o, 5380 bytes, 11 tape blocks
x ./sys/ovdev/dhdm.o, 1476 bytes, 3 tape blocks
x ./sys/ovdev/dhfdm.o, 220 bytes, 1 tape blocks
x ./sys/ovdev/dkleave.o, 464 bytes, 1 tape blocks
x ./sys/ovdev/dn.o, 1164 bytes, 3 tape blocks
x ./sys/ovdev/dsort.o, 2620 bytes, 6 tape blocks
x ./sys/ovdev/du.o, 3932 bytes, 8 tape blocks
x ./sys/ovdev/dz.o, 5296 bytes, 11 tape blocks
x ./sys/ovdev/hk.o, 12296 bytes, 25 tape blocks
x ./sys/ovdev/hm.o, 12656 bytes, 25 tape blocks
x ./sys/ovdev/hp.o, 12656 bytes, 25 tape blocks
x ./sys/ovdev/hs.o, 3536 bytes, 7 tape blocks
x ./sys/ovdev/ht.o, 6388 bytes, 13 tape blocks
x ./sys/ovdev/hx.o, 5336 bytes, 11 tape blocks
x ./sys/ovdev/kl.o, 3388 bytes, 7 tape blocks
x ./sys/ovdev/lp.o, 2868 bytes, 6 tape blocks
x ./sys/ovdev/mem.o, 1112 bytes, 3 tape blocks
x ./sys/ovdev/ml.o, 4028 bytes, 8 tape blocks
x ./sys/ovdev/mpxpk.o, 1856 bytes, 4 tape blocks
x ./sys/ovdev/mx1.o, 6552 bytes, 13 tape blocks
x ./sys/ovdev/mx2.o, 11360 bytes, 23 tape blocks
x ./sys/ovdev/partab.o, 284 bytes, 1 tape blocks
x ./sys/ovdev/pk0.o, 8588 bytes, 17 tape blocks
x ./sys/ovdev/pk1.o, 2992 bytes, 6 tape blocks
x ./sys/ovdev/pk2.o, 3380 bytes, 7 tape blocks
x ./sys/ovdev/pk3.o, 2476 bytes, 5 tape blocks
x ./sys/ovdev/ra.o, 12708 bytes, 25 tape blocks
x ./sys/ovdev/rk.o, 3368 bytes, 7 tape blocks
x ./sys/ovdev/rl.o, 4788 bytes, 10 tape blocks
x ./sys/ovdev/rp.o, 3792 bytes, 8 tape blocks
x ./sys/ovdev/sys.o, 600 bytes, 2 tape blocks
x ./sys/ovdev/t.o, 4748 bytes, 10 tape blocks
x ./sys/ovdev/tc.o, 1980 bytes, 4 tape blocks
x ./sys/ovdev/tm.o, 5160 bytes, 11 tape blocks
x ./sys/ovdev/ts.o, 7224 bytes, 15 tape blocks
x ./sys/ovdev/tty.o, 12148 bytes, 24 tape blocks
x ./sys/ovdev/u1.o, 732 bytes, 2 tape blocks
x ./sys/ovdev/u2.o, 732 bytes, 2 tape blocks
x ./sys/ovdev/u3.o, 732 bytes, 2 tape blocks
x ./sys/ovdev/u4.o, 732 bytes, 2 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_11of33_DICTIONARY_BL-X316A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./usr/dict/stop, 8003 bytes, 16 tape blocks
x ./usr/dict/papers/Ind.ia, 5986 bytes, 12 tape blocks
x ./usr/dict/papers/runinv, 115 bytes, 1 tape blocks
x ./usr/dict/papers/Ind.ib, 1630 bytes, 4 tape blocks
x ./usr/dict/papers/Ind.ic, 674 bytes, 2 tape blocks
x ./usr/dict/papers/Rv7man, 7981 bytes, 16 tape blocks
x ./usr/dict/words, 196513 bytes, 384 tape blocks
x ./usr/dict/spellhist, 0 bytes, 0 tape blocks
x ./usr/dict/local, 0 bytes, 0 tape blocks
x ./usr/dict/hlista, 45000 bytes, 88 tape blocks
x ./usr/dict/hlistb, 45000 bytes, 88 tape blocks
x ./usr/dict/hstop, 45000 bytes, 88 tape blocks
x ./usr/dict/american, 2969 bytes, 6 tape blocks
x ./usr/dict/british, 3184 bytes, 7 tape blocks
# sync
# 
```

NB: Volume 12 contains some binaries that can go into /bin
or into /usr/bin.  I'm going to restore them into the /usr
partition on the second RD51 drive.

```
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_12of33_OPTIONAL_BINARIES_1of3_BL-X317A-BC.IMG
RQ3: Unit is read only
sim> c

# cd /usr
# tar xvf /dev/rx3
Tar: blocksize = 20
x ./bin/ac, 9942 bytes, 20 tape blocks
x ./bin/arcv, 3610 bytes, 8 tape blocks
x ./bin/at, 8226 bytes, 17 tape blocks
x ./bin/badstat, 10108 bytes, 20 tape blocks
x ./bin/bas, 6842 bytes, 14 tape blocks
x ./bin/cal, 3682 bytes, 8 tape blocks
x ./bin/cb, 5422 bytes, 11 tape blocks
x ./bin/col, 4782 bytes, 10 tape blocks
x ./bin/comm, 4298 bytes, 9 tape blocks
x ./bin/cu, 11328 bytes, 23 tape blocks
x ./bin/cuo, 7208 bytes, 15 tape blocks
x ./bin/dc, 23050 bytes, 46 tape blocks
x ./bin/dcheck, 4312 bytes, 9 tape blocks
x ./bin/deroff, 8758 bytes, 18 tape blocks
x ./bin/icheck, 6948 bytes, 14 tape blocks
x ./bin/join, 5870 bytes, 12 tape blocks
x ./bin/look, 5042 bytes, 10 tape blocks
x ./bin/ncheck, 4858 bytes, 10 tape blocks
x ./bin/nroff, 38158 bytes, 75 tape blocks
x ./bin/prep, 7002 bytes, 14 tape blocks
x ./bin/prof, 11368 bytes, 23 tape blocks
x ./bin/ptx, 8042 bytes, 16 tape blocks
x ./bin/ranlib, 6096 bytes, 12 tape blocks
x ./bin/rev, 3462 bytes, 7 tape blocks
x ./bin/sed, 12876 bytes, 26 tape blocks
x ./bin/sort, 9360 bytes, 19 tape blocks
x ./bin/spell, 546 bytes, 2 tape blocks
x ./bin/spline, 9322 bytes, 19 tape blocks
x ./bin/split, 3876 bytes, 8 tape blocks
x ./bin/t300, 10206 bytes, 20 tape blocks
x ./bin/t300s, 10334 bytes, 21 tape blocks
x ./bin/t450, 10198 bytes, 20 tape blocks
x ./bin/tek, 10684 bytes, 21 tape blocks
x ./bin/tk, 5550 bytes, 11 tape blocks
x ./bin/tp, 9852 bytes, 20 tape blocks
x ./bin/tsort, 7840 bytes, 16 tape blocks
x ./bin/uniq, 4252 bytes, 9 tape blocks
x ./bin/units, 9440 bytes, 19 tape blocks
x ./bin/vplot, 10920 bytes, 22 tape blocks
# sync
# 
```

Volumes 13 to 23 have a ./usr relative, so restore them into the root
directory and they'll end up in the right places.

```
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_13of33_OPTIONAL_BINARIES_2of3_BL-X318A-BC.IMG 
RQ3: Unit is read only
sim> 
sim> c

# cd /
# tar xvf /dev/rx3
Tar: blocksize = 20
x ./usr/bin/bc, 13036 bytes, 26 tape blocks
x ./usr/bin/checkeq, 4374 bytes, 9 tape blocks
x ./usr/bin/enroll, 15486 bytes, 31 tape blocks
x ./usr/bin/eqn, 28236 bytes, 56 tape blocks
x ./usr/bin/graph, 15546 bytes, 31 tape blocks
x ./usr/bin/learn, 18702 bytes, 37 tape blocks
x ./usr/bin/lex, 28932 bytes, 57 tape blocks
x ./usr/bin/m4, 13510 bytes, 27 tape blocks
x ./usr/bin/neqn, 25790 bytes, 51 tape blocks
x ./usr/bin/refer, 29418 bytes, 58 tape blocks
x ./usr/bin/roff, 8662 bytes, 17 tape blocks
x ./usr/bin/tbl, 30464 bytes, 60 tape blocks
x ./usr/bin/troff, 41666 bytes, 82 tape blocks
x ./usr/bin/xsend, 21346 bytes, 42 tape blocks
# sync
#
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_14of33_OPTIONAL_BINARIES_3of3_BL-X319A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./usr/bin/awk, 51072 bytes, 100 tape blocks
x ./usr/bin/ex, 81900 bytes, 160 tape blocks
x ./usr/bin/f77, 14036 bytes, 28 tape blocks
x ./usr/bin/ratfor, 13410 bytes, 27 tape blocks
x ./usr/bin/uucp, 34716 bytes, 68 tape blocks
x ./usr/bin/uulog, 15450 bytes, 31 tape blocks
x ./usr/bin/uuname, 13880 bytes, 28 tape blocks
x ./usr/bin/uupoll, 27380 bytes, 54 tape blocks
x ./usr/bin/uustat, 29066 bytes, 57 tape blocks
x ./usr/bin/uux, 34926 bytes, 69 tape blocks
x ./usr/bin/xget, 20672 bytes, 41 tape blocks
x ./usr/bin/yacc, 24268 bytes, 48 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_15of33_OPTIONAL_LIBRARIES_1of4_BL-X320A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./lib/libsa.a, 41870 bytes, 82 tape blocks
x ./lib/libt300.a, 7602 bytes, 15 tape blocks
x ./lib/libt300s.a, 8090 bytes, 16 tape blocks
x ./lib/libt4014.a, 8464 bytes, 17 tape blocks
x ./lib/libt450.a, 7642 bytes, 15 tape blocks
x ./lib/libplot.a, 5052 bytes, 10 tape blocks
x ./usr/games/fortune, 4536 bytes, 9 tape blocks
x ./usr/games/lib/fortunes, 6841 bytes, 14 tape blocks
x ./usr/games/bj, 1562 bytes, 4 tape blocks
x ./usr/games/checkers, 26614 bytes, 52 tape blocks
x ./usr/games/chess, 19036 bytes, 38 tape blocks
x ./usr/games/cubic, 2450 bytes, 5 tape blocks
x ./usr/games/hangman, 7500 bytes, 15 tape blocks
x ./usr/games/maze, 9350 bytes, 19 tape blocks
x ./usr/games/moo, 624 bytes, 2 tape blocks
x ./usr/games/words, 69 bytes, 1 tape blocks
x ./usr/games/reversi, 9320 bytes, 19 tape blocks
x ./usr/games/ttt, 2192 bytes, 5 tape blocks
x ./usr/games/ttt.k, 278 bytes, 1 tape blocks
x ./usr/games/wump, 5184 bytes, 11 tape blocks
x ./usr/games/arithmetic, 5802 bytes, 12 tape blocks
x ./usr/games/words1, 1202 bytes, 3 tape blocks
x ./usr/games/ching, 145 bytes, 1 tape blocks
x ./usr/games/backgammon, 14148 bytes, 28 tape blocks
x ./usr/games/bcd, 2582 bytes, 6 tape blocks
x ./usr/games/quiz, 6306 bytes, 13 tape blocks
x ./usr/games/banner, 2840 bytes, 6 tape blocks
x ./usr/games/ppt, 2900 bytes, 6 tape blocks
x ./usr/games/quiz.k/africa, 917 bytes, 2 tape blocks
x ./usr/games/quiz.k/america, 548 bytes, 2 tape blocks
x ./usr/games/quiz.k/areas, 3898 bytes, 8 tape blocks
x ./usr/games/quiz.k/arith, 789 bytes, 2 tape blocks
x ./usr/games/quiz.k/asia, 733 bytes, 2 tape blocks
x ./usr/games/quiz.k/babies, 401 bytes, 1 tape blocks
x ./usr/games/quiz.k/bard, 6853 bytes, 14 tape blocks
x ./usr/games/quiz.k/chinese, 138 bytes, 1 tape blocks
x ./usr/games/quiz.k/collectives, 1403 bytes, 3 tape blocks
x ./usr/games/quiz.k/ed, 3630 bytes, 8 tape blocks
x ./usr/games/quiz.k/elements, 2130 bytes, 5 tape blocks
x ./usr/games/quiz.k/europe, 752 bytes, 2 tape blocks
x ./usr/games/quiz.k/greek, 249 bytes, 1 tape blocks
x ./usr/games/quiz.k/inca, 301 bytes, 1 tape blocks
x ./usr/games/quiz.k/index, 1461 bytes, 3 tape blocks
x ./usr/games/quiz.k/latin, 2956 bytes, 6 tape blocks
x ./usr/games/quiz.k/locomotive, 163 bytes, 1 tape blocks
x ./usr/games/quiz.k/midearth, 274 bytes, 1 tape blocks
x ./usr/games/quiz.k/morse, 160 bytes, 1 tape blocks
x ./usr/games/quiz.k/murders, 930 bytes, 2 tape blocks
x ./usr/games/quiz.k/poetry, 5967 bytes, 12 tape blocks
x ./usr/games/quiz.k/posneg, 814 bytes, 2 tape blocks
x ./usr/games/quiz.k/pres, 2351 bytes, 5 tape blocks
x ./usr/games/quiz.k/province, 314 bytes, 1 tape blocks
x ./usr/games/quiz.k/seq-easy, 722 bytes, 2 tape blocks
x ./usr/games/quiz.k/seq-hard, 872 bytes, 2 tape blocks
x ./usr/games/quiz.k/sexes, 405 bytes, 1 tape blocks
x ./usr/games/quiz.k/sov, 1652 bytes, 4 tape blocks
x ./usr/games/quiz.k/spell, 74 bytes, 1 tape blocks
x ./usr/games/quiz.k/state, 2098 bytes, 5 tape blocks
x ./usr/games/quiz.k/trek, 1060 bytes, 3 tape blocks
x ./usr/games/quiz.k/ucc, 6701 bytes, 14 tape blocks
x ./usr/games/ching.d/cno, 4204 bytes, 9 tape blocks
x ./usr/games/ching.d/hexagrams, 58975 bytes, 116 tape blocks
x ./usr/games/ching.d/macros, 1357 bytes, 3 tape blocks
x ./usr/games/ching.d/phx, 5608 bytes, 11 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_16of33_OPTIONAL_LIBRARIES_2of4_BL-X321A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./usr/lib/awk_strings, 1115 bytes, 3 tape blocks
x ./usr/lib/f77pass1, 92878 bytes, 182 tape blocks
x ./usr/lib/f77_strings, 7074 bytes, 14 tape blocks
x ./usr/lib/libF77.a, 28188 bytes, 56 tape blocks
x ./usr/lib/libF77ov.a, 36252 bytes, 71 tape blocks
x ./usr/lib/libI77.a, 87180 bytes, 171 tape blocks
x ./usr/lib/libI77ov.a, 87180 bytes, 171 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_17of33_OPTIONAL_LIBRARIES_3of4_BL-X322A-BC.IMG 
RQ3: Unit is read only
sim> c
tar xvf /dev/rx3
Tar: blocksize = 20
x ./usr/lib/atrun, 10328 bytes, 21 tape blocks
x ./usr/lib/font/ftC, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftCE, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftG, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftCI, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftCK, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftCS, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftCW, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftGI, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftI, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftGM, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftGR, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftLI, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftL, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftR, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftPA, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftPB, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftPI, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftSB, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftS, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftBC, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftSI, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftSM, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftUD, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftXM, 240 bytes, 1 tape blocks
x ./usr/lib/font/ftB, 240 bytes, 1 tape blocks
x ./usr/lib/lex/ncform, 3917 bytes, 8 tape blocks
x ./usr/lib/refer/inv, 13746 bytes, 27 tape blocks
x ./usr/lib/refer/hunt, 19050 bytes, 38 tape blocks
x ./usr/lib/refer/mkey, 6410 bytes, 13 tape blocks
x ./usr/lib/spell, 8864 bytes, 18 tape blocks
x ./usr/lib/spellin, 4496 bytes, 9 tape blocks
x ./usr/lib/spellout, 4608 bytes, 9 tape blocks
x ./usr/lib/term/tab300-12, 1434 bytes, 3 tape blocks
x ./usr/lib/term/tab300, 1436 bytes, 3 tape blocks
x ./usr/lib/term/tab300s-12, 1430 bytes, 3 tape blocks
x ./usr/lib/term/tab300s, 1434 bytes, 3 tape blocks
x ./usr/lib/term/tab37, 1288 bytes, 3 tape blocks
x ./usr/lib/term/tab450-12-8, 1446 bytes, 3 tape blocks
x ./usr/lib/term/tab450-12, 1440 bytes, 3 tape blocks
x ./usr/lib/term/tab450, 1434 bytes, 3 tape blocks
x ./usr/lib/term/tab832, 1432 bytes, 3 tape blocks
x ./usr/lib/term/taba1, 1432 bytes, 3 tape blocks
x ./usr/lib/term/tablp, 1164 bytes, 3 tape blocks
x ./usr/lib/term/tabtn300, 1174 bytes, 3 tape blocks
x ./usr/lib/tmac/tmac.an, 4052 bytes, 8 tape blocks
x ./usr/lib/tmac/tmac.s, 20194 bytes, 40 tape blocks
x ./usr/lib/tmac/tmac.scover, 4299 bytes, 9 tape blocks
x ./usr/lib/tmac/tmac.sdisp, 809 bytes, 2 tape blocks
x ./usr/lib/tmac/tmac.skeep, 1190 bytes, 3 tape blocks
x ./usr/lib/tmac/tmac.srefs, 2423 bytes, 5 tape blocks
x ./usr/lib/tmac/tmac.f, 20221 bytes, 40 tape blocks
x ./usr/lib/tmac/tmac.an.orig, 3941 bytes, 8 tape blocks
x ./usr/lib/tmac/tmac.r, 1260 bytes, 3 tape blocks
x ./usr/lib/tmac/tmac.e, 18747 bytes, 37 tape blocks
x ./usr/lib/struct/structure, 62028 bytes, 122 tape blocks
x ./usr/lib/struct/beautify, 20408 bytes, 40 tape blocks
x ./usr/lib/units, 7895 bytes, 16 tape blocks
x ./usr/lib/yaccpar, 3246 bytes, 7 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_18of33_OPTIONAL_LIBRARIES_4of4_BL-X323A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./usr/lib/uucp/uuxqt, 41094 bytes, 81 tape blocks
x ./usr/lib/uucp/SEQF, 4 bytes, 1 tape blocks
x ./usr/lib/uucp/uuclean, 25352 bytes, 50 tape blocks
x ./usr/lib/uucp/L-dialcodes, 11 bytes, 1 tape blocks
x ./usr/lib/uucp/L.sys, 0 bytes, 0 tape blocks
x ./usr/lib/uucp/L-devices, 61 bytes, 1 tape blocks
x ./usr/lib/uucp/USERFILE, 36 bytes, 1 tape blocks
x ./usr/lib/uucp/uusub, 4486 bytes, 9 tape blocks
x ./usr/lib/uucp/L.sys.sample, 465 bytes, 1 tape blocks
x ./usr/lib/uucp/uucico, 83026 bytes, 163 tape blocks
x ./usr/lib/uucp/L.cmds, 23 bytes, 1 tape blocks
x ./usr/lib/uucp/.XQTDIR, 0 bytes, 0 tape blocks
x ./usr/lib/uucp/INSECURE, 0 bytes, 0 tape blocks
x ./usr/lib/uucp/ouucico, 52188 bytes, 102 tape blocks
x ./usr/lib/uucp/USERFILE.save, 56 bytes, 1 tape blocks
x ./usr/lib/uucp/L_stat, 54 bytes, 1 tape blocks
x ./usr/lib/uucp/R_stat, 288 bytes, 1 tape blocks
x ./usr/lib/uucp/uucp.day, 868 bytes, 2 tape blocks
x ./usr/lib/uucp/uucp.hour, 463 bytes, 1 tape blocks
x ./usr/lib/uucp/uucp.night, 1113 bytes, 3 tape blocks
x ./usr/lib/uucp/uucp.noon, 404 bytes, 1 tape blocks
x ./usr/lib/uucp/uucp.week, 654 bytes, 2 tape blocks
x ./usr/lib/uucp/uumonitor, 17032 bytes, 34 tape blocks
x ./usr/lib/libcurses.a, 35044 bytes, 69 tape blocks
x ./usr/lib/libcursesov.a, 35044 bytes, 69 tape blocks
x ./usr/lib/libtermlib.a, 6878 bytes, 14 tape blocks
x ./usr/lib/libtermlibov.a, 6878 bytes, 14 tape blocks
x ./usr/lib/ex3.7preserve, 9856 bytes, 20 tape blocks
x ./usr/lib/ex3.7recover, 9856 bytes, 20 tape blocks
x ./usr/lib/ex3.7strings, 6324 bytes, 13 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_19of33_LEARN_1of4_BL-X324A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./usr/lib/learn/C/L0, 14 bytes, 1 tape blocks
x ./usr/lib/learn/C/L0.1a, 414 bytes, 1 tape blocks
x ./usr/lib/learn/C/L1.1a, 412 bytes, 1 tape blocks
x ./usr/lib/learn/C/L1.1b, 555 bytes, 2 tape blocks
x ./usr/lib/learn/C/L1.1c, 227 bytes, 1 tape blocks
x ./usr/lib/learn/C/L1.1d, 510 bytes, 1 tape blocks
x ./usr/lib/learn/C/L1.1e, 215 bytes, 1 tape blocks
x ./usr/lib/learn/C/L1.1f, 254 bytes, 1 tape blocks
x ./usr/lib/learn/C/L10, 375 bytes, 1 tape blocks
x ./usr/lib/learn/C/L11.1a, 770 bytes, 2 tape blocks
x ./usr/lib/learn/C/L11.2a, 681 bytes, 2 tape blocks
x ./usr/lib/learn/C/L12.1a, 579 bytes, 2 tape blocks
x ./usr/lib/learn/C/L12.1b, 1063 bytes, 3 tape blocks
x ./usr/lib/learn/C/L13.1a, 959 bytes, 2 tape blocks
x ./usr/lib/learn/C/L14.1a, 644 bytes, 2 tape blocks
x ./usr/lib/learn/C/L14.2a, 520 bytes, 2 tape blocks
x ./usr/lib/learn/C/L14.2b, 992 bytes, 2 tape blocks
x ./usr/lib/learn/C/L15.1a, 698 bytes, 2 tape blocks
x ./usr/lib/learn/C/L15.1b, 605 bytes, 2 tape blocks
x ./usr/lib/learn/C/L16.2a, 1680 bytes, 4 tape blocks
x ./usr/lib/learn/C/L16.2b, 999 bytes, 2 tape blocks
x ./usr/lib/learn/C/L16.2c, 1947 bytes, 4 tape blocks
x ./usr/lib/learn/C/L17.1a, 2951 bytes, 6 tape blocks
x ./usr/lib/learn/C/L17.1c, 707 bytes, 2 tape blocks
x ./usr/lib/learn/C/L18.1a, 933 bytes, 2 tape blocks
x ./usr/lib/learn/C/L19.1a, 498 bytes, 1 tape blocks
x ./usr/lib/learn/C/L2.1a, 1408 bytes, 3 tape blocks
x ./usr/lib/learn/C/L2.1b, 445 bytes, 1 tape blocks
x ./usr/lib/learn/C/L2.1c, 231 bytes, 1 tape blocks
x ./usr/lib/learn/C/L2.1d, 215 bytes, 1 tape blocks
x ./usr/lib/learn/C/L2.1e, 211 bytes, 1 tape blocks
x ./usr/lib/learn/C/L20.1a, 1840 bytes, 4 tape blocks
x ./usr/lib/learn/C/L3.1a, 1123 bytes, 3 tape blocks
x ./usr/lib/learn/C/L3.1b, 211 bytes, 1 tape blocks
x ./usr/lib/learn/C/L30.1a, 946 bytes, 2 tape blocks
x ./usr/lib/learn/C/L31.1a, 642 bytes, 2 tape blocks
x ./usr/lib/learn/C/L32.1a, 736 bytes, 2 tape blocks
x ./usr/lib/learn/C/L33.1a, 442 bytes, 1 tape blocks
x ./usr/lib/learn/C/L35.1a, 775 bytes, 2 tape blocks
x ./usr/lib/learn/C/L37.1a, 1064 bytes, 3 tape blocks
x ./usr/lib/learn/C/L4.1a, 151 bytes, 1 tape blocks
x ./usr/lib/learn/C/L40.1a, 862 bytes, 2 tape blocks
x ./usr/lib/learn/C/L41.1a, 1266 bytes, 3 tape blocks
x ./usr/lib/learn/C/L42.1a, 1018 bytes, 2 tape blocks
x ./usr/lib/learn/C/L43.1a, 1061 bytes, 3 tape blocks
x ./usr/lib/learn/C/L43.1b, 1212 bytes, 3 tape blocks
x ./usr/lib/learn/C/L5.1a, 377 bytes, 1 tape blocks
x ./usr/lib/learn/C/L5.1b, 474 bytes, 1 tape blocks
x ./usr/lib/learn/C/L5.1c, 608 bytes, 2 tape blocks
x ./usr/lib/learn/C/L5.1d, 852 bytes, 2 tape blocks
x ./usr/lib/learn/C/L5.1e, 639 bytes, 2 tape blocks
x ./usr/lib/learn/C/L5.1f, 426 bytes, 1 tape blocks
x ./usr/lib/learn/C/L5.1g, 622 bytes, 2 tape blocks
x ./usr/lib/learn/C/L5.2a, 543 bytes, 2 tape blocks
x ./usr/lib/learn/C/L5.2b, 581 bytes, 2 tape blocks
x ./usr/lib/learn/C/L5.2e, 1579 bytes, 4 tape blocks
x ./usr/lib/learn/C/L5.3e, 635 bytes, 2 tape blocks
x ./usr/lib/learn/C/L50.1a, 4556 bytes, 9 tape blocks
x ./usr/lib/learn/C/L9.1a, 586 bytes, 2 tape blocks
x ./usr/lib/learn/C/getline.c, 242 bytes, 1 tape blocks
x ./usr/lib/learn/C/getline.o, 492 bytes, 1 tape blocks
x ./usr/lib/learn/C/getnum.c, 158 bytes, 1 tape blocks
x ./usr/lib/learn/C/getnum.o, 424 bytes, 1 tape blocks
x ./usr/lib/learn/C.a, 51524 bytes, 101 tape blocks
x ./usr/lib/learn/morefiles/L0, 14 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L0.1a, 699 bytes, 2 tape blocks
x ./usr/lib/learn/morefiles/L0.1b, 1419 bytes, 3 tape blocks
x ./usr/lib/learn/morefiles/L0.1c, 291 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L0.1d, 201 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L0.1e, 440 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L0.1f, 230 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L0.1g, 126 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L1.1a, 12648 bytes, 25 tape blocks
x ./usr/lib/learn/morefiles/L1.1b, 12697 bytes, 25 tape blocks
x ./usr/lib/learn/morefiles/L1.1c, 449 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L1.1d, 3594 bytes, 8 tape blocks
x ./usr/lib/learn/morefiles/L2.1a, 899 bytes, 2 tape blocks
x ./usr/lib/learn/morefiles/L2.1b, 267 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L2.1c, 342 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L2.1d, 177 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L2.1e, 529 bytes, 2 tape blocks
x ./usr/lib/learn/morefiles/L2.1f, 299 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L3.1a, 805 bytes, 2 tape blocks
x ./usr/lib/learn/morefiles/L3.1b, 429 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L3.1c, 1710 bytes, 4 tape blocks
x ./usr/lib/learn/morefiles/L3.1d, 1719 bytes, 4 tape blocks
x ./usr/lib/learn/morefiles/L3.1e, 1403 bytes, 3 tape blocks
x ./usr/lib/learn/morefiles/L3.1f, 297 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L3.1g, 2060 bytes, 5 tape blocks
x ./usr/lib/learn/morefiles/L4.1a, 621 bytes, 2 tape blocks
x ./usr/lib/learn/morefiles/L4.1b, 1375 bytes, 3 tape blocks
x ./usr/lib/learn/morefiles/L4.1c, 1637 bytes, 4 tape blocks
x ./usr/lib/learn/morefiles/L4.1d, 6323 bytes, 13 tape blocks
x ./usr/lib/learn/morefiles/L4.1e, 1381 bytes, 3 tape blocks
x ./usr/lib/learn/morefiles/L4.1f, 2284 bytes, 5 tape blocks
x ./usr/lib/learn/morefiles/L4.1g, 2399 bytes, 5 tape blocks
x ./usr/lib/learn/morefiles/L4.2a, 130 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L5.1a, 587 bytes, 2 tape blocks
x ./usr/lib/learn/morefiles/L5.1b, 654 bytes, 2 tape blocks
x ./usr/lib/learn/morefiles/L5.1c, 714 bytes, 2 tape blocks
x ./usr/lib/learn/morefiles/L5.1d, 350 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L5.1e, 331 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles/L6.1a, 1612 bytes, 4 tape blocks
x ./usr/lib/learn/morefiles/L6.1b, 1883 bytes, 4 tape blocks
x ./usr/lib/learn/morefiles/L6.1c, 555 bytes, 2 tape blocks
x ./usr/lib/learn/morefiles/L6.1d, 2581 bytes, 6 tape blocks
x ./usr/lib/learn/morefiles/L6.1e, 1606 bytes, 4 tape blocks
x ./usr/lib/learn/morefiles/L6.2e, 1199 bytes, 3 tape blocks
x ./usr/lib/learn/morefiles/L7.1a, 127 bytes, 1 tape blocks
x ./usr/lib/learn/morefiles.a, 73294 bytes, 144 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_20of33_LEARN_2of4_BL-X325A-BC.IMG 
RQ3: Unit is read only
sim> c
tar xvf /dev/rx3
Tar: blocksize = 20
x ./usr/lib/learn/editor.a, 213732 bytes, 418 tape blocks
x ./usr/lib/learn/eqn/L0, 11 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L0.1a, 1208 bytes, 3 tape blocks
x ./usr/lib/learn/eqn/L1.1a, 830 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L1.1b, 1110 bytes, 3 tape blocks
x ./usr/lib/learn/eqn/L1.1c, 1101 bytes, 3 tape blocks
x ./usr/lib/learn/eqn/L1.1d, 803 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L1.1e, 811 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L1.1f, 2050 bytes, 5 tape blocks
x ./usr/lib/learn/eqn/L10.1a, 1470 bytes, 3 tape blocks
x ./usr/lib/learn/eqn/L10.1b, 577 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L10.1c, 484 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L10.2c, 202 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L11.1a, 838 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L11.1b, 575 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L11.1c, 471 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L11.1d, 387 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L11.1e, 459 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L11.1f, 966 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L11.1g, 410 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L12.1a, 1052 bytes, 3 tape blocks
x ./usr/lib/learn/eqn/L12.1b, 738 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L12.1c, 699 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L12.1d, 522 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L12.1e, 865 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L2.1a, 687 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L2.1b, 920 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L2.1c, 1031 bytes, 3 tape blocks
x ./usr/lib/learn/eqn/L2.1d, 776 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L2.1e, 608 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L2.1f, 758 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L2.2a, 338 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L2.2b, 595 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L2.2e, 598 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L3.1a, 980 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L3.1b, 655 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L3.1c, 623 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L3.1d, 901 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L3.1e, 702 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L3.2a, 149 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L3.2c, 341 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L3.2d, 161 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L4.1a, 582 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L4.1b, 571 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L4.1c, 193 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L4.1d, 742 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L4.2a, 426 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L4.2c, 189 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L5.1a, 776 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L5.1b, 961 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L5.1c, 798 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L5.1d, 1185 bytes, 3 tape blocks
x ./usr/lib/learn/eqn/L5.1e, 1032 bytes, 3 tape blocks
x ./usr/lib/learn/eqn/L5.1f, 129 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L5.1g, 275 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L5.1h, 978 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L5.2b, 484 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L5.2d, 413 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L5.2g, 147 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L6.1a, 1588 bytes, 4 tape blocks
x ./usr/lib/learn/eqn/L6.1b, 841 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L6.1c, 536 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L6.1d, 214 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L7.1a, 852 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L7.1b, 889 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L7.1c, 562 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L7.1d, 638 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L7.2b, 188 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L7.2c, 371 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L8.1a, 866 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L8.1b, 535 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L8.2b, 188 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L9.1a, 1553 bytes, 4 tape blocks
x ./usr/lib/learn/eqn/L9.1b, 711 bytes, 2 tape blocks
x ./usr/lib/learn/eqn/L9.2a, 394 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L9.2b, 255 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/L9.3b, 130 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/tinyms, 205 bytes, 1 tape blocks
x ./usr/lib/learn/eqn/Init, 273 bytes, 1 tape blocks
x ./usr/lib/learn/eqn.a, 53200 bytes, 104 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_21of33_LEARN_3of4_BL-X326A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./usr/lib/learn/editor/L0, 14 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L1.1a, 577 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L10.1a, 566 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L10.1b, 724 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L10.2a, 613 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L10.2b, 436 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L10.2c, 366 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L10.3a, 584 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L10.3b, 530 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L10.3c, 461 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L10.3d, 262 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L10.3e, 301 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L10.3f, 499 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L11.1a, 991 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L11.2a, 1023 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L11.2b, 427 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L11.2c, 667 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L12.1a, 862 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L12.1b, 746 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L12.2a, 1065 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L12.2b, 313 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L12.2c, 428 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L13.1a, 818 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L13.2a, 613 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L13.2b, 312 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L14.1a, 716 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L14.2a, 739 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L14.2b, 481 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L15.1a, 917 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L15.1b, 314 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L15.2a, 642 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L15.2b, 189 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L15.2c, 599 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L15.2d, 308 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L15.3b, 486 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L15.3d, 209 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L15.3e, 350 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L15.3f, 261 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L16.1a, 721 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L16.1b, 480 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L16.1c, 554 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L16.2a, 712 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L16.2c, 281 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L17.2a, 355 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L17.2b, 207 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L17.2c, 178 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L17.2d, 200 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L18.1a, 633 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L18.2a, 614 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L18.2c, 223 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L18.2d, 200 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L18.2e, 364 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L18.3a, 266 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L18.3b, 340 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L19.1a, 766 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L19.1b, 734 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L19.2a, 659 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L19.2c, 621 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L19.2d, 703 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L19.2e, 565 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L19.2f, 313 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L19.3b, 448 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L2.1a, 330 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L2.2a, 205 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L20.1a, 986 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L20.2a, 506 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L20.2b, 731 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L20.2c, 1332 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L21.1a, 300 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L21.1b, 596 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L21.1c, 707 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L21.1d, 3464 bytes, 7 tape blocks
x ./usr/lib/learn/editor/L21.1e, 1206 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L3.1a, 493 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L3.1b, 922 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L30.1a, 816 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L30.1b, 486 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L30.2a, 650 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L30.2b, 426 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L30.2c, 521 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L30.2d, 307 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L30.2e, 335 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L30.2f, 327 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L30.2g, 344 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L30.2h, 394 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L31.1a, 637 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L31.2b, 348 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L31.2c, 1419 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L32.1a, 771 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L32.1b, 380 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L32.1c, 282 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L32.2a, 731 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L32.2b, 311 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L32.2c, 205 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L32.2d, 135 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L32.2e, 143 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L32.2f, 135 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L32.2g, 548 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L33.1a, 521 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L33.1b, 639 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L33.2a, 384 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L33.2b, 677 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L33.2c, 271 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L34.1a, 272 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L34.1b, 307 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L34.2a, 308 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L34.2b, 120 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L34.2c, 105 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L34.2d, 118 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L34.2e, 287 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L34.2f, 176 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L35.1a, 893 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L35.2a, 627 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L35.2b, 324 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L35.2c, 570 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L35.2d, 604 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L35.2e, 724 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L36.1a, 831 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L36.2a, 552 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L36.2b, 239 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L36.2c, 910 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L36.2d, 454 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L37.1a, 514 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L37.2a, 688 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L37.2b, 300 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L37.2c, 585 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L37.2d, 379 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L37.2e, 1246 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L37.2f, 607 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L38.1a, 764 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L38.2a, 404 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L38.2b, 425 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L39.1a, 427 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L4.1a, 618 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L4.1b, 160 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L4.2a, 361 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L4.2b, 135 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L40.1a, 660 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L40.1b, 972 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L40.2b, 622 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L40.2c, 525 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L40.2d, 717 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L41.1a, 1377 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L41.1b, 482 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L42.1a, 749 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L42.2a, 538 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L42.2b, 410 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L42.2c, 900 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L43.1a, 605 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L43.2a, 581 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L43.2b, 203 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L43.2c, 393 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L43.2d, 285 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L44.1a, 1242 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L44.1b, 1563 bytes, 4 tape blocks
x ./usr/lib/learn/editor/L44.1c, 901 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L44.1d, 703 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L44.1e, 751 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L44.1f, 754 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L44.1g, 2441 bytes, 5 tape blocks
x ./usr/lib/learn/editor/L44.1h, 483 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L44.1i, 383 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L45.1a, 845 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L45.1b, 1016 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L5.1a, 441 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L50.1a, 887 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L50.1b, 627 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L50.1c, 875 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L50.2c, 368 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L50.2d, 1368 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L50.2e, 747 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L50.2f, 2071 bytes, 5 tape blocks
x ./usr/lib/learn/editor/L50.2g, 2144 bytes, 5 tape blocks
x ./usr/lib/learn/editor/L51.1a, 577 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L51.2a, 3767 bytes, 8 tape blocks
x ./usr/lib/learn/editor/L51.2b, 353 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L51.2c, 4264 bytes, 9 tape blocks
x ./usr/lib/learn/editor/L52.1a, 1768 bytes, 4 tape blocks
x ./usr/lib/learn/editor/L52.1b, 1447 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L52.2a, 701 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L52.2b, 1588 bytes, 4 tape blocks
x ./usr/lib/learn/editor/L52.2c, 1350 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L53.1a, 539 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L53.2b, 18913 bytes, 37 tape blocks
x ./usr/lib/learn/editor/L54.1a, 1012 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L54.1b, 2145 bytes, 5 tape blocks
x ./usr/lib/learn/editor/L55.1a, 744 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L56.1a, 692 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L57.1a, 827 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L6.1a, 843 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L6.2a, 728 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L6.2b, 349 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L60.1a, 1221 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L60.1b, 1864 bytes, 4 tape blocks
x ./usr/lib/learn/editor/L60.2a, 2843 bytes, 6 tape blocks
x ./usr/lib/learn/editor/L60.2b, 5425 bytes, 11 tape blocks
x ./usr/lib/learn/editor/L60.2c, 2934 bytes, 6 tape blocks
x ./usr/lib/learn/editor/L60.2d, 2734 bytes, 6 tape blocks
x ./usr/lib/learn/editor/L61.1a, 621 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L62.1a, 849 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L62.2a, 673 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L62.2b, 1638 bytes, 4 tape blocks
x ./usr/lib/learn/editor/L62.2c, 486 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L63.1a, 881 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L63.1b, 213 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L63.1c, 207 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L63.1d, 135 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L63.1e, 159 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L64.1a, 757 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L64.1b, 271 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L65.1a, 1276 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L66.1a, 4279 bytes, 9 tape blocks
x ./usr/lib/learn/editor/L7.1a, 716 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L7.1b, 515 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L7.2c, 405 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L70.1a, 1043 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L70.2a, 1224 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L70.2b, 1703 bytes, 4 tape blocks
x ./usr/lib/learn/editor/L70.2c, 1577 bytes, 4 tape blocks
x ./usr/lib/learn/editor/L70.2d, 1961 bytes, 4 tape blocks
x ./usr/lib/learn/editor/L70.2e, 2066 bytes, 5 tape blocks
x ./usr/lib/learn/editor/L70.2f, 1579 bytes, 4 tape blocks
x ./usr/lib/learn/editor/L70.2g, 788 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L71.1a, 4658 bytes, 10 tape blocks
x ./usr/lib/learn/editor/L72.1a, 1068 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L72.2a, 1220 bytes, 3 tape blocks
x ./usr/lib/learn/editor/L72.2b, 963 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L72.2c, 758 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L72.2d, 739 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L73.1a, 784 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L74.1a, 2447 bytes, 5 tape blocks
x ./usr/lib/learn/editor/L8.1a, 698 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L8.1b, 398 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L8.2a, 726 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L8.2b, 359 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L8.2c, 523 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L9.1a, 542 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L9.2a, 518 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L9.2b, 438 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L9.2d, 697 bytes, 2 tape blocks
x ./usr/lib/learn/editor/L9.2e, 428 bytes, 1 tape blocks
x ./usr/lib/learn/editor/L9.3c, 568 bytes, 2 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_22of33_LEARN_4of4_BL-X327A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./usr/lib/learn/Linfo, 55 bytes, 1 tape blocks
x ./usr/lib/learn/Xinfo, 764 bytes, 2 tape blocks
x ./usr/lib/learn/files/L0, 14 bytes, 1 tape blocks
x ./usr/lib/learn/files/L0.1a, 957 bytes, 2 tape blocks
x ./usr/lib/learn/files/L0.1b, 837 bytes, 2 tape blocks
x ./usr/lib/learn/files/L0.1c, 603 bytes, 2 tape blocks
x ./usr/lib/learn/files/L0.1d, 635 bytes, 2 tape blocks
x ./usr/lib/learn/files/L1.1a, 214 bytes, 1 tape blocks
x ./usr/lib/learn/files/L1.2a, 296 bytes, 1 tape blocks
x ./usr/lib/learn/files/L1.2b, 539 bytes, 2 tape blocks
x ./usr/lib/learn/files/L10.1a, 548 bytes, 2 tape blocks
x ./usr/lib/learn/files/L10.2a, 299 bytes, 1 tape blocks
x ./usr/lib/learn/files/L10.2b, 474 bytes, 1 tape blocks
x ./usr/lib/learn/files/L10.3a, 397 bytes, 1 tape blocks
x ./usr/lib/learn/files/L10.3b, 267 bytes, 1 tape blocks
x ./usr/lib/learn/files/L10.3c, 333 bytes, 1 tape blocks
x ./usr/lib/learn/files/L10.3d, 404 bytes, 1 tape blocks
x ./usr/lib/learn/files/L11.1a, 942 bytes, 2 tape blocks
x ./usr/lib/learn/files/L11.2a, 317 bytes, 1 tape blocks
x ./usr/lib/learn/files/L11.2b, 469 bytes, 1 tape blocks
x ./usr/lib/learn/files/L11.3a, 412 bytes, 1 tape blocks
x ./usr/lib/learn/files/L11.3b, 194 bytes, 1 tape blocks
x ./usr/lib/learn/files/L11.3c, 396 bytes, 1 tape blocks
x ./usr/lib/learn/files/L12.1a, 1178 bytes, 3 tape blocks
x ./usr/lib/learn/files/L12.2a, 554 bytes, 2 tape blocks
x ./usr/lib/learn/files/L12.2b, 435 bytes, 1 tape blocks
x ./usr/lib/learn/files/L12.2c, 589 bytes, 2 tape blocks
x ./usr/lib/learn/files/L12.3a, 446 bytes, 1 tape blocks
x ./usr/lib/learn/files/L12.3b, 364 bytes, 1 tape blocks
x ./usr/lib/learn/files/L12.3c, 491 bytes, 1 tape blocks
x ./usr/lib/learn/files/L13.1a, 306 bytes, 1 tape blocks
x ./usr/lib/learn/files/L13.1b, 285 bytes, 1 tape blocks
x ./usr/lib/learn/files/L13.1c, 559 bytes, 2 tape blocks
x ./usr/lib/learn/files/L13.1d, 250 bytes, 1 tape blocks
x ./usr/lib/learn/files/L13.1e, 256 bytes, 1 tape blocks
x ./usr/lib/learn/files/L13.1f, 527 bytes, 2 tape blocks
x ./usr/lib/learn/files/L13.1g, 639 bytes, 2 tape blocks
x ./usr/lib/learn/files/L2.1a, 504 bytes, 1 tape blocks
x ./usr/lib/learn/files/L2.2a, 420 bytes, 1 tape blocks
x ./usr/lib/learn/files/L2.2b, 605 bytes, 2 tape blocks
x ./usr/lib/learn/files/L3.1a, 706 bytes, 2 tape blocks
x ./usr/lib/learn/files/L3.2a, 616 bytes, 2 tape blocks
x ./usr/lib/learn/files/L3.2b, 749 bytes, 2 tape blocks
x ./usr/lib/learn/files/L3.3a, 360 bytes, 1 tape blocks
x ./usr/lib/learn/files/L3.3b, 547 bytes, 2 tape blocks
x ./usr/lib/learn/files/L4.1a, 351 bytes, 1 tape blocks
x ./usr/lib/learn/files/L4.2a, 429 bytes, 1 tape blocks
x ./usr/lib/learn/files/L4.2b, 493 bytes, 1 tape blocks
x ./usr/lib/learn/files/L4.3a, 584 bytes, 2 tape blocks
x ./usr/lib/learn/files/L4.3b, 359 bytes, 1 tape blocks
x ./usr/lib/learn/files/L4.3c, 385 bytes, 1 tape blocks
x ./usr/lib/learn/files/L5.1a, 918 bytes, 2 tape blocks
x ./usr/lib/learn/files/L5.1b, 363 bytes, 1 tape blocks
x ./usr/lib/learn/files/L5.1c, 523 bytes, 2 tape blocks
x ./usr/lib/learn/files/L5.1d, 159 bytes, 1 tape blocks
x ./usr/lib/learn/files/L5.1e, 664 bytes, 2 tape blocks
x ./usr/lib/learn/files/L6.1a, 875 bytes, 2 tape blocks
x ./usr/lib/learn/files/L6.1b, 222 bytes, 1 tape blocks
x ./usr/lib/learn/files/L6.1c, 246 bytes, 1 tape blocks
x ./usr/lib/learn/files/L6.1d, 291 bytes, 1 tape blocks
x ./usr/lib/learn/files/L6.1e, 262 bytes, 1 tape blocks
x ./usr/lib/learn/files/L6.2a, 709 bytes, 2 tape blocks
x ./usr/lib/learn/files/L6.2b, 511 bytes, 1 tape blocks
x ./usr/lib/learn/files/L7.1a, 876 bytes, 2 tape blocks
x ./usr/lib/learn/files/L7.2a, 677 bytes, 2 tape blocks
x ./usr/lib/learn/files/L7.2b, 727 bytes, 2 tape blocks
x ./usr/lib/learn/files/L7.3a, 737 bytes, 2 tape blocks
x ./usr/lib/learn/files/L7.3b, 314 bytes, 1 tape blocks
x ./usr/lib/learn/files/L7.3c, 459 bytes, 1 tape blocks
x ./usr/lib/learn/files/L8.1a, 429 bytes, 1 tape blocks
x ./usr/lib/learn/files/L8.2a, 186 bytes, 1 tape blocks
x ./usr/lib/learn/files/L8.2b, 346 bytes, 1 tape blocks
x ./usr/lib/learn/files/L8.2c, 421 bytes, 1 tape blocks
x ./usr/lib/learn/files/L9.1a, 851 bytes, 2 tape blocks
x ./usr/lib/learn/files/L9.2a, 595 bytes, 2 tape blocks
x ./usr/lib/learn/files/L9.2b, 500 bytes, 1 tape blocks
x ./usr/lib/learn/files/L9.2c, 356 bytes, 1 tape blocks
x ./usr/lib/learn/files.a, 38744 bytes, 76 tape blocks
x ./usr/lib/learn/lcount, 2850 bytes, 6 tape blocks
x ./usr/lib/learn/log/C, 360 bytes, 1 tape blocks
x ./usr/lib/learn/log/macros, 225 bytes, 1 tape blocks
x ./usr/lib/learn/log/editor, 3735 bytes, 8 tape blocks
x ./usr/lib/learn/log/files, 450 bytes, 1 tape blocks
x ./usr/lib/learn/log/morefiles, 2340 bytes, 5 tape blocks
x ./usr/lib/learn/macros/L0, 13 bytes, 1 tape blocks
x ./usr/lib/learn/macros/L1.1a, 767 bytes, 2 tape blocks
x ./usr/lib/learn/macros/L10.1a, 1794 bytes, 4 tape blocks
x ./usr/lib/learn/macros/L11.1a, 3405 bytes, 7 tape blocks
x ./usr/lib/learn/macros/L12.1a, 3981 bytes, 8 tape blocks
x ./usr/lib/learn/macros/L13.1a, 3745 bytes, 8 tape blocks
x ./usr/lib/learn/macros/L14.1a, 3428 bytes, 7 tape blocks
x ./usr/lib/learn/macros/L15.1a, 13221 bytes, 26 tape blocks
x ./usr/lib/learn/macros/L2.1a, 781 bytes, 2 tape blocks
x ./usr/lib/learn/macros/L3.1a, 3073 bytes, 7 tape blocks
x ./usr/lib/learn/macros/L4.1a, 2617 bytes, 6 tape blocks
x ./usr/lib/learn/macros/L5.1a, 2611 bytes, 6 tape blocks
x ./usr/lib/learn/macros/L6.1a, 2766 bytes, 6 tape blocks
x ./usr/lib/learn/macros/L7.1a, 2985 bytes, 6 tape blocks
x ./usr/lib/learn/macros/L8.1a, 3084 bytes, 7 tape blocks
x ./usr/lib/learn/macros/L9.1a, 2971 bytes, 6 tape blocks
x ./usr/lib/learn/macros.a, 51672 bytes, 101 tape blocks
x ./usr/lib/learn/play/dummy, 22 bytes, 1 tape blocks
x ./usr/lib/learn/tee, 494 bytes, 1 tape blocks
# sync
# 
```

The online manuals should be written to the /usr partition
so they end up in /usr/man heirarchy.
The remaining tar volumes are all relative directories,
so restore 23 to 33 into /usr.

```
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_23of33_ON-LINE_MANUALS_1of4_BL-X328A-BC.IMG 
RQ3: Unit is read only
sim> c

# cd /usr
# tar xvf /dev/rx3
Tar: blocksize = 20
x ./man/man0/mkpref, 15 bytes, 1 tape blocks
x ./man/man0/mktoc, 22 bytes, 1 tape blocks
x ./man/man0/ptxmac, 252 bytes, 1 tape blocks
x ./man/man0/permindex, 73136 bytes, 143 tape blocks
x ./man/man0/pref, 834 bytes, 2 tape blocks
x ./man/man0/title, 360 bytes, 1 tape blocks
x ./man/man0/xx, 109 bytes, 1 tape blocks
x ./man/man0/intro, 22538 bytes, 45 tape blocks
x ./man/man0/toc, 16416 bytes, 33 tape blocks
x ./man/man0/mkintro, 37 bytes, 1 tape blocks
x ./man/man2/mpx.2, 10692 bytes, 21 tape blocks
x ./man/man2/mpxcall.2, 674 bytes, 2 tape blocks
x ./man/man2/pkon.2, 1121 bytes, 3 tape blocks
x ./man/man2/phys.2, 1368 bytes, 3 tape blocks
x ./man/man2/lock.2, 552 bytes, 2 tape blocks
x ./man/man2/exec.2, 5476 bytes, 11 tape blocks
x ./man/man2/indir.2, 565 bytes, 2 tape blocks
x ./man/man2/intro.2, 8284 bytes, 17 tape blocks
x ./man/man2/brk.2, 1401 bytes, 3 tape blocks
x ./man/man2/wait.2, 1548 bytes, 4 tape blocks
x ./man/man2/unlink.2, 908 bytes, 2 tape blocks
x ./man/man2/umask.2, 731 bytes, 2 tape blocks
x ./man/man2/sync.2, 558 bytes, 2 tape blocks
x ./man/man2/stime.2, 469 bytes, 1 tape blocks
x ./man/man2/setuid.2, 566 bytes, 2 tape blocks
x ./man/man2/read.2, 1060 bytes, 3 tape blocks
x ./man/man2/ptrace.2, 4945 bytes, 10 tape blocks
x ./man/man2/profil.2, 1246 bytes, 3 tape blocks
x ./man/man2/pipe.2, 1431 bytes, 3 tape blocks
x ./man/man2/pause.2, 323 bytes, 1 tape blocks
x ./man/man2/open.2, 899 bytes, 2 tape blocks
x ./man/man2/mknod.2, 917 bytes, 2 tape blocks
x ./man/man2/lseek.2, 1267 bytes, 3 tape blocks
x ./man/man2/link.2, 682 bytes, 2 tape blocks
x ./man/man2/ioctl.2, 1652 bytes, 4 tape blocks
x ./man/man2/getuid.2, 824 bytes, 2 tape blocks
x ./man/man2/getpid.2, 315 bytes, 1 tape blocks
x ./man/man2/fork.2, 1204 bytes, 3 tape blocks
x ./man/man2/exit.2, 625 bytes, 2 tape blocks
x ./man/man2/dup.2, 1088 bytes, 3 tape blocks
x ./man/man2/creat.2, 1374 bytes, 3 tape blocks
x ./man/man2/close.2, 778 bytes, 2 tape blocks
x ./man/man2/chdir.2, 796 bytes, 2 tape blocks
x ./man/man2/alarm.2, 948 bytes, 2 tape blocks
x ./man/man2/access.2, 1159 bytes, 3 tape blocks
x ./man/man2/write.2, 848 bytes, 2 tape blocks
x ./man/man2/times.2, 680 bytes, 2 tape blocks
x ./man/man2/time.2, 1076 bytes, 3 tape blocks
x ./man/man2/utime.2, 528 bytes, 2 tape blocks
x ./man/man2/stat.2, 1986 bytes, 4 tape blocks
x ./man/man2/signal.2, 3129 bytes, 7 tape blocks
x ./man/man2/nice.2, 904 bytes, 2 tape blocks
x ./man/man2/mount.2, 1816 bytes, 4 tape blocks
x ./man/man2/kill.2, 1030 bytes, 3 tape blocks
x ./man/man2/chown.2, 635 bytes, 2 tape blocks
x ./man/man2/chmod.2, 1500 bytes, 3 tape blocks
x ./man/man2/acct.2, 894 bytes, 2 tape blocks
x ./man/man2/fperr.2, 439 bytes, 1 tape blocks
x ./man/man3/qsort.3, 683 bytes, 2 tape blocks
x ./man/man3/perror.3, 1077 bytes, 3 tape blocks
x ./man/man3/sinh.3m, 424 bytes, 1 tape blocks
x ./man/man3/setbuf.3s, 836 bytes, 2 tape blocks
x ./man/man3/scanf.3s, 5597 bytes, 11 tape blocks
x ./man/man3/nlist.3, 975 bytes, 2 tape blocks
x ./man/man3/monitor.3, 1593 bytes, 4 tape blocks
x ./man/man3/puts.3s, 662 bytes, 2 tape blocks
x ./man/man3/printf.3s, 4352 bytes, 9 tape blocks
x ./man/man3/popen.3s, 1439 bytes, 3 tape blocks
x ./man/man3/mktemp.3, 392 bytes, 1 tape blocks
x ./man/man3/malloc.3, 2237 bytes, 5 tape blocks
x ./man/man3/l3tol.3, 616 bytes, 2 tape blocks
x ./man/man3/intro.3, 2328 bytes, 5 tape blocks
x ./man/man3/hypot.3m, 354 bytes, 1 tape blocks
x ./man/man3/gets.3s, 937 bytes, 2 tape blocks
x ./man/man3/getpwent.3, 1147 bytes, 3 tape blocks
x ./man/man3/getpw.3, 428 bytes, 1 tape blocks
x ./man/man3/getpass.3, 527 bytes, 2 tape blocks
x ./man/man3/getlogin.3, 757 bytes, 2 tape blocks
x ./man/man3/getgrent.3, 1465 bytes, 3 tape blocks
x ./man/man3/getenv.3, 339 bytes, 1 tape blocks
x ./man/man3/fseek.3s, 1024 bytes, 2 tape blocks
x ./man/man3/fopen.3s, 1382 bytes, 3 tape blocks
x ./man/man3/floor.3m, 453 bytes, 1 tape blocks
x ./man/man3/exp.3m, 1007 bytes, 2 tape blocks
x ./man/man3/frexp.3, 654 bytes, 2 tape blocks
x ./man/man3/getc.3s, 1435 bytes, 3 tape blocks
x ./man/man3/end.3, 781 bytes, 2 tape blocks
x ./man/man3/ecvt.3, 1350 bytes, 3 tape blocks
x ./man/man3/ctype.3, 1138 bytes, 3 tape blocks
x ./man/man3/abort.3, 346 bytes, 1 tape blocks
x ./man/man3/stdio.3s, 2039 bytes, 4 tape blocks
x ./man/man3/ctime.3, 2177 bytes, 5 tape blocks
x ./man/man3/crypt.3, 1740 bytes, 4 tape blocks
x ./man/man3/atof.3, 795 bytes, 2 tape blocks
x ./man/man3/abs.3, 255 bytes, 1 tape blocks
x ./man/man3/assert.3x, 593 bytes, 2 tape blocks
x ./man/man3/fclose.3s, 771 bytes, 2 tape blocks
x ./man/man3/mp.3x, 1471 bytes, 3 tape blocks
x ./man/man3/dbm.3x, 2899 bytes, 6 tape blocks
x ./man/man3/pkopen.3, 1587 bytes, 4 tape blocks
x ./man/man3/plot.3x, 1169 bytes, 3 tape blocks
x ./man/man3/fread.3s, 792 bytes, 2 tape blocks
x ./man/man3/ferror.3s, 900 bytes, 2 tape blocks
x ./man/man3/sin.3m, 1111 bytes, 3 tape blocks
x ./man/man3/j0.3m, 574 bytes, 2 tape blocks
x ./man/man3/putc.3s, 1809 bytes, 4 tape blocks
x ./man/man3/ttyname.3, 889 bytes, 2 tape blocks
x ./man/man3/system.3, 466 bytes, 1 tape blocks
x ./man/man3/swab.3, 349 bytes, 1 tape blocks
x ./man/man3/string.3, 1707 bytes, 4 tape blocks
x ./man/man3/sleep.3, 784 bytes, 2 tape blocks
x ./man/man3/setjmp.3, 797 bytes, 2 tape blocks
x ./man/man3/ungetc.3s, 691 bytes, 2 tape blocks
x ./man/man3/rand.3, 486 bytes, 1 tape blocks
x ./man/man3/termlib.3, 3410 bytes, 7 tape blocks
x ./man/man3/curses.3, 3302 bytes, 7 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_24of33_ON-LINE_MANUALS_2of4_BL-X329A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./man/man1/ac.1m, 1081 bytes, 3 tape blocks
x ./man/man1/arcv.1m, 466 bytes, 1 tape blocks
x ./man/man1/bufstat.1m, 1327 bytes, 3 tape blocks
x ./man/man1/cda.1m, 354 bytes, 1 tape blocks
x ./man/man1/clri.1m, 1203 bytes, 3 tape blocks
x ./man/man1/csf.1m, 2034 bytes, 4 tape blocks
x ./man/man1/dcheck.1m, 1594 bytes, 4 tape blocks
x ./man/man1/df.1m, 387 bytes, 1 tape blocks
x ./man/man1/dump.1m, 3661 bytes, 8 tape blocks
x ./man/man1/dumpdir.1m, 758 bytes, 2 tape blocks
x ./man/man1/fsck.1m, 4532 bytes, 9 tape blocks
x ./man/man1/fsdb.1m, 5901 bytes, 12 tape blocks
x ./man/man1/icheck.1m, 2736 bytes, 6 tape blocks
x ./man/man1/iostat.1m, 3511 bytes, 7 tape blocks
x ./man/man1/ipatch.1m, 1478 bytes, 3 tape blocks
x ./man/man1/logins.1m, 638 bytes, 2 tape blocks
x ./man/man1/lpset.1m, 1429 bytes, 3 tape blocks
x ./man/man1/memstat.1m, 1612 bytes, 4 tape blocks
x ./man/man1/mkconf.1m, 3908 bytes, 8 tape blocks
x ./man/man1/mkfs.1m, 3277 bytes, 7 tape blocks
x ./man/man1/mknod.1m, 619 bytes, 2 tape blocks
x ./man/man1/mount.1m, 1766 bytes, 4 tape blocks
x ./man/man1/ncheck.1m, 991 bytes, 2 tape blocks
x ./man/man1/pstat.1m, 5539 bytes, 11 tape blocks
x ./man/man1/quot.1m, 1171 bytes, 3 tape blocks
x ./man/man1/rasize.1m, 1074 bytes, 3 tape blocks
x ./man/man1/restor.1m, 3493 bytes, 7 tape blocks
x ./man/man1/rx2fmt.1m, 483 bytes, 1 tape blocks
x ./man/man1/sa.1m, 2285 bytes, 5 tape blocks
x ./man/man1/sync.1m, 289 bytes, 1 tape blocks
x ./man/man1/sysgen.1m, 231 bytes, 1 tape blocks
x ./man/man1/tss.1m, 593 bytes, 2 tape blocks
x ./man/man1/wall.1m, 475 bytes, 1 tape blocks
x ./man/man1/graph.1g, 2479 bytes, 5 tape blocks
x ./man/man1/plot.1g, 1145 bytes, 3 tape blocks
x ./man/man1/spline.1g, 3367 bytes, 7 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_25of33_ON-LINE_MANUALS_3of4_BL-X330A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./man/man1/adb.1, 15135 bytes, 30 tape blocks
x ./man/man1/ar.1, 2680 bytes, 6 tape blocks
x ./man/man1/as.1, 1895 bytes, 4 tape blocks
x ./man/man1/at.1, 1912 bytes, 4 tape blocks
x ./man/man1/awk.1, 5490 bytes, 11 tape blocks
x ./man/man1/bas.1, 6921 bytes, 14 tape blocks
x ./man/man1/basename.1, 568 bytes, 2 tape blocks
x ./man/man1/bc.1, 3198 bytes, 7 tape blocks
x ./man/man1/cal.1, 558 bytes, 2 tape blocks
x ./man/man1/calendar.1, 1022 bytes, 2 tape blocks
x ./man/man1/cat.1, 661 bytes, 2 tape blocks
x ./man/man1/cb.1, 246 bytes, 1 tape blocks
x ./man/man1/cc.1, 5167 bytes, 11 tape blocks
x ./man/man1/cd.1, 435 bytes, 1 tape blocks
x ./man/man1/chmod.1, 1982 bytes, 4 tape blocks
x ./man/man1/chown.1, 626 bytes, 2 tape blocks
x ./man/man1/cmp.1, 775 bytes, 2 tape blocks
x ./man/man1/col.1, 2049 bytes, 5 tape blocks
x ./man/man1/comm.1, 676 bytes, 2 tape blocks
x ./man/man1/cp.1, 452 bytes, 1 tape blocks
x ./man/man1/crypt.1, 2341 bytes, 5 tape blocks
x ./man/man1/date.1, 1078 bytes, 3 tape blocks
x ./man/man1/dc.1, 4237 bytes, 9 tape blocks
x ./man/man1/dd.1, 4010 bytes, 8 tape blocks
x ./man/man1/deroff.1, 1146 bytes, 3 tape blocks
x ./man/man1/diff.1, 2386 bytes, 5 tape blocks
x ./man/man1/diff3.1, 1601 bytes, 4 tape blocks
x ./man/man1/du.1, 756 bytes, 2 tape blocks
x ./man/man1/echo.1, 444 bytes, 1 tape blocks
x ./man/man1/ed.1, 15767 bytes, 31 tape blocks
x ./man/man1/eqn.1, 5358 bytes, 11 tape blocks
x ./man/man1/ex.1, 3101 bytes, 7 tape blocks
x ./man/man1/expr.1, 1996 bytes, 4 tape blocks
x ./man/man1/f77.1, 3399 bytes, 7 tape blocks
x ./man/man1/factor.1, 1099 bytes, 3 tape blocks
x ./man/man1/file.1, 384 bytes, 1 tape blocks
x ./man/man1/find.1, 2994 bytes, 6 tape blocks
x ./man/man1/grep.1, 3718 bytes, 8 tape blocks
x ./man/man1/intro.1, 1245 bytes, 3 tape blocks
x ./man/man1/join.1, 1775 bytes, 4 tape blocks
x ./man/man1/kill.1, 965 bytes, 2 tape blocks
x ./man/man1/ld.1, 5075 bytes, 10 tape blocks
x ./man/man1/learn.1, 1482 bytes, 3 tape blocks
x ./man/man1/lex.1, 1323 bytes, 3 tape blocks
x ./man/man1/lint.1, 3033 bytes, 6 tape blocks
x ./man/man1/ln.1, 718 bytes, 2 tape blocks
x ./man/man1/login.1, 1439 bytes, 3 tape blocks
x ./man/man1/look.1, 617 bytes, 2 tape blocks
x ./man/man1/lorder.1, 818 bytes, 2 tape blocks
x ./man/man1/lpr.1, 1118 bytes, 3 tape blocks
x ./man/man1/ls.1, 5646 bytes, 12 tape blocks
x ./man/man1/m4.1, 4840 bytes, 10 tape blocks
x ./man/man1/mail.1, 2528 bytes, 5 tape blocks
x ./man/man1/make.1, 4317 bytes, 9 tape blocks
x ./man/man1/man.1, 1857 bytes, 4 tape blocks
x ./man/man1/mesg.1, 491 bytes, 1 tape blocks
x ./man/man1/mkdir.1, 490 bytes, 1 tape blocks
x ./man/man1/more.1, 8034 bytes, 16 tape blocks
x ./man/man1/mv.1, 1010 bytes, 2 tape blocks
x ./man/man1/newgrp.1, 663 bytes, 2 tape blocks
x ./man/man1/nice.1, 1012 bytes, 2 tape blocks
x ./man/man1/nm.1, 1248 bytes, 3 tape blocks
x ./man/man1/od.1, 1230 bytes, 3 tape blocks
x ./man/man1/passwd.1, 828 bytes, 2 tape blocks
x ./man/man1/pr.1, 1303 bytes, 3 tape blocks
x ./man/man1/prep.1, 1156 bytes, 3 tape blocks
x ./man/man1/prof.1, 1546 bytes, 4 tape blocks
x ./man/man1/ps.1, 3065 bytes, 6 tape blocks
x ./man/man1/ptx.1, 2396 bytes, 5 tape blocks
x ./man/man1/pwd.1, 170 bytes, 1 tape blocks
x ./man/man1/ranlib.1, 786 bytes, 2 tape blocks
x ./man/man1/ratfor.1, 1501 bytes, 3 tape blocks
x ./man/man1/refer.1, 3357 bytes, 7 tape blocks
x ./man/man1/rev.1, 258 bytes, 1 tape blocks
x ./man/man1/rm.1, 1343 bytes, 3 tape blocks
x ./man/man1/roff.1, 6401 bytes, 13 tape blocks
x ./man/man1/sed.1, 5978 bytes, 12 tape blocks
x ./man/man1/sh.1, 19174 bytes, 38 tape blocks
x ./man/man1/size.1, 571 bytes, 2 tape blocks
x ./man/man1/sleep.1, 452 bytes, 1 tape blocks
x ./man/man1/sort.1, 3766 bytes, 8 tape blocks
x ./man/man1/spell.1, 3046 bytes, 6 tape blocks
x ./man/man1/split.1, 496 bytes, 1 tape blocks
x ./man/man1/strip.1, 506 bytes, 1 tape blocks
x ./man/man1/struct.1, 2331 bytes, 5 tape blocks
x ./man/man1/stty.1, 4430 bytes, 9 tape blocks
x ./man/man1/su.1, 552 bytes, 2 tape blocks
x ./man/man1/sum.1, 451 bytes, 1 tape blocks
x ./man/man1/tabs.1, 399 bytes, 1 tape blocks
x ./man/man1/tail.1, 779 bytes, 2 tape blocks
x ./man/man1/tar.1, 5068 bytes, 10 tape blocks
x ./man/man1/tbl.1, 4690 bytes, 10 tape blocks
x ./man/man1/tc.1, 1421 bytes, 3 tape blocks
x ./man/man1/tee.1, 334 bytes, 1 tape blocks
x ./man/man1/test.1, 1816 bytes, 4 tape blocks
x ./man/man1/time.1, 690 bytes, 2 tape blocks
x ./man/man1/tk.1, 997 bytes, 2 tape blocks
x ./man/man1/touch.1, 365 bytes, 1 tape blocks
x ./man/man1/tp.1, 3729 bytes, 8 tape blocks
x ./man/man1/tr.1, 1507 bytes, 3 tape blocks
x ./man/man1/troff.1, 3737 bytes, 8 tape blocks
x ./man/man1/true.1, 372 bytes, 1 tape blocks
x ./man/man1/tsort.1, 708 bytes, 2 tape blocks
x ./man/man1/tty.1, 206 bytes, 1 tape blocks
x ./man/man1/uniq.1, 1280 bytes, 3 tape blocks
x ./man/man1/units.1, 1572 bytes, 4 tape blocks
x ./man/man1/vi.1, 1777 bytes, 4 tape blocks
x ./man/man1/wait.1, 479 bytes, 1 tape blocks
x ./man/man1/wc.1, 434 bytes, 1 tape blocks
x ./man/man1/who.1, 1008 bytes, 2 tape blocks
x ./man/man1/write.1, 1464 bytes, 3 tape blocks
x ./man/man1/xsend.1, 1018 bytes, 2 tape blocks
x ./man/man1/yacc.1, 1931 bytes, 4 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_26of33_ON-LINE_MANUALS_4of4_BL-X331A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./man/man4/hx.4, 1930 bytes, 4 tape blocks
x ./man/man4/ml.4, 1926 bytes, 4 tape blocks
x ./man/man4/ts.4, 2293 bytes, 5 tape blocks
x ./man/man4/rl.4, 1755 bytes, 4 tape blocks
x ./man/man4/hp.4, 2000 bytes, 4 tape blocks
x ./man/man4/hk.4, 1253 bytes, 3 tape blocks
x ./man/man4/lp.4, 1464 bytes, 3 tape blocks
x ./man/man4/tm.4, 2157 bytes, 5 tape blocks
x ./man/man4/rp.4, 1542 bytes, 4 tape blocks
x ./man/man4/dh.4, 766 bytes, 2 tape blocks
x ./man/man4/hs.4, 1423 bytes, 3 tape blocks
x ./man/man4/du.4, 958 bytes, 2 tape blocks
x ./man/man4/pk.4, 1909 bytes, 4 tape blocks
x ./man/man4/ht.4, 2951 bytes, 6 tape blocks
x ./man/man4/rk.4, 1563 bytes, 4 tape blocks
x ./man/man4/null.4, 185 bytes, 1 tape blocks
x ./man/man4/mem.4, 835 bytes, 2 tape blocks
x ./man/man4/dn.4, 765 bytes, 2 tape blocks
x ./man/man4/tc.4, 398 bytes, 1 tape blocks
x ./man/man4/cat.4, 711 bytes, 2 tape blocks
x ./man/man4/tty.4, 14823 bytes, 29 tape blocks
x ./man/man4/ra.4, 1549 bytes, 4 tape blocks
./man/man4/hm.4 linked to ./man/man4/hp.4
./man/man4/dz.4 linked to ./man/man4/dh.4
x ./man/man5/termcap.5, 25158 bytes, 50 tape blocks
x ./man/man5/mpxio.5, 4032 bytes, 8 tape blocks
x ./man/man5/ttys.5, 1056 bytes, 3 tape blocks
x ./man/man5/plot.5, 2658 bytes, 6 tape blocks
x ./man/man5/mtab.5, 700 bytes, 2 tape blocks
x ./man/man5/group.5, 716 bytes, 2 tape blocks
x ./man/man5/environ.5, 1187 bytes, 3 tape blocks
x ./man/man5/utmp.5, 1215 bytes, 3 tape blocks
x ./man/man5/types.5, 769 bytes, 2 tape blocks
x ./man/man5/tp.5, 1478 bytes, 3 tape blocks
x ./man/man5/passwd.5, 1092 bytes, 3 tape blocks
x ./man/man5/dump.5, 3340 bytes, 7 tape blocks
x ./man/man5/dir.5, 970 bytes, 2 tape blocks
x ./man/man5/core.5, 989 bytes, 2 tape blocks
x ./man/man5/ar.5, 1067 bytes, 3 tape blocks
x ./man/man5/acct.5, 668 bytes, 2 tape blocks
x ./man/man5/a.out.5, 4415 bytes, 9 tape blocks
x ./man/man5/ttytype.5, 458 bytes, 1 tape blocks
x ./man/man5/tar.5, 824 bytes, 2 tape blocks
x ./man/man5/fstab.5, 915 bytes, 2 tape blocks
x ./man/man5/tsaf.5, 561 bytes, 2 tape blocks
x ./man/man5/filsys.5, 5895 bytes, 12 tape blocks
x ./man/man6/checkers.6, 1158 bytes, 3 tape blocks
x ./man/man6/wump.6, 745 bytes, 2 tape blocks
x ./man/man6/ttt.6, 534 bytes, 2 tape blocks
x ./man/man6/reversi.6, 2537 bytes, 5 tape blocks
x ./man/man6/quiz.6, 1639 bytes, 4 tape blocks
x ./man/man6/moo.6, 461 bytes, 1 tape blocks
x ./man/man6/maze.6, 218 bytes, 1 tape blocks
x ./man/man6/words.6, 797 bytes, 2 tape blocks
x ./man/man6/cubic.6, 24 bytes, 1 tape blocks
x ./man/man6/ching.6, 2656 bytes, 6 tape blocks
x ./man/man6/chess.6, 732 bytes, 2 tape blocks
x ./man/man6/bj.6, 1966 bytes, 4 tape blocks
x ./man/man6/banner.6, 208 bytes, 1 tape blocks
x ./man/man6/backgammon.6, 182 bytes, 1 tape blocks
x ./man/man6/arithmetic.6, 1639 bytes, 4 tape blocks
x ./man/man6/bcd.6, 286 bytes, 1 tape blocks
x ./man/man7/hier.7, 6537 bytes, 13 tape blocks
x ./man/man7/eqnchar.7, 4386 bytes, 9 tape blocks
x ./man/man7/term.7, 1334 bytes, 3 tape blocks
x ./man/man7/ms.7, 6297 bytes, 13 tape blocks
x ./man/man7/man.7, 3141 bytes, 7 tape blocks
x ./man/man7/ascii.7, 1303 bytes, 3 tape blocks
x ./man/man7/greek.7, 991 bytes, 2 tape blocks
x ./man/man8/shutdown.8, 990 bytes, 2 tape blocks
x ./man/man8/dmesg.8, 1464 bytes, 3 tape blocks
x ./man/man8/boot.8, 453 bytes, 1 tape blocks
x ./man/man8/cron.8, 1262 bytes, 3 tape blocks
x ./man/man8/init.8, 2500 bytes, 5 tape blocks
x ./man/man8/getty.8, 2008 bytes, 4 tape blocks
x ./man/man8/crash.8, 283 bytes, 1 tape blocks
x ./man/man8/makekey.8, 1448 bytes, 3 tape blocks
x ./man/man8/update.8, 809 bytes, 2 tape blocks
x ./man/man8/lpd.8, 1908 bytes, 4 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_27of33_DOCUMENTS_1of7_BL-X332A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./doc/README, 314 bytes, 1 tape blocks
x ./doc/adb/tut, 30140 bytes, 59 tape blocks
x ./doc/adb/tut1, 14255 bytes, 28 tape blocks
x ./doc/adv.ed/ae.mac, 822 bytes, 2 tape blocks
x ./doc/adv.ed/ae0, 898 bytes, 2 tape blocks
x ./doc/adv.ed/ae1, 1367 bytes, 3 tape blocks
x ./doc/adv.ed/ae2, 22045 bytes, 44 tape blocks
x ./doc/adv.ed/ae3, 10731 bytes, 21 tape blocks
x ./doc/adv.ed/ae4, 4173 bytes, 9 tape blocks
x ./doc/adv.ed/ae5, 5953 bytes, 12 tape blocks
x ./doc/adv.ed/ae6, 9296 bytes, 19 tape blocks
x ./doc/adv.ed/ae7, 4410 bytes, 9 tape blocks
x ./doc/adv.ed/ae9, 429 bytes, 1 tape blocks
x ./doc/assembler, 29586 bytes, 58 tape blocks
x ./doc/awk, 26452 bytes, 52 tape blocks
x ./doc/bc, 28742 bytes, 57 tape blocks
x ./doc/beginners/u.mac, 643 bytes, 2 tape blocks
x ./doc/beginners/u0, 867 bytes, 2 tape blocks
x ./doc/beginners/u1, 12934 bytes, 26 tape blocks
x ./doc/beginners/u2, 24229 bytes, 48 tape blocks
x ./doc/beginners/u3, 7966 bytes, 16 tape blocks
x ./doc/beginners/u4, 6907 bytes, 14 tape blocks
x ./doc/beginners/u5, 3960 bytes, 8 tape blocks
x ./doc/cacm/p.mac, 192 bytes, 1 tape blocks
x ./doc/cacm/p1, 16189 bytes, 32 tape blocks
x ./doc/cacm/p2, 12911 bytes, 26 tape blocks
x ./doc/cacm/p3, 4425 bytes, 9 tape blocks
x ./doc/cacm/p4, 14217 bytes, 28 tape blocks
x ./doc/cacm/p5, 7380 bytes, 15 tape blocks
x ./doc/cacm/p6, 1369 bytes, 3 tape blocks
x ./doc/cman, 92 bytes, 1 tape blocks
x ./doc/ctour/cdoc0, 190 bytes, 1 tape blocks
x ./doc/ctour/cdoc1, 9432 bytes, 19 tape blocks
x ./doc/ctour/cdoc2, 5407 bytes, 11 tape blocks
x ./doc/ctour/cdoc3, 24673 bytes, 49 tape blocks
x ./doc/ctour/cdoc4, 3932 bytes, 8 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_28of33_DOCUMENTS_2of7_BL-X333A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./doc/dc, 23784 bytes, 47 tape blocks
x ./doc/edtut/e.mac, 506 bytes, 1 tape blocks
x ./doc/edtut/e0, 645 bytes, 2 tape blocks
x ./doc/edtut/e1, 4946 bytes, 10 tape blocks
x ./doc/edtut/e2, 6994 bytes, 14 tape blocks
x ./doc/edtut/e3, 7129 bytes, 14 tape blocks
x ./doc/edtut/e4, 5619 bytes, 11 tape blocks
x ./doc/edtut/e5, 4403 bytes, 9 tape blocks
x ./doc/edtut/e6, 4015 bytes, 8 tape blocks
x ./doc/edtut/e7, 3144 bytes, 7 tape blocks
x ./doc/eqn/e.mac, 795 bytes, 2 tape blocks
x ./doc/eqn/e0, 1397 bytes, 3 tape blocks
x ./doc/eqn/e1, 1661 bytes, 4 tape blocks
x ./doc/eqn/e2, 1232 bytes, 3 tape blocks
x ./doc/eqn/e3, 4557 bytes, 9 tape blocks
x ./doc/eqn/e4, 9242 bytes, 19 tape blocks
x ./doc/eqn/e5, 5373 bytes, 11 tape blocks
x ./doc/eqn/e6, 4979 bytes, 10 tape blocks
x ./doc/eqn/e7, 2017 bytes, 4 tape blocks
x ./doc/eqn/g.mac, 243 bytes, 1 tape blocks
x ./doc/eqn/g0, 1827 bytes, 4 tape blocks
x ./doc/eqn/g1, 9818 bytes, 20 tape blocks
x ./doc/eqn/g2, 9286 bytes, 19 tape blocks
x ./doc/eqn/g3, 4242 bytes, 9 tape blocks
x ./doc/eqn/g4, 4761 bytes, 10 tape blocks
x ./doc/eqn/g5, 1959 bytes, 4 tape blocks
x ./doc/f77, 52340 bytes, 103 tape blocks
x ./doc/implement, 34660 bytes, 68 tape blocks
x ./doc/index, 6883 bytes, 14 tape blocks
x ./doc/iosys, 28162 bytes, 56 tape blocks
x ./doc/learn/p0, 1464 bytes, 3 tape blocks
x ./doc/learn/p2, 7974 bytes, 16 tape blocks
x ./doc/learn/p3, 6180 bytes, 13 tape blocks
x ./doc/learn/p4, 1799 bytes, 4 tape blocks
x ./doc/learn/p5, 12090 bytes, 24 tape blocks
x ./doc/learn/p6, 5147 bytes, 11 tape blocks
x ./doc/learn/p7, 556 bytes, 2 tape blocks
x ./doc/lex, 50672 bytes, 99 tape blocks
x ./doc/lint, 32431 bytes, 64 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_29of33_DOCUMENTS_3of7_BL-X334A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./doc/m4, 18275 bytes, 36 tape blocks
x ./doc/make, 23641 bytes, 47 tape blocks
x ./doc/msmacros/ms, 20808 bytes, 41 tape blocks
x ./doc/msmacros/refcard, 19349 bytes, 38 tape blocks
x ./doc/password, 20688 bytes, 41 tape blocks
x ./doc/porttour/porttour1, 53171 bytes, 104 tape blocks
x ./doc/porttour/porttour2, 46862 bytes, 92 tape blocks
x ./doc/ratfor/m.mac, 789 bytes, 2 tape blocks
x ./doc/ratfor/m0, 2278 bytes, 5 tape blocks
x ./doc/ratfor/m1, 2381 bytes, 5 tape blocks
x ./doc/ratfor/m2, 27612 bytes, 54 tape blocks
x ./doc/ratfor/m3, 4210 bytes, 9 tape blocks
x ./doc/ratfor/m4, 3836 bytes, 8 tape blocks
x ./doc/ratfor/m5, 1700 bytes, 4 tape blocks
x ./doc/ratfor/m9, 1040 bytes, 3 tape blocks
x ./doc/ratfor/m99, 2108 bytes, 5 tape blocks
x ./doc/refer/refer, 42611 bytes, 84 tape blocks
x ./doc/refer/pubuse, 18981 bytes, 38 tape blocks
x ./doc/run, 2116 bytes, 5 tape blocks
x ./doc/scope, 17 bytes, 1 tape blocks
x ./doc/security, 11975 bytes, 24 tape blocks
x ./doc/sed, 27119 bytes, 53 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_30of33_DOCUMENTS_4of7_BL-X335A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./doc/ig/01.frontp, 3825 bytes, 8 tape blocks
x ./doc/ig/02.preface, 1651 bytes, 4 tape blocks
x ./doc/ig/03.intro, 7986 bytes, 16 tape blocks
x ./doc/ig/1.mag, 38881 bytes, 76 tape blocks
x ./doc/ig/2.rl2, 24950 bytes, 49 tape blocks
x ./doc/ig/3.rx50, 31783 bytes, 63 tape blocks
x ./doc/ig/4.appA, 2052 bytes, 5 tape blocks
x ./doc/ig/4.appB, 8096 bytes, 16 tape blocks
x ./doc/ig/4.appD, 1776 bytes, 4 tape blocks
x ./doc/ig/4.appC, 14045 bytes, 28 tape blocks
x ./doc/ig/4.appE, 3036 bytes, 6 tape blocks
x ./doc/ig/macs, 329 bytes, 1 tape blocks
x ./doc/ig/4.appF, 4952 bytes, 10 tape blocks
x ./doc/ig/1.toc, 691 bytes, 2 tape blocks
x ./doc/ig/2.toc, 604 bytes, 2 tape blocks
x ./doc/ig/mkig, 662 bytes, 2 tape blocks
x ./doc/ig/3.toc, 681 bytes, 2 tape blocks
x ./doc/ig/4.appG, 4118 bytes, 9 tape blocks
x ./doc/ig/toc, 19116 bytes, 38 tape blocks
x ./doc/summary/hel.mac, 151 bytes, 1 tape blocks
x ./doc/summary/hel0, 2739 bytes, 6 tape blocks
x ./doc/summary/hel1, 17603 bytes, 35 tape blocks
x ./doc/summary/hel2, 4528 bytes, 9 tape blocks
x ./doc/summary/hel3, 6875 bytes, 14 tape blocks
x ./doc/summary/hel4, 2484 bytes, 5 tape blocks
x ./doc/summary/hel5, 608 bytes, 2 tape blocks
x ./doc/summary/hel6, 1358 bytes, 3 tape blocks
x ./doc/tbl, 43059 bytes, 85 tape blocks
x ./doc/troff/tprint, 170 bytes, 1 tape blocks
x ./doc/troff/m.mac, 2423 bytes, 5 tape blocks
x ./doc/troff/m0, 6778 bytes, 14 tape blocks
x ./doc/troff/m0a, 14404 bytes, 29 tape blocks
x ./doc/troff/m1, 24680 bytes, 49 tape blocks
x ./doc/troff/m2, 14126 bytes, 28 tape blocks
x ./doc/troff/m3, 16761 bytes, 33 tape blocks
x ./doc/troff/m4, 13259 bytes, 26 tape blocks
x ./doc/troff/m5, 12491 bytes, 25 tape blocks
x ./doc/troff/table1, 1443 bytes, 3 tape blocks
x ./doc/troff/table2, 4223 bytes, 9 tape blocks
x ./doc/troff/add, 2582 bytes, 6 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_31of33_DOCUMENTS_5of7_BL-X336A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./doc/shell/t.mac, 542 bytes, 2 tape blocks
x ./doc/shell/t1, 10236 bytes, 20 tape blocks
x ./doc/shell/t2, 18410 bytes, 36 tape blocks
x ./doc/shell/t3, 20145 bytes, 40 tape blocks
x ./doc/shell/t4, 2501 bytes, 5 tape blocks
x ./doc/trofftut/tt.mac, 1275 bytes, 3 tape blocks
x ./doc/trofftut/tt00, 1476 bytes, 3 tape blocks
x ./doc/trofftut/tt01, 3960 bytes, 8 tape blocks
x ./doc/trofftut/tt02, 4500 bytes, 9 tape blocks
x ./doc/trofftut/tt03, 3915 bytes, 8 tape blocks
x ./doc/trofftut/tt04, 3029 bytes, 6 tape blocks
x ./doc/trofftut/tt05, 1804 bytes, 4 tape blocks
x ./doc/trofftut/tt06, 6765 bytes, 14 tape blocks
x ./doc/trofftut/tt07, 1696 bytes, 4 tape blocks
x ./doc/trofftut/tt08, 2641 bytes, 6 tape blocks
x ./doc/trofftut/tt09, 5701 bytes, 12 tape blocks
x ./doc/trofftut/tt10, 4555 bytes, 9 tape blocks
x ./doc/trofftut/tt11, 3892 bytes, 8 tape blocks
x ./doc/trofftut/tt12, 2353 bytes, 5 tape blocks
x ./doc/trofftut/tt13, 1571 bytes, 4 tape blocks
x ./doc/trofftut/tt14, 2984 bytes, 6 tape blocks
x ./doc/trofftut/ttack, 986 bytes, 2 tape blocks
x ./doc/trofftut/ttcharset, 2118 bytes, 5 tape blocks
x ./doc/trofftut/ttindex, 3268 bytes, 7 tape blocks
x ./doc/uprog/p0, 876 bytes, 2 tape blocks
x ./doc/uprog/p1, 932 bytes, 2 tape blocks
x ./doc/uprog/p2, 4599 bytes, 9 tape blocks
x ./doc/uprog/p3, 8679 bytes, 17 tape blocks
x ./doc/uprog/p4, 12281 bytes, 24 tape blocks
x ./doc/uprog/p5, 12542 bytes, 25 tape blocks
x ./doc/uprog/p6, 8428 bytes, 17 tape blocks
x ./doc/uprog/p8, 336 bytes, 1 tape blocks
x ./doc/uprog/p9, 12886 bytes, 26 tape blocks
x ./doc/uprog/p.mac, 1220 bytes, 3 tape blocks
x ./doc/uprog/cwscript, 560 bytes, 2 tape blocks
x ./doc/uucp/network, 21714 bytes, 43 tape blocks
x ./doc/uucp/implement, 37037 bytes, 73 tape blocks
x ./doc/uucp/admin/p10.m, 5363 bytes, 11 tape blocks
x ./doc/uucp/admin/p20.m, 10443 bytes, 21 tape blocks
x ./doc/uucp/admin/p30.m, 15166 bytes, 30 tape blocks
x ./doc/uucp/admin/p40.m, 23672 bytes, 47 tape blocks
x ./doc/yacc/ss.., 1829 bytes, 4 tape blocks
x ./doc/yacc/ss0, 6439 bytes, 13 tape blocks
x ./doc/yacc/ss1, 4255 bytes, 9 tape blocks
x ./doc/yacc/ss2, 4213 bytes, 9 tape blocks
x ./doc/yacc/ss3, 3358 bytes, 7 tape blocks
x ./doc/yacc/ss4, 9162 bytes, 18 tape blocks
x ./doc/yacc/ss5, 8647 bytes, 17 tape blocks
x ./doc/yacc/ss6, 5391 bytes, 11 tape blocks
x ./doc/yacc/ss7, 4933 bytes, 10 tape blocks
x ./doc/yacc/ss8, 2576 bytes, 6 tape blocks
x ./doc/yacc/ss9, 4858 bytes, 10 tape blocks
x ./doc/yacc/ssA, 5226 bytes, 11 tape blocks
x ./doc/yacc/ssB, 802 bytes, 2 tape blocks
x ./doc/yacc/ssa, 2769 bytes, 6 tape blocks
x ./doc/yacc/ssb, 2568 bytes, 6 tape blocks
x ./doc/yacc/ssc, 8466 bytes, 17 tape blocks
x ./doc/yacc/ssd, 1317 bytes, 3 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_32of33_DOCUMENTS_6of7_BL-X795A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./doc/smg/smg.2, 24331 bytes, 48 tape blocks
x ./doc/smg/appD, 6913 bytes, 14 tape blocks
x ./doc/smg/smg.9, 26254 bytes, 52 tape blocks
x ./doc/smg/macs, 333 bytes, 1 tape blocks
x ./doc/smg/appB, 20848 bytes, 41 tape blocks
x ./doc/smg/smg.1, 18499 bytes, 37 tape blocks
x ./doc/smg/mksmg, 51 bytes, 1 tape blocks
x ./doc/smg/smg.6, 11049 bytes, 22 tape blocks
x ./doc/smg/sg, 20220 bytes, 40 tape blocks
x ./doc/smg/appC, 1629 bytes, 4 tape blocks
x ./doc/smg/smg.intro, 16601 bytes, 33 tape blocks
x ./doc/smg/smg.4, 28654 bytes, 56 tape blocks
x ./doc/smg/appE, 1956 bytes, 4 tape blocks
x ./doc/smg/smg.10, 37257 bytes, 73 tape blocks
x ./doc/smg/smg.5, 4304 bytes, 9 tape blocks
x ./doc/smg/smg.8, 18637 bytes, 37 tape blocks
x ./doc/smg/smg.3, 11032 bytes, 22 tape blocks
x ./doc/smg/smg.7, 44627 bytes, 88 tape blocks
x ./doc/smg/appA, 24115 bytes, 48 tape blocks
x ./doc/smg/appF, 4484 bytes, 9 tape blocks
x ./doc/std/frontp, 2620 bytes, 6 tape blocks
x ./doc/std/std, 43241 bytes, 85 tape blocks
x ./doc/std/rn, 4549 bytes, 9 tape blocks
x ./doc/std/toc.rn, 1272 bytes, 3 tape blocks
# sync
# 
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> att -r rq3 V7M-11_V1.0_6_USR_RX50_33of33_DOCUMENTS_7of7_BL-Y665A-BC.IMG 
RQ3: Unit is read only
sim> c

# tar xvf /dev/rx3
Tar: blocksize = 20
x ./doc/ex/edit.tut, 56411 bytes, 111 tape blocks
x ./doc/ex/edit.vindex, 1345 bytes, 3 tape blocks
x ./doc/ex/ex.rm, 57630 bytes, 113 tape blocks
x ./doc/ex/ex.summary, 13971 bytes, 28 tape blocks
x ./doc/ex/ex1.1-2.0, 5697 bytes, 12 tape blocks
x ./doc/ex/ex2.0-3.1, 10539 bytes, 21 tape blocks
x ./doc/ex/ex3.1-3.5, 7289 bytes, 15 tape blocks
x ./doc/ex/makefile, 884 bytes, 2 tape blocks
x ./doc/ex/vi.apwh.ms, 38894 bytes, 76 tape blocks
x ./doc/ex/vi.chars, 25024 bytes, 49 tape blocks
x ./doc/ex/vi.in, 78389 bytes, 154 tape blocks
x ./doc/ex/vi.summary, 9772 bytes, 20 tape blocks
# sync
# 
```

Now everything is installed.

```
# df
/dev/rd0 12240
/dev/rd1 5752
# 
```

There's a missing hard link for the vi editor

```
# cd /usr/bin
# ln ex vi
#
```


Logout from root (press CTRL-D) and shutdown the system to restart
everything.

```
# ^D
login: operator

Welcome to V7M-11 V1.0


V7M-11 Operator Services

Line editing: delete - erase one character, ^U - kill entire line

For help, type h followed by a return

opr> s

V7M-11 Shutdown
The following users are logged into the System

operator console Oct  3 02:59

How many minutes until shutdown [1-99] ? 0

Warning Phase

        FINAL WARNING SENT

Kill Process Phase

        Killing User Processes
        Killing System Processes
        Disabling Error Logging

Dismounting mounted filesystems

Dismounting /dev/rd1

System Timesharing Stopped

opr> halt
Ready to halt system ? y
Halting System in 1 second

opr> 
HALT instruction, PC: 002202 (BR 2200)
sim> exit
Goodbye
```

Next, restart SIMH pdp11 again and we'll do a sysgen...

```
simh tony$ pdp11 v7m.ini 

PDP-11 simulator V4.0-0 Current        git commit id: eadf1269
Disabling RK
Disabling HK
Disabling TM
CPU     11/23+, FPP, NOCIS, autoconfiguration enabled, idle enabled
        512KB
CLK     60Hz, address=17777546-17777547, vector=100, BR6
LPT     address=17777514-17777517, vector=200, BR4
[snip]
To boot the BOOT floppy
boot rq2

#boot
Boot
: rx(2,0)restor         to run stand-alone restor
: rx(2,0)mkfs           to make a new filesystem

To boot the RD51 (/ on rq0, /usr on rq1)
boot rq0

#boot
Boot
: rd(0,0)unix           to boot the Unix V7M kernel

sim> 
```

We're going to boot from the freshly created RD51 drive this time,
start-up timesharing and use the sys account to run sysgen -

```
sim> b rq0
#boot

Boot
: rd(0,0)unix


V7M-11 V1.0

realmem = 524288
usermem = 448256
erase = delete, kill = ^U, intr = ^C
# ^D

Restricted rights:

        Use, duplication, or disclosure is subject
        to restrictions stated in your contract with
        Digital Equipment Corporation.

*UNIX is a Trademark of Bell Laboratories.

Mounted /dev/rd1 on /usr 

Sun Oct  3 03:00:03 EDT 1993

ERROR LOG has - 1 of 40 blocks used

login: sys

Welcome to V7M-11 V1.0

erase = delete, kill = ^U, intr = ^C
$ cd conf
$ sysgen


V7M-11 System Generation Program
Type h for help


> h

The `>' prompt indicates that SYSGEN is ready to accept a command.
Except for `control d' and `control c', commands are executed by typing
the command letter followed by a return. The commands will ask for
additional information, such as the configuration file name, if required.
For more help type `h' followed by the command, `h c' for help create.

Command         Description

control d       Exit from the SYSGEN program.
control c       Cancel current command and return to the `>' prompt.

! command       Execute a unix command.
c               Create a configuration file.
r               Remove a configuration file.
l               List the names of all existing configuration files.
p               Prints the contents of a configuration file.
m               Make the unix operating system.
i               Install the new unix operating system.
d               Devices - Print a list of valid controller and device names.
s               Sources - Recompile and archive unix source modules.

The SYSGEN sequence is; `c' to create the configuration file, `m' to make the
unix operating system, and `i' to install the new unix operating system.
> c

To backup to the previous question, type `control d' !
Answer any question with a `?' followed by a return for help !

Configuration name <unix> ? nunix

Processor type:

< 23 24 34 40 44 45 55 60 70 > ? 23

Disk controller type:

< rh11 rh70 rp11 rk611 rk711 rl11 rx211 rk11 uda50 rqdx1 > ? rqdx1

Drive 0 type < rx50 rd51 > ? rd51

Drive 1 type < rx50 rd51 > ? rd51

Drive 2 type < rx50 rd51 > ? rx50

Drive 3 type < rx50 rd51 > ? rx50

Drive 4 type < rx50 rd51 > ? 

CSR address <172150> ? 

Vector address <154> ? 

Is the system disk on this controller <yes> ? yes

System disk unit number <0> ? 0

Disk controller type:

< rh11 rh70 rp11 rk611 rk711 rl11 rx211 rk11 uda50 rqdx1 > ? rl11

Drive 0 type < rl01 rl02 > ? rl02

Drive 1 type < rl01 rl02 > ? rl02

Drive 2 type < rl01 rl02 > ? rl02

Drive 3 type < rl01 rl02 > ? rl02

CSR address <174400> ? 

Vector address <160> ? 

Disk controller type:

< rh11 rh70 rp11 rk611 rk711 rl11 rx211 rk11 uda50 rqdx1 > ? 

Use standard placement of root, swap, and error log <yes> ? yes

Magtape controller:

< tm02 tm03 tm11 ts11 tsv05 tc11 > ? 

Crash dumps to RD51 unit 0 (swap area)

LP11 line printer present <no> ? yes

CSR address <177514> ? 

vector address <200> ? 

Communications devices:

< dz dzv dh dhdm du dn kl dl > ? dzv

Number of units <1> ? 1

CSR address <160100> ? 

Vector address <300> ? 310

Communications devices:

< dz dzv dh dhdm du dn kl dl > ? dl

Number of units <1> ? 1

CSR address <175610> ? 176500

Vector address <300> ? 300

Communications devices:

< dz dzv dh dhdm du dn kl dl > ? 

Include C/A/T phototypesetter driver <no> ? no

User devices:

< u1 u2 u3 u4 > ? 

Include packet driver <no> ? no

Include multiplexed files support <no> ? no

Use standard system parameters <yes> ? yes

Line frequency in hertz <60> ? 

Timezone (hours ahead of GMT) <5=EST 6=CST 7=MST 8=PST> ? 0

Does your area use daylight savings time <yes> ? no


V7M-11 System Generation Program
Type h for help


> m

Configuration name <unix> ? nunix

****** CREATING UNIX CONFIGURATION AND VECTOR TABLES ******

Device  Address Vector  units

console 177560   60
kw11-l  177456  100
kw11-p  172540  104
ra      172150  154     4       (core dump device)
rl      174400  160     4
lp      177514  200     1
dl      176500  300     1
dzv     160100  310     1

Filsys  Device  maj/min start   length

root    ra        2/7
pipe    ra        2/7
swap    ra        2/7   18920   2648
errlog  ra       22/7   18880   40
WARNING, root & error log on ra 7 watchout for overlap !
WARNING, root & swap on ra 7 watchout for overlap !

****** MAKING UNIX FOR NON SEPARATE I & D SPACE PROCESSORS ******

as - -o l.o l.s
ovas -o dump_ov.o mch0.s dump.s
cc -c -O -DK_OV -V c.c
mv c.o c_ov.o

The output file will be named unix_ov !!!!!

ovload

The unix_ov sizes must be within the following limits:

root text segment > 8192 but <= 16384
overlay text segments <= 8192, 7 overlays maximum
bss + data segments <= 24576 total

root+(overlay 1, overlay 2,...overlay n)+data+bss = root+data = (total)

size unix_ov
15808+(7936,7808,7936,8000)+3358+17680 = 36846b = 0107756b (47488 total text)

rm l.o c_ov.o dump_ov.o

New unix is now named `nunix.os' !

****** CHECKING SIZE OF NEW UNIX OPERATING SYSTEM ******

`nunix.os' within limits, SYSGEN successful !


> i

Configuration name <unix> ? nunix

Use the following procedure to install the new unix:

1.      Type `su' to become super-user.
        Enter the root password if requested.

2.      Type `mv nunix.os /nunix' to move the new unix to the root.

3.      Type `/etc/shutdown' to shutdown the system.

4.      Reboot the system using `nunix' in place of `unix'.
        See reboot instructions for procedure.

5.      Type `mv unix ounix' to save old unix !

6.      Type `mv nunix unix' to rename nunix to unix.

7.      Type `sync' to update the file system.

8.      Type `control d' to bring unix up multi-user.

> > ^D

$ su
# cp nunix.os /nunix
# /etc/shutdown

How many minutes until shutdown [1-99] ? 0
Broadcast Message ...

System going down. Bye


Error Log Shutdown Phase


Kill Process Phase


****** WAIT FOR # THEN HALT THE PROCESSOR ******
# 
```

Press CTRL-E, exit SIMH pdp11 and restart it (we can't reboot from the sim>
prompt because of some issue/state the devices are in).

```
Simulation stopped, PC: 004636 (MOV (SP)+,177776)
sim> exit
Goodbye
```


This time we boot from the new nunix kernel


```
simh tony$ pdp11 v7m.ini
[snip]
sim>  b rq0
#boot

Boot
: rd(0,0)nunix


V7M-11 V1.0

realmem = 524288
usermem = 445696
erase = delete, kill = ^U, intr = ^C
# 
```

Now create the devices for the RL02 drives and terminals

```
# cd /dev    
# csf rl0 rl1 rl2 rl3
# msf dzv11 0 tty00
# msf dl11 1 tty04
# ls -la
total 24
drwxr-xr-x 2 root       4736 Oct  3 07:27 .
drwxr-xr-x15 root        912 Oct  3 07:08 ..
crw--w--w- 1 root      0,  0 Oct  3 07:27 console
crw------- 1 root     22,  7 Oct  3 07:26 errlog
crw-r--r-- 1 root     11,  1 Jan  9 13:53 kmem
c-w------- 1 daemon    2,  0 Jul 30 19:17 lp
-rw-rw-r-- 1 bin        3900 Apr 28 22:00 makefile
crw-r--r-- 1 root     11,  0 Jul  7 21:42 mem
-rwxr--r-- 1 root       2020 Jul 15 14:41 msf
crw-rw-rw- 1 root     11,  2 Aug  2 14:24 null
brw------- 2 root      2,  7 Oct  3 05:40 rd0
brw------- 1 root      2, 15 Oct  3 05:41 rd1
brw------- 1 root      3,  0 Oct  3 07:12 rl0
brw------- 1 root      3,  1 Oct  3 07:12 rl1
brw------- 1 root      3,  2 Oct  3 07:12 rl2
brw------- 1 root      3,  3 Oct  3 07:12 rl3
crw------- 1 root     22,  7 Oct  3 05:40 rrd0
crw------- 1 root     22, 15 Oct  3 05:41 rrd1
crw------- 1 root     23,  0 Oct  3 07:12 rrl0
crw------- 1 root     23,  1 Oct  3 07:12 rrl1
crw------- 1 root     23,  2 Oct  3 07:12 rrl2
crw------- 1 root     23,  3 Oct  3 07:12 rrl3
crw------- 1 root     22, 23 Oct  3 05:42 rrx2
crw------- 1 root     22, 31 Oct  3 05:42 rrx3
brw------- 1 root      2, 23 Oct  3 05:42 rx2
brw------- 1 root      2, 31 Oct  3 05:42 rx3
brw------- 2 root      2,  7 Oct  3 05:40 swap
crw-rw-rw- 1 root     10,  0 Sep 22 20:00 tty
crw-rw-rw- 1 root      8,  0 Oct  3 07:27 tty00
crw-rw-rw- 1 root      8,  1 Oct  3 07:27 tty01
crw-rw-rw- 1 root      8,  2 Oct  3 07:27 tty02
crw-rw-rw- 1 root      8,  3 Oct  3 07:27 tty03
crw-rw-rw- 1 root      0,  1 Oct  3 07:27 tty04
# 
```

Next enable the extra terminals

```
# cd /etc
# more ttys
14console
00tty00
00tty01
00tty02
00tty03
00tty04
# mount /dev/rd1 /usr
# sed 's/^00/22/g' <ttys >ttys.new
# more ttys.new
14console
22tty00
22tty01
22tty02
22tty03
22tty04
# mv ttys ttys.orig
# mv ttys.new ttys
# more ttytype
vt100   console
vt100   tty00
vt100   tty01
vt100   tty02
vt100   tty03
# cat <<EOF >>ttytype
> vt100 tty04
> EOF
# 
# /etc/shutdown

How many minutes until shutdown [1-99] ? 0

Error Log Shutdown Phase


Kill Process Phase


****** WAIT FOR # THEN HALT THE PROCESSOR ******
# 
Simulation stopped, PC: 004662 (MOV (SP)+,177776)
sim> exit
Goodbye
```

Now reboot using the nunix kernel and verify the tty devices
are present and active

```
simh tony$ pdp11 v7m.ini
[snip]
sim> b rq0
#rd(0,0)nunix
#boot

Boot
: rd(0,0)nunix


V7M-11 V1.0

realmem = 524288
usermem = 445696
erase = delete, kill = ^U, intr = ^C
# tss

  1 KL  lines
  1 DL  lines
  0 DH  lines
  4 DZ  lines
  6 TTY structures

Device  Unit    Line    TTY     Special
Name    Number  Number  Struct  File

KL       0               0      /dev/console
DL       0               1      /dev/tty04
DZ       0       0       2      /dev/tty00
DZ       0       1       3      /dev/tty01
DZ       0       2       4      /dev/tty02
DZ       0       3       5      /dev/tty03
#
```

Save the original kernel

```
# mv unix ounix
# cp nunix unix
# date 9310030354
Sun Oct  3 03:54:00 GMT 1993
# ^D

Restricted rights:

        Use, duplication, or disclosure is subject
        to restrictions stated in your contract with
        Digital Equipment Corporation.

*UNIX is a Trademark of Bell Laboratories.

Mounted /dev/rd1 on /usr 

Sun Oct  3 03:54:05 GMT 1993

ERROR LOG has - 1 of 40 blocks used

login: root

Welcome to V7M-11 V1.0

erase = delete, kill = ^U, intr = ^C
# who
root     console Oct  3 03:54
# 
```


Now create a generic user account

```
# mkdir /home
# cat <<EOF >>/etc/passwd
> user::100:100::/home/user:
> EOF
# cat <<EOF >>/etc/group
> staff::100:
> EOF
# mkdir /home/user
# chown user /home/user
# chgrp staff /home/user
# cp /.profile /home/user/.profile
# chown user /home/user/.profile
# chgrp staff /home/user/.profile
```


Verify you can login to this account by telneting to one of the
serial devices (TCP port 2040 or 2041 - as per the SIMH configuration).

(on another session)

```
tony$ telnet 127.0.0.1 2041
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.


Connected to the PDP-11 simulator DLI device


login: user

Welcome to V7M-11 V1.0

erase = delete, kill = ^U, intr = ^C
$ who
root     console Oct  3 03:54
user     tty04   Oct  3 03:55
$ 
```

Remember to check the disk volumes and shutdown correctly using the
/etc/shutdown command (as root) or login as operator, (s)hutdown, then
halt.

To check the disk volumes in single user mode use

```
sim> b rq0
#boot

Boot
: rd(0,0)unix


V7M-11 V1.0

realmem = 524288
usermem = 445696
erase = delete, kill = ^U, intr = ^C
# fsck /dev/rrd0

/dev/rrd0
File System:  Volume: 

** Phase 1 - Check Blocks and Sizes
** Phase 2 - Check Pathnames
** Phase 3 - Check Connectivity
** Phase 4 - Check Reference Counts
** Phase 5 - Check Free List 
657 files 7887 blocks 10198 free
# fsck /dev/rrd1

/dev/rrd1
File System:  Volume: 

** Phase 1 - Check Blocks and Sizes
** Phase 2 - Check Pathnames
** Phase 3 - Check Connectivity
** Phase 4 - Check Reference Counts
** Phase 5 - Check Free List 
1534 files 18332 blocks 2372 free
# 
```


Have fun!

Tony Nicholson
03-Oct-2021
