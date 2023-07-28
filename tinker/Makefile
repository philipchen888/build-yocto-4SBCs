export ARCH=arm
export CROSS_COMPILE=/usr/local/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf-

u-boot: builduboot postbuilduboot

builduboot:
	cd debian_u-boot; \
	make clean; \
	make ARCH=arm tinker-rk3288_defconfig; \
	make all

postbuilduboot:
	cd debian_u-boot; \
	./tools/mkimage -n rk3288 -T rksd -d ./spl/u-boot-spl-dtb.bin u-boot.img; \
	cat ./u-boot.bin >> u-boot.img

mykernel:
	cd debian_kernel; mkdir tmp; cd tmp; mkdir lib; cd ../; \
	cp ../patches/rk3288-miniarm.dts ./arch/arm/boot/dts/; \
	make ARCH=arm clean; \
	make ARCH=arm miniarm-rk3288_defconfig; \
	make ARCH=arm all -j4; \
	sudo make ARCH=arm INSTALL_MOD_PATH=./tmp modules_install

apply_patches:
	cd poky/build; \
	cp -r ../../patches/recipes-graphics ../meta-radxa/; \
	cp -r ../../patches/images ../meta-radxa/recipes-core/; \
	cp -r ../../patches/packagegroups ../meta-radxa/recipes-core/; \
	cp ../../patches/tinker-rk3288.conf ../meta-radxa/conf/machine/; \
	cp ../../patches/tinker-img.bbclass ../meta-radxa/classes/; \
	cp ../../patches/tcmode-default.inc ../meta/conf/distro/include/; \
	cp ../../patches/bblayers.conf ./conf/; \
	cp ../../patches/local.conf ./conf/
