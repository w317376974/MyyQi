If you appreciate this project, support me on Patreon or Pledgie !

[![Patreon !](https://raw.githubusercontent.com/Miouyouyou/RockMyy/master/.img/button-patreon.png)](https://www.patreon.com/Miouyouyou)

[![Pledgie !](https://pledgie.com/campaigns/32702.png)](https://pledgie.com/campaigns/32702)

4.13 and onwards
----------------

See the [RockMyy](https://github.com/Miouyouyou/RockMyy) branch !

About this branch
-----------------

The compilation script of this branch uses the 
**[linux-stable](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git)**
kernel branch. The **linux-stable** branch backports a lot of fixes and 
security patches for an eternity and a half.

Now, basically, the script of this stable branch will try to retrieve
the latest **linux-stable** branch, apply the patches, cross-compile and
install the cross-compiled kernel like in the
**[master](https://github.com/Miouyouyou/MyyQi)** branch.

The thing is :
I don't care about the **linux-stable** kernel branch, since I don't use
it.

So if the patches break in the future, you'll either have to maintain
them yourselves or show me the money if you want me to maintain them.

Or try the [latest RC branch](https://github.com/Miouyouyou/RockMyy) !

About
-----

This is a working patched 4.12.3 kernel with [Mali r17p0 Kernel drivers](http://malideveloper.arm.com/resources/drivers/open-source-mali-midgard-gpu-kernel-drivers/), using the [torvalds branch](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/) as a basis. This also integrate patches from Willy Tarreau, making possible to get better performances from the board. [More informations in this thread](https://forum.mqmaker.com/t/miqi-based-build-farm-finally-up-and-running/605).

Currently this kernel has been tested sucessfully with the [Firefly's Mali User-space r12p0 drivers for fbdev and wayland](http://malideveloper.arm.com/resources/drivers/arm-mali-midgard-gpu-user-space-drivers/#mali-user-space-driver-r12p0-mali-t760-gnulinux), using the [OpenGL ES 3.1 and 3.2 samples of the Mali OpenGL ES SDK](http://malideveloper.arm.com/resources/sdks/opengl-es-sdk-for-linux/). Pure DRM OpenGL was also tested successfully with these drivers, using [this patched gl2mark](https://github.com/Miouyouyou/glmark2).

[A few benchmarks using previous versions of kernels patched the same way are available on ARMbian forums](https://forum.armbian.com/index.php/topic/2045-armbian-on-miqi-sbc-hardware/).

X11 drivers were not tested successfully however.

The kernel was compiled using the following procedure :
```bash
export ARCH=arm
export CROSS_COMPILE=armv7a-hardfloat-linux-gnueabi-

if [ -z ${MAKE_CONFIG+x} ]; then
  export MAKE_CONFIG=oldconfig
fi

export DTB_FILES="
rk3288-evb-act8846.dtb
rk3288-evb-rk808.dtb
rk3288-fennec.dtb
rk3288-firefly-beta.dtb
rk3288-firefly-reload.dtb
rk3288-firefly.dtb
rk3288-tinker.dtb
rk3288-miniarm.dtb
rk3288-miqi.dtb
rk3288-popmetal.dtb
rk3288-r89.dtb
rk3288-rock2-square.dtb
rk3288-veyron-brain.dtb
rk3288-veyron-jaq.dtb
rk3288-veyron-jerry.dtb
rk3288-veyron-mickey.dtb
rk3288-veyron-minnie.dtb
rk3288-veyron-pinky.dtb
rk3288-veyron-speedy.dtb
"

export KERNEL_GIT_URL=git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git
export KERNEL_SERIES=v4.12
export KERNEL_BRANCH=linux-4.12.y
export KERNEL_VERSION=4.12.3
export MYY_VERSION=-The-Twelve-MyyQi+
export MALI_VERSION=r17p0-01rel0
export MALI_BASE_URL=https://developer.arm.com/-/media/Files/downloads/mali-drivers/kernel/mali-midgard-gpu

export GITHUB_REPO=Miouyouyou/MyyQi
export GIT_BRANCH=stable
export GIT_TAG=v4.12.3

export BASE_FILES_URL=https://raw.githubusercontent.com
export PATCHES_FOLDER_URL=$BASE_FILES_URL/$GITHUB_REPO/$GIT_TAG/patches
export KERNEL_PATCHES_FOLDER_URL=$PATCHES_FOLDER_URL/kernel/$KERNEL_SERIES
export MALI_PATCHES_FOLDER=$PATCHES_FOLDER_URL/Mali/$MALI_VERSION

export KERNEL_PATCHES="
0001-Readaptation-of-Rockchip-DRM-patches-provided-by-ARM.patch
0002-Integrate-the-Mali-GPU-address-to-the-rk3288-and-rk3.patch
0003-Post-Mali-Kernel-device-drivers-modifications.patch
0004-Post-Mali-UMP-integration.patch
0005-ARM-dts-rockchip-fix-the-regulator-s-voltage-range-o.patch
0007-Adaptation-ARM-dts-rockchip-add-the-MiQi-board-s-fan.patch
0008-ARM-dts-rockchip-add-support-for-1800-MHz-operation-.patch
0009-clk-rockchip-add-all-known-operating-points-to-the-a.patch
0010-Readapt-ARM-dts-rockchip-miqi-add-turbo-mode-operati.patch
0011-arm-dts-Adding-and-enabling-VPU-services-addresses-f.patch
0012-Export-rockchip_pmu_set_idle_request-for-out-of-tree.patch
0013-clk-rockchip-rk3288-prefer-vdpu-for-vcodec-clock-sou.patch
0014-ARMbian-RK3288-DTSI-changes.patch
0015-Enabling-Tinkerboard-s-Wifi-Third-tentative.patch
0016-Added-support-for-Tinkerboard-s-SPI-interface.patch
0017-Testing-DTS-changes-in-order-to-resolve-bug-8.patch
0018-Added-debug-messages-to-check-the-Bluetooth-Coexiste.patch
0019-The-ASUS-Tinkerboard-reboot-patch.patch
0020-Common-RK3288-DTSI-additions-by-ARMbian.patch
0100-First-Mali-integration-test-for-ASUS-Tinkerboards.patch
0200-The-Tinkerboard-DTS-file-maintained-by-TonyMac32-and.patch
0300-Adding-Mali-Midgard-and-VCodec-support-to-Firefly-RK.patch
"

export MALI_PATCHES="
0001-Midgard-daptation-to-Linux-4.10.0-rcX-signatures.patch
0002-UMP-Adapt-get_user_pages-calls.patch
0003-Renamed-Kernel-DMA-Fence-structures-and-functions.patch
0004-Few-modifications-after-v4.11-headers-and-signatures.patch
0005-Using-the-new-header-on-4.12-kernels-for-copy_-_user.patch
"

# -- Helper functions

function die_on_error {
	if [ ! $? = 0 ]; then
		echo $1
		exit 1
	fi
}

function download_patches {
	base_url=$1
	patches=${@:2}
	for patch in $patches; do
		wget $base_url/$patch ||
		{ echo "Could not download $patch"; exit 1; }
	done
}

function download_and_apply_patches {
	base_url=$1
	patches=${@:2}
	download_patches $base_url $patches
	git apply $patches
	rm $patches
}

function copy_and_apply_patches {
	patch_base_dir=$1
	patches=${@:2}
	
	apply_dir=$PWD
	cd $patch_base_dir
	cp $patches $apply_dir || 
	{ echo "Could not copy $patch"; exit 1; }
	cd $apply_dir
	git apply $patches
	rm $patches
}

# Get the kernel

# If we haven't already clone the Linux Kernel tree, clone it and move
# into the linux folder created during the cloning.
if [ ! -d "linux" ]; then
  git clone --depth 1 --branch $KERNEL_BRANCH $KERNEL_GIT_URL linux
  die_on_error "Could not Git the kernel"
fi
cd linux
die_on_error "No linux folder !?"
export SRC_DIR=$PWD

# Check if the tree is patched
if [ ! -e ".is_patched" ]; then
  # If not, cleanup, apply the patches, commit and mark the tree as 
  # patched
  
  # Remove all untracked files. These are residues from failed runs
  git clean -fdx &&
  # Rewind modified files to their initial state.
  git checkout -- .

  # Download, prepare and copy the Mali Kernel-Space drivers. 
  # Some TGZ are AWFULLY packaged with everything having 0777 rights.
  
  wget "$MALI_BASE_URL/TX011-SW-99002-$MALI_VERSION.tgz" &&
  tar zxvf TX011-SW-99002-$MALI_VERSION.tgz &&
  cd TX011-SW-99002-$MALI_VERSION &&
  find . -type 'f' -exec chmod 0644 {} ';' && # Every file   should have -rw-r--r-- rights
  find . -type 'd' -exec chmod 0755 {} ';' && # Every folder should have drwxr-xr-x rights
  find . -name 'sconscript' -exec rm {} ';' && # Remove sconscript files. Useless.
  cd driver/product/kernel &&
  rm -r 'patches' 'license.txt' && # Remove the patches and GPL license file.
  cp -r drivers/gpu/arm  $SRC_DIR/drivers/gpu/ && # Copy the Midgard code
  cp -r drivers/base/ump $SRC_DIR/drivers/base/ && # Copy the Unified Memory Provider code
  cp include/linux/ump*  $SRC_DIR/include/linux/ && # Copy the Unified Memory Provider headers.
  cp include/linux/kds.h $SRC_DIR/include/linux/ && # Copy the Kernel Dependency System header ↑ (dependency)
  cd $SRC_DIR &&
  rm -r TX011-SW-99002-$MALI_VERSION TX011-SW-99002-$MALI_VERSION.tgz
  
  # Download and apply the various kernel and Mali kernel-space driver patches
  if [ ! -d "../patches" ]; then
    download_and_apply_patches $KERNEL_PATCHES_FOLDER_URL $KERNEL_PATCHES
    download_and_apply_patches $MALI_PATCHES_FOLDER $MALI_PATCHES
  else
    copy_and_apply_patches ../patches/kernel/$KERNEL_SERIES $KERNEL_PATCHES
    copy_and_apply_patches ../patches/Mali/$MALI_VERSION $MALI_PATCHES
  fi


  # Cleanup, get the configuration file and mark the tree as patched
  git add . &&
  git commit -m "Apply ALL THE PATCHES !" &&
  touch .is_patched
fi

# Download a .config file if none present
if [ ! -e ".config" ]; then
  make mrproper &&
  wget -O .config "$BASE_FILES_URL/$GITHUB_REPO/$GIT_BRANCH/boot/config-$KERNEL_VERSION$MYY_VERSION"
fi

make $MAKE_CONFIG
make $DTB_FILES zImage modules -j5

if [ -z ${DONT_INSTALL_IN_TMP+x} ]; then
	# Kernel compiled
	# This will just copy the kernel files and libraries in /tmp
	# This part is only useful if you're cross-compiling the kernel, of course
	export INSTALL_MOD_PATH=/tmp/MyyQi
	export INSTALL_PATH=/tmp/MyyQi/boot
	export INSTALL_HDR_PATH=/tmp/usr
	mkdir -p $INSTALL_MOD_PATH $INSTALL_PATH $INSTALL_HDR_PATH
	make modules_install &&
	make install &&
	make INSTALL_HDR_PATH=$INSTALL_HDR_PATH headers_install && # This command IGNORES predefined variables
	cp arch/arm/boot/zImage $INSTALL_PATH &&
	cp arch/arm/boot/dts/*.dtb $INSTALL_PATH
fi

```

This procedure was stored in the **[GetPatchAndCompileKernel.sh](./GetPatchAndCompileKernel.sh)** file and can be run like this :
```bash
sh GetPatchAndCompileKernel.sh
```

You will need compiling tools, **git**, **wget** and **find** in order to execute this procedure successfully.

The patches applied are stored in the **[patches/](./patches/)** folder.

To install this kernel, copy the **zImage** and the **rk3288-miqi.dtb** file in your boot partition.
Note that if you have access to U-boot through a serial console AND your MiQi is powered through your USB computer, you can access the whole eMMC like a USB memory stick using the following command :
```
ums 0 mmc 1
```

Debian packages for release versions
------------------------------------

[Debian packages containing releases versions of kernels patched the same way are available in Armbian repositories](https://www.armbian.com/kernel/), thanks to [Armbian](https://www.armbian.com/)'s team. Note that Armbian also provides [Debian installation scripts](https://docs.armbian.com/User-Guide_Getting-Started/) and [cross-building scripts (Building ARM Debian images using Intel/AMD machines)](https://docs.armbian.com/Developer-Guide_Build-Preparation/).

If you already have a Debian system, you'll just have to add the [beta.armbian.com](https://beta.armbian.com) Debian repository and do :

    apt install linux-image-dev-rockchip linux-headers-dev-rockchip linux-dtb-dev-miqi

`linux-dtb-dev-miqi` being useful if you're installing this kernel on a [MiQi](https://mqmaker.com/miqi_retailers/) board.

TODO
----

- [x] Document how to use the generated kernel and boot it
- [x] Add the [Open Source Kernel-space Mali Midgard drivers](http://malideveloper.arm.com/resources/drivers/open-source-mali-midgard-gpu-kernel-drivers/)
- [ ] Add [gator](https://github.com/ARM-software/gator) (Generates lock-ups)
- [ ] Document how to use [DS-5 : Streamline](https://developer.arm.com/products/software-development-tools/ds-5-development-studio/streamline/overview) to analyse OpenGL ES 2.x/3.x programs running on MiQi boards using such kernels. (Generates lock-ups)
- [ ] Integrating Rockchip VPU code (In progress)



