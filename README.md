# Android/riscv64

## What is this?

This github project is solely for issue tracking/discussion/documentation
purposes.

All Android/riscv64 work is being done directly in [AOSP](https://source.android.com/).
Patches should be sent to AOSP using the usual
[AOSP contribution process](https://source.android.com/docs/setup/contribute#contribute-to-the-code)
and *not* as pull requests here.

You might also want to subscribe to the
[sig-android mailing list](https://lists.riscv.org/g/sig-android).
(You need to send mail to
[sig-android+subscribe@lists.riscv.org](mailto:sig-android+subscribe@lists.riscv.org)
and then click "Join" on the mailing list web site.)

## Status

We're currently (2023Q3) still working on
[cuttlefish virtual devices](https://source.android.com/docs/setup/create/cuttlefish)
and
[ART](https://source.android.com/docs/core/runtime),
although the
[shell and command-line tools](https://android.googlesource.com/platform/system/core/+/main/shell_and_utilities/README.md)
(and all the libraries they rely on) have been working great for a while.
ART works, but is currently interpreter-only, so quite slow.
cuttlefish works, but you'll need a very recent qemu (see the cuttlefish setup section below for more details).

You can see the current status of the
riscv64 build in the `aosp_riscv64` column of
[ci.android.com](https://ci.android.com/builds/branches/aosp-master/grid?). (Note that this seems to be out of date as part of the 2023-07-06 transition from "master" to "main" --- "master" seems to be no longer building, but the "main" build doesn't exist yet.)

## Can I try it?

Download the source using the usual
[AOSP download process](https://source.android.com/docs/setup/download/downloading)
and then:
```
$ cd aosp
$ source build/envsetup.sh
$ lunch aosp_cf_riscv64_phone-userdebug

============================================
PLATFORM_VERSION_CODENAME=UpsideDownCake
PLATFORM_VERSION=UpsideDownCake
TARGET_PRODUCT=aosp_cf_riscv64_phone
TARGET_BUILD_VARIANT=userdebug
TARGET_ARCH=riscv64
TARGET_ARCH_VARIANT=riscv64
TARGET_CPU_VARIANT=generic
HOST_OS=linux
HOST_OS_EXTRA=Linux-5.19.11-1rodete1-amd64-x86_64-Debian-GNU/Linux-rodete
HOST_CROSS_OS=windows
BUILD_ID=AOSP.MASTER
OUT_DIR=out
============================================
$ make -j
```
If you want to check whether a particular directory builds, `cd` into
that directory and use `mm -j`.

(For more tips on building in general, see
[Building Android](https://source.android.com/docs/setup/build/building).)

### Cuttlefish (emulator) setup

To launch cuttlefish, follow the general
[AOSP cuttlefish setup](https://source.android.com/docs/setup/create/cuttlefish-use)
instructions.

(Note that in addition to the general setup mentioned above, if your host Linux
distro doesn't already have it, you will have to `apt install qemu-system-riscv64`.
If your host Linux distro's qemu version is too old -- we recommend 7.2.0 --
you may need to build your own. In that case, you'll need the `-qemu_binary_dir=`
option when calling `launch_cvd` to point it at the correct copy of qemu.)

### Getting to a shell (faster, but no graphics)

After building, run this following command from the same shell:
```
$ launch_cvd -cpus=4 --memory_mb=8192
```
After about 10s you should be able to use `adb shell` to connect to your riscv64 cuttlefish!

### Getting to the home screen (slower, but with graphics)

After building, run this following command from the same shell:
```
$ launch_cvd -cpus=8 --memory_mb=8192 --gpu_mode=drm_virgl
```
You can then use `vncviewer localhost:6444` to connect to your riscv64 cuttlefish!

(Note that even on a fast Xeon workstation it takes several minutes to get to
the boot animation and ten minutes to get to the home screen!)

## How do I contribute?

Consult the regular AOSP
[Contributing](https://source.android.com/docs/setup/contribute#contribute-to-the-code)
documentation for more information on how to send us your patches.

Note that changes to projects under `external/` (other than to
`Android.bp` files) typically need to go to the upstream open source
project in question first, and will then be merged into AOSP from
there. In many cases, the `METADATA` file will point you to the
upstream source, but feel free to ask here if not!

## Review our Community Guidelines

This project follows [Google's Open Source Community
Guidelines](https://opensource.google/conduct/).
