# Grub for Chromium OS on Intel N4200
There is a bug in Grub 2 that prevents it from booting on N4200 CPUs.
This prevents Chromium OS from booting on it. The fix has been
committed upstream but has not been released on a stable version yet.
Ubuntu has the fix included in their Grub compile, so Ubuntu does
not have this issue.

In this repository, I am compiling Grub 2.02 with the fix and other
Chromium OS-specific patches so that Chromium OS can be used on
N4200 CPUs.

## Download
See [Release Page](https://github.com/pawitp/grub2-chromiumos-n4200/releases)

## The Fix
The file `0003-tsc-calibration-pmtimer.patch` contains the fix.

The patch was retrieved from https://git.savannah.gnu.org/cgit/grub.git/commit/?id=446794de8da4329ea532cbee4ca877bcafd0e534.

## Other Chromium OS patches
The other Chromium OS specific patches are:
 - `0001-Forward-port-ChromeOS-specific-GRUB-environment-vari.patch`
 - `0002-Forward-port-gptpriority-command-to-GRUB-2.00.patch`

They were retrieved from https://chromium.googlesource.com/chromiumos/overlays/chromiumos-overlay/+/4b5a6928cf7f351725139098c5faffd8fe64c3fe/sys-boot/grub/files/.

## How to compile
The binary found on the release page was compiled using Ubuntu 18.04
with the following commands:

```
sudo apt-get install build-essential flex bison autoconf
wget https://ftp.gnu.org/gnu/grub/grub-2.02.tar.gz
tar zxf grub-2.02.tar.gz
cd grub-2.02
for p in ../*.patch; do patch -p1 < $p; done
./autogen.sh
./configure --prefix=/opt/grub --with-platform=efi --target=x86_64 --disable-werror
sudo make install
/opt/grub/bin/grub-mkimage -O x86_64-efi -o grubx64.efi -p /efi/boot part_gpt gptpriority test fat ext2 hfs hfsplus normal boot chain efi_gop configfile linux
```
