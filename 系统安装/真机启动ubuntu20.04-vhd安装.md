# 真机启动ubuntu20.04-vhd安装



## 1. 所需软件

###     1.1 ubuntu20.04镜像 （http://mirrors.ustc.edu.cn/ubuntu-releases/）

###     1.2 BOOTICE64位（修改启动项  挂载和分离vhd虚拟硬盘）

###     1.3 DiskGenius5.2_x64 （硬盘分区格式化工具）

###     1.4 vmware15.05 版本 虚拟机软件



## 2. 生成固定vhd文件，用vmware将ubuntu20镜像装入vhd中



## 3.修改ubuntu文件 让真机可以引导

### 1.安装所需软件

```
sudo apt-get install kpartx kpartx-boot util-linux dmsetup lvm2
sudo apt-get install  gcc g++ build-essential
```

### 2.编译安装ntfs-3g

```
把ntfs-3g_ntfsprogs-2017.3.23-fixed文件夹放到主目录下,打开一个终端.进入该目录.依次执行以下命令:

cd ~/ntfs-3g_ntfsprogs-2017.3.23-fixed
./configure
make
sudo make install
```

### 3.修改文件

#### 3.1  修改init文件，删除文件内所有内容，粘贴上下面内容

```
sudo cp /usr/share/initramfs-tools/init ~/init.backup
sudo gedit /usr/share/initramfs-tools/init
```

```
#!/bin/sh

# Default PATH differs between shells, and is not automatically exported
# by klibc dash.  Make it consistent.
# Furthermore, this PATH ends up being used by the init, set it to the
# Standard PATH, without /snap/bin as documented in
# https://wiki.ubuntu.com/PATH
# This also matches /etc/environment, but without games path
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

[ -d /dev ] || mkdir -m 0755 /dev
[ -d /root ] || mkdir -m 0700 /root
[ -d /sys ] || mkdir /sys
[ -d /proc ] || mkdir /proc
[ -d /tmp ] || mkdir /tmp
mkdir -p /var/lock
mount -t sysfs -o nodev,noexec,nosuid sysfs /sys
mount -t proc -o nodev,noexec,nosuid proc /proc

# shellcheck disable=SC2013
for x in $(cat /proc/cmdline); do
	case $x in
	initramfs.clear)
		clear
		;;
	quiet)
		quiet=y
		;;
	esac
done

if [ "$quiet" != "y" ]; then
	quiet=n
	echo "Loading, please wait..."
fi
export quiet

# Note that this only becomes /dev on the real filesystem if udev's scripts
# are used; which they will be, but it's worth pointing out
test -x /usr/sbin/v86d && dev_exec="exec" || dev_exec="noexec"
mount -t devtmpfs -o $dev_exec,nosuid,mode=0755 udev /dev
mkdir /dev/pts
mount -t devpts -o noexec,nosuid,gid=5,mode=0620 devpts /dev/pts || true

# Export the dpkg architecture
export DPKG_ARCH=
. /conf/arch.conf

# Set modprobe env
export MODPROBE_OPTIONS="-qb"

# Export relevant variables
export ROOT=
export ROOTDELAY=
export ROOTFLAGS=
export ROOTFSTYPE=
export IP=
export IP6=
export VLAN=
export DEVICE=
export BOOT=
export BOOTIF=
export UBIMTD=
export NETWORK_SKIP_ENSLAVED=
export break=
export init=/sbin/init
export readonly=y
export rootmnt=/root
export debug=
export panic=
export blacklist=
export resume=
export resume_offset=
export noresume=
export drop_caps=
export fastboot=n
export forcefsck=n
export fsckfix=
export recovery=
export NETWORK_SKIP_ENSLAVED=
export KLOOP=
export KROOT=
export KLVM=
export HOSTFSTYPE=
export KLOOPFSTYPE=
export SQUSHFS=
export UPPERDIR=
export LOWERDIR=
export WORKDIR=
export QEMUNBD=

# mdadm needs hostname to be set. This has to be done before the udev rules are called!
if [ -f "/etc/hostname" ]; then
        /bin/hostname -F /etc/hostname >/dev/null 2>&1
fi

# Bring in the main config
. /conf/initramfs.conf
for conf in conf/conf.d/*; do
	[ -f "${conf}" ] && . "${conf}"
done
. /scripts/functions

# Parse command line options
# shellcheck disable=SC2013
for x in $(cat /proc/cmdline); do
	case $x in
	init=*)
		init=${x#init=}
		;;
	root=*)
		ROOT=${x#root=}
		if [ -z "${BOOT}" ] && [ "$ROOT" = "/dev/nfs" ]; then
			BOOT=nfs
		fi
		;;
	rootflags=*)
		ROOTFLAGS="-o ${x#rootflags=}"
		;;
	rootfstype=*)
		ROOTFSTYPE="${x#rootfstype=}"
		;;
	rootdelay=*)
		ROOTDELAY="${x#rootdelay=}"
		case ${ROOTDELAY} in
		*[![:digit:].]*)
			ROOTDELAY=
			;;
		esac
		;;
	roottimeout=*)
		ROOTDELAY="${x#roottimeout=}"
		case ${ROOTDELAY} in
		*[![:digit:].]*)
			ROOTDELAY=
			;;
		esac
		;;
	loop=*)
		# shellcheck disable=SC2034
		LOOP="${x#loop=}"
		;;
	loopflags=*)
		# shellcheck disable=SC2034
		LOOPFLAGS="-o ${x#loopflags=}"
		;;
	loopfstype=*)
		# shellcheck disable=SC2034
		LOOPFSTYPE="${x#loopfstype=}"
		;;
	nfsroot=*)
		# shellcheck disable=SC2034
		NFSROOT="${x#nfsroot=}"
		;;
	initramfs.runsize=*)
		RUNSIZE="${x#initramfs.runsize=}"
		;;
	ip=*)
		IP="${x#ip=}"
		;;
	ip6=*)
		IP6="${x#ip6=}"
		;;
	vlan=*)
		VLAN="${x#vlan=}"
		;;
	boot=*)
		BOOT=${x#boot=}
		;;
	ubi.mtd=*)
		UBIMTD=${x#ubi.mtd=}
		;;
	resume=*)
		RESUME="${x#resume=}"
		case $RESUME in
		UUID=*)
			RESUME="/dev/disk/by-uuid/${RESUME#UUID=}"
		esac
		;;
	resume_offset=*)
		resume_offset="${x#resume_offset=}"
		;;
	noresume)
		noresume=y
		;;
	drop_capabilities=*)
		drop_caps="-d ${x#drop_capabilities=}"
		;;
	panic=*)
		panic="${x#panic=}"
		case ${panic} in
		-1) ;;
		*[![:digit:].]*)
			panic=
			;;
		esac
		;;
	ro)
		readonly=y
		;;
	rw)
		readonly=n
		;;
	debug)
		debug=y
		quiet=n
		if [ -n "${netconsole}" ]; then
			log_output=/dev/kmsg
		else
			log_output=/run/initramfs/initramfs.debug
		fi
		set -x
		;;
	debug=*)
		debug=y
		quiet=n
		set -x
		;;
	break=*)
		break=${x#break=}
		;;
	break)
		break=premount
		;;
	blacklist=*)
		blacklist=${x#blacklist=}
		;;
	netconsole=*)
		netconsole=${x#netconsole=}
		[ "x$debug" = "xy" ] && log_output=/dev/kmsg
		;;
	BOOTIF=*)
		BOOTIF=${x#BOOTIF=}
		;;
	hwaddr=*)
		BOOTIF=${x#hwaddr=}
		;;
	fastboot|fsck.mode=skip)
		fastboot=y
		;;
	forcefsck|fsck.mode=force)
		forcefsck=y
		;;
	fsckfix|fsck.repair=yes)
		fsckfix=y
		;;
	fsck.repair=no)
		fsckfix=n
		;;
	kloop=*)
		KLOOP=${x#kloop=}
		;;
	kroot=*)
		KROOT=${x#kroot=}
		;;
	klvm=*)
		KLVM=${x#klvm=}
		;;
	hostfstype=*)
		HOSTFSTYPE=${x#hostfstype=}
		;;
	kloopfstype=*)
		KLOOPFSTYPE=${x#kloopfstype=}
		;;
	squashfs=*)
		SQUASHFS=${x#squashfs=}
		;;
	upperdir=*)
		UPPERDIR=${x#upperdir=}
		;;
	lowerdir=*)
		LOWERDIR=${x#lowerdir=}
		;;
	workdir=*)
		WORKDIR=${x#workdir=}
		;;
	qemunbd=*)
		QEMUNBD=${x#qemunbd=}
		;;	
	esac
done

# Default to BOOT=local if no boot script defined.
if [ -z "${BOOT}" ]; then
	BOOT=local
fi

if [ -n "${noresume}" ] || [ "$RESUME" = none ]; then
	noresume=y
else
	resume=${RESUME:-}
fi

mount -t tmpfs -o "nodev,noexec,size=${RUNSIZE:-10%},mode=0755" tmpfs /run
mkdir -m 0700 /run/initramfs

if [ -n "$log_output" ]; then
	exec >$log_output 2>&1
	unset log_output
fi

maybe_break top

# export BOOT variable value for compcache,
# so we know if we run from casper
export BOOT

# Don't do log messages here to avoid confusing graphical boots
run_scripts /scripts/init-top

maybe_break modules
[ "$quiet" != "y" ] && log_begin_msg "Loading essential drivers"
[ -n "${netconsole}" ] && modprobe netconsole netconsole="${netconsole}"
load_modules
[ "$quiet" != "y" ] && log_end_msg

maybe_break premount
[ "$quiet" != "y" ] && log_begin_msg "Running /scripts/init-premount"
run_scripts /scripts/init-premount
[ "$quiet" != "y" ] && log_end_msg

maybe_break mount
log_begin_msg "Mounting root file system"
# Always load local and nfs (since these might be needed for /etc or
# /usr, irrespective of the boot script used to mount the rootfs).
. /scripts/local
. /scripts/nfs
. /scripts/${BOOT}
parse_numeric "${ROOT}"
maybe_break mountroot
mount_top
mount_premount
mountroot
log_end_msg

if read_fstab_entry /usr; then
	log_begin_msg "Mounting /usr file system"
	mountfs /usr
	log_end_msg
fi

# Mount cleanup
mount_bottom
nfs_bottom
local_bottom

maybe_break bottom
[ "$quiet" != "y" ] && log_begin_msg "Running /scripts/init-bottom"
# We expect udev's init-bottom script to move /dev to ${rootmnt}/dev
run_scripts /scripts/init-bottom
[ "$quiet" != "y" ] && log_end_msg

# Move /run to the root
mount -n -o move /run ${rootmnt}/run

validate_init() {
	run-init -n "${rootmnt}" "${1}"
}

# Check init is really there
if ! validate_init "$init"; then
	echo "Target filesystem doesn't have requested ${init}."
	init=
	for inittest in /sbin/init /etc/init /bin/init /bin/sh; do
		if validate_init "${inittest}"; then
			init="$inittest"
			break
		fi
	done
fi

# No init on rootmount
if ! validate_init "${init}" ; then
	panic "No init found. Try passing init= bootarg."
fi

maybe_break init

# don't leak too much of env - some init(8) don't clear it
# (keep init, rootmnt, drop_caps)
unset debug
unset MODPROBE_OPTIONS
unset DPKG_ARCH
unset ROOTFLAGS
unset ROOTFSTYPE
unset ROOTDELAY
unset ROOT
unset IP
unset IP6
unset VLAN
unset BOOT
unset BOOTIF
unset DEVICE
unset UBIMTD
unset blacklist
unset break
unset noresume
unset panic
unset quiet
unset readonly
unset resume
unset resume_offset
unset noresume
unset fastboot
unset forcefsck
unset fsckfix

unset kloop
unset kroot
unset klvm
unset hostfstype
unset kloopfstype
unset squashfs
unset upperdir
unset lowerdir
unset workdir
unset qemunbd
# Move virtual filesystems over to the real filesystem
mount -n -o move /sys ${rootmnt}/sys
mount -n -o move /proc ${rootmnt}/proc

# Chain to real filesystem
# shellcheck disable=SC2086,SC2094
exec run-init ${drop_caps} "${rootmnt}" "${init}" "$@" <"${rootmnt}/dev/console" >"${rootmnt}/dev/console" 2>&1
echo "Something went badly wrong in the initramfs."
panic "Please file a bug on initramfs-tools."

```

#### 3.2  修改local文件，删除文件内所有内容，粘贴上下面内容

```
sudo cp /usr/share/initramfs-tools/scripts/local ~/local.backup
sudo gedit /usr/share/initramfs-tools/scripts/local
```

```
# Local filesystem mounting			-*- shell-script -*-

local_top()
{
	if [ "${local_top_used}" != "yes" ]; then
		[ "${quiet?}" != "y" ] && log_begin_msg "Running /scripts/local-top"
		run_scripts /scripts/local-top
		[ "$quiet" != "y" ] && log_end_msg
	fi
	local_top_used=yes

	# Start time for measuring elapsed time in local_device_setup
	if [ -z "${local_top_time}" ]; then
		local_top_time="$(cat /proc/uptime)"
		local_top_time="${local_top_time%%[. ]*}"
		local_top_time=$((local_top_time + 1)) # round up
		export local_top_time
	fi
}

local_block()
{
	[ "${quiet?}" != "y" ] && log_begin_msg "Running /scripts/local-block"
	run_scripts /scripts/local-block "$@"
	[ "$quiet" != "y" ] && log_end_msg
}

local_premount()
{
	if [ "${local_premount_used}" != "yes" ]; then
		[ "${quiet?}" != "y" ] && log_begin_msg "Running /scripts/local-premount"
		run_scripts /scripts/local-premount
		[ "$quiet" != "y" ] && log_end_msg
	fi
	local_premount_used=yes
}

local_bottom()
{
	if [ "${local_premount_used}" = "yes" ] || [ "${local_top_used}" = "yes" ]; then
		[ "${quiet?}" != "y" ] && log_begin_msg "Running /scripts/local-bottom"
		run_scripts /scripts/local-bottom
		[ "$quiet" != "y" ] && log_end_msg
	fi
	local_premount_used=no
	local_top_used=no
	unset local_top_time
}

# $1=device ID to mount
# $2=optionname (for root and etc)
# $3=panic if device is missing (true or false, default: true)
# Sets $DEV to the resolved device node
local_device_setup()
{
	local dev_id="$1"
	local name="$2"
	local may_panic="${3:-true}"
	local real_dev
	local time_elapsed
	local count

	# If wait-for-root understands this prefix, then use it to wait for
	# the device rather than settling the whole of udev.

	# Timeout is max(30, rootdelay) seconds (approximately)
	local slumber=30
	case $DPKG_ARCH in
		powerpc|ppc64|ppc64el)
			slumber=180
			;;
		*)
			slumber=30
			;;
	esac
	if [ "${ROOTDELAY:-0}" -gt $slumber ]; then
		slumber=$ROOTDELAY
	fi

	case "$dev_id" in
	UUID=*|LABEL=*|PARTUUID=*|/dev/*)
		FSTYPE=$( wait-for-root "$dev_id" "$slumber" )
		;;
	*)
		wait_for_udev 10
		;;
	esac

	# Load ubi with the correct MTD partition and return since fstype
	# doesn't work with a char device like ubi.
	if [ -n "$UBIMTD" ]; then
		modprobe ubi "mtd=$UBIMTD"
		DEV="${dev_id}"
		return
	fi

	# Don't wait for a device that doesn't have a corresponding
	# device in /dev and isn't resolvable by blkid (e.g. mtd0)
	if [ "${dev_id#/dev}" = "${dev_id}" ] &&
	   [ "${dev_id#*=}" = "${dev_id}" ]; then
		DEV="${dev_id}"
		return
	fi

	# If the root device hasn't shown up yet, give it a little while
	# to allow for asynchronous device discovery (e.g. USB).  We
	# also need to keep invoking the local-block scripts in case
	# there are devices stacked on top of those.
	#
	# in Ubuntu, we should never actually enter this loop as wait-for-root
	# above should have waited until the device appeared.
	if ! real_dev=$(resolve_device "${dev_id}") ||
	   ! get_fstype "${real_dev}" >/dev/null; then
		log_begin_msg "Waiting for ${name}"

		while true; do
			sleep 1
			time_elapsed="$(cat /proc/uptime)"
			time_elapsed="${time_elapsed%%[. ]*}"
			time_elapsed=$((time_elapsed - local_top_time))

			local_block "${dev_id}"

			# If mdadm's local-block script counts the
			# number of times it is run, make sure to
			# run it the expected number of times.
			while true; do
				if [ -f /run/count.mdadm.initrd ]; then
					count="$(cat /run/count.mdadm.initrd)"
				elif [ -n "${count}" ]; then
					# mdadm script deleted it; put it back
					count=$((count + 1))
					echo "${count}" >/run/count.mdadm.initrd
				else
					break
				fi
				if [ ${count} -ge ${time_elapsed} ]; then
					break;
				fi
				/scripts/local-block/mdadm "${dev_id}"
			done

			if real_dev=$(resolve_device "${dev_id}") &&
			   get_fstype "${real_dev}" >/dev/null; then
				wait_for_udev 10
				log_end_msg 0
				break
			fi
			if [ ${time_elapsed} -ge "${slumber}" ]; then
				log_end_msg 1 || true
				break
			fi
		done
	fi

	# We've given up, but we'll let the user fix matters if they can
	while ! real_dev=$(resolve_device "${dev_id}") ||
	      ! get_fstype "${real_dev}" >/dev/null; do
		if ! $may_panic; then
			echo "Gave up waiting for ${name}"
			return 1
		fi
		echo "Gave up waiting for ${name} device.  Common problems:"
		echo " - Boot args (cat /proc/cmdline)"
		echo "   - Check rootdelay= (did the system wait long enough?)"
		if [ "${name}" = root ]; then
			echo "   - Check root= (did the system wait for the right device?)"
		fi
		echo " - Missing modules (cat /proc/modules; ls /dev)"
		panic "ALERT!  ${dev_id} does not exist.  Dropping to a shell!"
	done

	DEV="${real_dev}"
}

local_mount_root()
{
	local_top
	local_device_setup "${ROOT}" "root file system"
	ROOT="${DEV}"

	# Get the root filesystem type if not set
	if [ -z "${ROOTFSTYPE}" ]; then
		FSTYPE=$(get_fstype "${ROOT}")
	else
		FSTYPE=${ROOTFSTYPE}
	fi

	local_premount

if [ -z "$KLOOP" ] && [ -z "$SQUASHFS" ] && [ -z "$UPPERDIR" ] && [ -z "$QEMUNBD" ] ; then

	if [ "${readonly}" = "y" ] && \
	   [ -z "$LOOP" ]; then
		roflag=-r
	else
		roflag=-w
	fi

	# FIXME This has no error checking
	[ -n "${FSTYPE}" ] && modprobe ${FSTYPE}

	checkfs ${ROOT} root "${FSTYPE}"

	# FIXME This has no error checking
	# Mount root
	mount ${roflag} ${FSTYPE:+-t ${FSTYPE} }${ROOTFLAGS} ${ROOT} ${rootmnt}
	mountroot_status="$?"
	if [ "$LOOP" ]; then
		if [ "$mountroot_status" != 0 ]; then
			if [ ${FSTYPE} = ntfs ] || [ ${FSTYPE} = vfat ]; then
				panic "
Could not mount the partition ${ROOT}.
This could also happen if the file system is not clean because of an operating
system crash, an interrupted boot process, an improper shutdown, or unplugging
of a removable device without first unmounting or ejecting it.  To fix this,
simply reboot into Windows, let it fully start, log in, run 'chkdsk /r', then
gracefully shut down and reboot back into Windows. After this you should be
able to reboot again and resume the installation.
(filesystem = ${FSTYPE}, error code = $mountroot_status)
"
			fi
		fi

		mkdir -p /host
		mount -o move ${rootmnt} /host

		while [ ! -e "/host/${LOOP#/}" ]; do
			panic "ALERT!  /host/${LOOP#/} does not exist.  Dropping to a shell!"
		done

		# Get the loop filesystem type if not set
		if [ -z "${LOOPFSTYPE}" ]; then
			eval $(fstype < "/host/${LOOP#/}")
		else
			FSTYPE="${LOOPFSTYPE}"
		fi
		if [ "$FSTYPE" = "unknown" ] && [ -x /sbin/blkid ]; then
			FSTYPE=$(/sbin/blkid -s TYPE -o value "/host/${LOOP#/}")
			[ -z "$FSTYPE" ] && FSTYPE="unknown"
		fi

		if [ ${readonly} = y ]; then
			roflag=-r
		else
			roflag=-w
		fi

		# FIXME This has no error checking
		modprobe loop
		modprobe ${FSTYPE}

		# FIXME This has no error checking
		mount ${roflag} -o loop -t ${FSTYPE} ${LOOPFLAGS} "/host/${LOOP#/}" ${rootmnt}

		if [ -d ${rootmnt}/host ]; then
			mount -o move /host ${rootmnt}/host
		fi
	fi
fi

	#########################################################
	#                       kloop by niumao			#
	#########################################################

if [ -n "$KLOOP" ]; then

	### reset the value of the root variable 
	HOSTDEV="${ROOT}"
	NEWROOT="${rootmnt}"
	[ -n "$KROOT" ] && ROOT="$KROOT"
	[ -n "$KROOT" ] || ROOT="/dev/loop0"
	export ROOT
	realroot="$ROOT"

	###  auto probe the fs-type of the partition in which vhd-file live and mount it  /host 
	mkdir -p /host
	if [ -e ${NEWROOT}${KLOOP} ]; then
		mount --move $NEWROOT /host
	else	
		if [ -z "$HOSTFSTYPE" ]; then
			HOSTFSTYPE="$(blkid -s TYPE -o value "${HOSTDEV}")"
			[ -z "$HOSTFSTYPE"  -o "${HOSTFSTYPE}" = "ntfs" ] && HOSTFSTYPE="ntfs-3g"
		fi
		[ "${HOSTFSTYPE}" = "ntfs-3g" ] || modprobe ${HOSTFSTYPE}
		mount -t ${HOSTFSTYPE} -o rw  ${HOSTDEV}  /host
	fi
	
	### mount the vhd-file on a loop-device 
	if [ "${KLOOP#/}" !=  "${KLOOP}" ]; then       	
		modprobe  loop  
		kpartx -av /host${KLOOP}
		[ -e "$realroot" ] || sleep 3
	fi

	### probe lvm on vhd-file
	if [ -n "$KLVM" ];  then
		modprobe dm-mod
		vgscan
		vgchange  -ay  ${KLVM}
		[ -e "$realroot" ] ||  sleep 3
	fi 

	if [ "${readonly}" = "y" ] ; then
		roflag="-r"
	else
		roflag="-w"
	fi
	 
	### mount the realroot / in vhd-file on $NEWROOT 
	if [ -z "${KLOOPFSTYPE}" ]; then
		KLOOPFSTYPE="$(blkid -s TYPE -o value "$realroot")"
		[ -z "${KLOOPFSTYPE}" ] && KLOOPFSTYPE="ext4"
	fi
	[ -e "$realroot" ] || sleep 3
	mount    ${roflag} -t "${KLOOPFSTYPE}"  $realroot $NEWROOT
	
	### mount /host in initrd to /host of the realrootfs
	[ -d  ${NEWROOT}/host ] || mkdir -p ${NEWROOT}/host 
	mount --move /host   ${NEWROOT}/host
fi

if [ -n "$SQUASHFS" ];  then

	### reset the value of the root variable 
	HOSTDEV="${ROOT}"
	NEWROOT="${rootmnt}"
	
	###  auto probe the fs-type of the partition in which vhd-file live and mount it  /host 
	mkdir -p /host
	if [ -e ${NEWROOT}${SQUASHFS} ]; then
		mount --move $NEWROOT /host
	else	
		if [ -z "$HOSTFSTYPE" ]; then
			HOSTFSTYPE="$(blkid -s TYPE -o value "${HOSTDEV}")"
			[ -z "$HOSTFSTYPE"  -o "${HOSTFSTYPE}" = "ntfs" ] && HOSTFSTYPE="ntfs-3g"
		fi
		[ "${HOSTFSTYPE}" = "ntfs-3g" ] || modprobe ${HOSTFSTYPE}
		mount -t ${HOSTFSTYPE} -o rw  ${HOSTDEV}  /host
	fi
	
	###try to boot from squashfs
	modprobe overlay
	mkdir  -p /run/lowerdir /run/upperdir  /run/workdir
	mount  /host$SQUASHFS  /run/lowerdir
	mount  -t overlay overlay -o lowerdir=/run/lowerdir,upperdir=/run/upperdir,workdir=/run/workdir    $NEWROOT

	### mount /host in initrd to /host of the realrootfs
	[ -d  ${NEWROOT}/host ] || mkdir -p ${NEWROOT}/host 
	mount --move /host   ${NEWROOT}/host
fi	

if [ -n "$UPPERDIR" ] && [ -n "$WORKDIR" ];  then

	### reset the value of the root variable 
	HOSTDEV="${ROOT}"
	NEWROOT="${rootmnt}"
	
	###  auto probe the fs-type of the partition in which vhd-file live and mount it  /host 
	mkdir -p /host
	if [ -e ${NEWROOT}${UPPERDIR} ]; then
		mount --move $NEWROOT /host
	else	
		if [ -z "$HOSTFSTYPE" ]; then
			HOSTFSTYPE="$(blkid -s TYPE -o value "${HOSTDEV}")"
			[ -z "$HOSTFSTYPE"  -o "${HOSTFSTYPE}" = "ntfs" ] && HOSTFSTYPE="ntfs-3g"
		fi
		[ "${HOSTFSTYPE}" = "ntfs-3g" ] || modprobe ${HOSTFSTYPE}
		mount -t ${HOSTFSTYPE} -o rw  ${HOSTDEV}  /host
	fi
		
	###try to boot from dir
	modprobe overlay
	mkdir  /run/lowerdir 
	mount  -t overlay overlay -o lowerdir=/run/lowerdir,upperdir=/host$UPPERDIR,workdir=/host$WORKDIR  $NEWROOT

	### mount /host in initrd to /host of the realrootfs
	[ -d  ${NEWROOT}/host ] || mkdir -p ${NEWROOT}/host 
	mount --move /host   ${NEWROOT}/host
fi	


if  [ -n "$QEMUNBD" ] ; then

	### reset the value of the root variable 
	HOSTDEV="${ROOT}"
	NEWROOT="${rootmnt}"
	[ -n "$KROOT" ] && ROOT="$KROOT"
	[ -n "$KROOT" ] || ROOT="/dev/loop0"
	export ROOT
	realroot="$ROOT"

	###  auto probe the fs-type of the partition in which vhd-file live and mount it  /host 
	mkdir -p /host
	if [ -e $NEWROOT$QEMUNBD ]; then
		mount --move $NEWROOT /host
	else	
		if [ -z "$HOSTFSTYPE" ]; then
			HOSTFSTYPE="$(blkid -s TYPE -o value "${HOSTDEV}")"
			[ -z "$HOSTFSTYPE"  -o "${HOSTFSTYPE}" = "ntfs" ] && HOSTFSTYPE="ntfs-3g"
		fi
		[ "${HOSTFSTYPE}" = "ntfs-3g" ] || modprobe ${HOSTFSTYPE}
		mount -t ${HOSTFSTYPE} -o rw  ${HOSTDEV}  /host
	fi
	
	### mount the vhd-file on a loop-device 
	if [ "${QEMUNBD#/}" !=  "${QEMUNBD}" ]; then       	
		modprobe  nbd max_part=8 
		modprobe  loop
		[ -e  /dev/nbd0 ] || sleep 3 
		qemu-nbd  -c /dev/nbd0  /host${QEMUNBD}
		kpartx -av /dev/nbd0
		[ -e "$realroot" ] || sleep 3
	fi

	if [ "${readonly}" = "y" ] ; then
		roflag="-r"
	else
		roflag="-w"
	fi
	 
	### mount the realroot / in vhd-file on $NEWROOT 
	if [ -z "${KLOOPFSTYPE}" ]; then
		KLOOPFSTYPE="$(blkid -s TYPE -o value "$realroot")"
		[ -z "${KLOOPFSTYPE}" ] && KLOOPFSTYPE="ext4"
	fi
	[ -e "$realroot" ] || sleep 3
	mount    ${roflag} -t "${KLOOPFSTYPE}"  $realroot $NEWROOT
	
	### mount /host in initrd to /host of the realrootfs
	[ -d  ${NEWROOT}/host ] || mkdir -p ${NEWROOT}/host 
	mount --move /host   ${NEWROOT}/host
fi

	#########################################################
	#                       kloop by niumao			#
	#########################################################


}

local_mount_fs()
{
	read_fstab_entry "$1"

	local_device_setup "$MNT_FSNAME" "$1 file system"
	MNT_FSNAME="${DEV}"

	local_premount

	if [ "${readonly}" = "y" ]; then
		roflag=-r
	else
		roflag=-w
	fi

	if [ "$MNT_PASS" != 0 ]; then
		checkfs "$MNT_FSNAME" "$MNT_DIR" "${MNT_TYPE}"
	fi

	# Mount filesystem
	if ! mount ${roflag} -t "${MNT_TYPE}" -o "${MNT_OPTS}" "$MNT_FSNAME" "${rootmnt}${MNT_DIR}"; then
		panic "Failed to mount ${MNT_FSNAME} as $MNT_DIR file system."
	fi
}

mountroot()
{
	local_mount_root
}

mount_top()
{
	# Note, also called directly in case it's overridden.
	local_top
}

mount_premount()
{
	# Note, also called directly in case it's overridden.
	local_premount
}

mount_bottom()
{
	# Note, also called directly in case it's overridden.
	local_bottom
}

```

#### 3.3  修改mkinitramfs文件，删除文件内所有内容，粘贴上下面内容

```
sudo cp /usr/sbin/mkinitramfs ~/mkinitramfs.backup
sudo gedit /usr/sbin/mkinitramfs
```

```
#!/bin/sh

umask 0022
export PATH='/usr/bin:/sbin:/bin'

# Defaults
keep="n"
CONFDIR="/etc/initramfs-tools"
verbose="n"
# Will be updated by busybox's conf hook, if present
BUSYBOXDIR=
export BUSYBOXDIR

usage()
{
	cat << EOF

Usage: mkinitramfs [option]... -o outfile [version]

Options:
  -c compress	Override COMPRESS setting in initramfs.conf.
  -d confdir	Specify an alternative configuration directory.
  -k		Keep temporary directory used to make the image.
  -o outfile	Write to outfile.
  -r root	Override ROOT setting in initramfs.conf.

See mkinitramfs(8) for further details.

EOF
}

usage_error()
{
	usage >&2
	exit 2
}

OPTIONS=$(getopt -o c:d:hko:r:v --long help -n "$0" -- "$@") || usage_error

eval set -- "$OPTIONS"

while true; do
	case "$1" in
	-c)
		compress="$2"
		shift 2
		;;
	-d)
		CONFDIR="$2"
		shift 2
		if [ ! -d "${CONFDIR}" ]; then
			echo "${0}: ${CONFDIR}: Not a directory" >&2
			exit 1
		fi
		;;
	-h|--help)
		usage
		exit 0
		;;
	-o)
		outfile="$2"
		shift 2
		;;
	-k)
		keep="y"
		shift
		;;
	-r)
		ROOT="$2"
		shift 2
		;;
	-v)
		verbose="y"
		shift
		;;
	--)
		shift
		break
		;;
	*)
		echo "Internal error!" >&2
		exit 1
		;;
	esac
done

# For dependency ordered mkinitramfs hook scripts.
. /usr/share/initramfs-tools/scripts/functions
. /usr/share/initramfs-tools/hook-functions

# Auto-export any variables set in the conf snippets such that
# initramfs hooks can configure each other
# i.e. initramfs-tools-ubuntu-core affecting
# intel-microcode/amd-microcode hook defaults
set -a

. "${CONFDIR}/initramfs.conf"

EXTRA_CONF=''
maybe_add_conf() {
	if [ -e "$1" ] && \
	   basename "$1" \
	   | grep '^[[:alnum:]][[:alnum:]\._-]*$' \
	   | grep -qv '\.dpkg-.*$'; then
		if [ -d "$1" ]; then
			echo "W: $1 is a directory instead of file" >&2
		else
			EXTRA_CONF="${EXTRA_CONF} $1"
			. "$1"
		fi
	fi
}
for i in /usr/share/initramfs-tools/conf.d/*; do
	# Configuration files in /etc mask those in /usr/share
	if ! [ -e "${CONFDIR}"/conf.d/"$(basename "${i}")" ]; then
		maybe_add_conf "${i}"
	fi
done
for i in "${CONFDIR}"/conf.d/*; do
	maybe_add_conf "${i}"
done

# source package confs
for i in /usr/share/initramfs-tools/conf-hooks.d/*; do
	if [ -d "${i}" ]; then
		echo "W: ${i} is a directory instead of file." >&2
	elif [ -e "${i}" ]; then
		. "${i}"
	fi
done

# Finish loading conf snippets
set +a

# Check busybox dependency
if [ "${BUSYBOX}" = "y" ] && [ -z "${BUSYBOXDIR}" ]; then
	echo >&2 "E: busybox-initramfs, version 1:1.30.1-4ubuntu5~ or later, is required but not installed"
	exit 1
fi

if [ -n "${UMASK:-}" ]; then
	umask "${UMASK}"
fi

if [ -z "${outfile}" ]; then
	usage_error
fi

touch "$outfile"
outfile="$(readlink -f "$outfile")"

# And by "version" we really mean path to kernel modules
# This is braindead, and exists to preserve the interface with mkinitrd
if [ ${#} -ne 1 ]; then
	version="$(uname -r)"
else
	version="${1}"
fi

case "${version}" in
/lib/modules/*/[!/]*)
	;;
/lib/modules/[!/]*)
	version="${version#/lib/modules/}"
	version="${version%%/*}"
	;;
esac

case "${version}" in
*/*)
	echo "$PROG: ${version} is not a valid kernel version" >&2
	exit 2
	;;
esac

if [ -z "${compress:-}" ]; then
	compress=${COMPRESS?}
fi
unset COMPRESS

if ! command -v "${compress}" >/dev/null 2>&1; then
	compress=gzip
	[ "${verbose}" = y ] && \
		echo "No ${compress} in ${PATH}, using gzip"
fi

case "${compress}" in
gzip)	# If we're doing a reproducible build, use gzip -n
	if [ -n "${SOURCE_DATE_EPOCH}" ]; then
		compress="gzip -n"
	# Otherwise, substitute pigz if it's available
	elif command -v pigz >/dev/null; then
		compress=pigz
	fi
	;;
lz4)	compress="lz4 -9 -l" ;;
xz)	compress="xz --check=crc32"
	# If we're not doing a reproducible build, enable multithreading
	test -z "${SOURCE_DATE_EPOCH}" && compress="$compress --threads=0"
	;;
bzip2|lzma|lzop)
	# no parameters needed
	;;
*)	echo "W: Unknown compression command ${compress}" >&2 ;;
esac

if [ -d "${outfile}" ]; then
	echo "${outfile} is a directory" >&2
	exit 1
fi

MODULESDIR="/lib/modules/${version}"

if [ ! -e "${MODULESDIR}" ]; then
	echo "W: missing ${MODULESDIR}" >&2
	echo "W: Ensure all necessary drivers are built into the linux image!" >&2
fi
if [ ! -e "${MODULESDIR}/modules.dep" ]; then
	depmod "${version}"
fi

# Prepare to clean up temporary files on exit
DESTDIR=
__TMPCPIOGZ=
__TMPEARLYCPIO=
clean_on_exit() {
	if [ "${keep}" = "y" ]; then
		echo "Working files in ${DESTDIR:-<not yet created>}, early initramfs in ${__TMPEARLYCPIO:-<not yet created>} and overlay in ${__TMPCPIOGZ:-<not yet created>}"
	else
		for path in "${DESTDIR}" "${__TMPCPIOGZ}" "${__TMPEARLYCPIO}"; do
			test -z "${path}" || rm -rf "${path}"
		done
	fi
}
trap clean_on_exit EXIT
trap "exit 1" INT TERM	# makes the EXIT trap effective even when killed

# Create temporary directory and files for initramfs contents
[ -n "${TMPDIR}" ] && [ ! -w "${TMPDIR}" ] && unset TMPDIR
DESTDIR="$(mktemp -d "${TMPDIR:-/var/tmp}/mkinitramfs_XXXXXX")" || exit 1
chmod 755 "${DESTDIR}"
__TMPCPIOGZ="$(mktemp "${TMPDIR:-/var/tmp}/mkinitramfs-OL_XXXXXX")" || exit 1
__TMPEARLYCPIO="$(mktemp "${TMPDIR:-/var/tmp}/mkinitramfs-FW_XXXXXX")" || exit 1

DPKG_ARCH=$(dpkg --print-architecture)

# Export environment for hook scripts.
#
export MODULESDIR
export version
export CONFDIR
export DESTDIR
export DPKG_ARCH
export verbose
export MODULES
export BUSYBOX
export COMPCACHE_SIZE
export RESUME

# Private, used by 'catenate_cpiogz'.
export __TMPCPIOGZ

# Private, used by 'prepend_earlyinitramfs'.
export __TMPEARLYCPIO

# Create usr-merged filesystem layout, to avoid duplicates if the host
# filesystem is usr-merged.
for d in /bin /lib* /sbin; do
	mkdir -p "${DESTDIR}/usr${d}"
	ln -s "usr${d}" "${DESTDIR}${d}"
done
for d in conf/conf.d etc run scripts ${MODULESDIR}; do
	mkdir -p "${DESTDIR}/${d}"
done

# Copy in modules.builtin and modules.order (not generated by depmod)
# and modules.builtin.bin (generated by depmod, but too late to avoid
# error messages as in #948257)
for x in modules.builtin modules.builtin.bin modules.order; do
	if [ -f "${MODULESDIR}/${x}" ]; then
		cp -p "${MODULESDIR}/${x}" "${DESTDIR}${MODULESDIR}/${x}"
	fi
done

# MODULES=list case.  Always honour.
for x in "${CONFDIR}/modules" /usr/share/initramfs-tools/modules.d/*; do
	if [ -f "${x}" ]; then
		add_modules_from_file "${x}"
	fi
done

# MODULES=most is default
case "${MODULES}" in
dep)
	dep_add_modules
	;;
most)
	auto_add_modules
	;;
netboot)
	auto_add_modules base
	auto_add_modules net
	;;
list)
	# nothing to add
	;;
*)
	echo "W: mkinitramfs: unsupported MODULES setting: ${MODULES}." >&2
	echo "W: mkinitramfs: Falling back to MODULES=most." >&2
	auto_add_modules
	;;
esac

# Resolve hidden dependencies
hidden_dep_add_modules

# First file executed by linux
cp -p /usr/share/initramfs-tools/init "${DESTDIR}/init"

# add existant boot scripts
for b in $(cd /usr/share/initramfs-tools/scripts/ && find . \
	-regextype posix-extended -regex '.*/[[:alnum:]\._-]+$' -type f); do
	option=$(sed '/^OPTION=/!d;$d;s/^OPTION=//;s/[[:space:]]*$//' "/usr/share/initramfs-tools/scripts/${b}")
	# shellcheck disable=SC1083,SC2086
	[ -z "$option" ] || eval test -n \"\${$option}\" -a \"\${$option}\" != \"n\" || continue

	[ -d "${DESTDIR}/scripts/$(dirname "${b}")" ] \
		|| mkdir -p "${DESTDIR}/scripts/$(dirname "${b}")"
	cp -p "/usr/share/initramfs-tools/scripts/${b}" \
		"${DESTDIR}/scripts/$(dirname "${b}")/"
done
# Prune dot-files/directories and limit depth to exclude VCS files
for b in $(cd "${CONFDIR}/scripts" && find . -maxdepth 2 -name '.?*' -prune -o \
	-regextype posix-extended -regex '.*/[[:alnum:]\._-]+$' -type f -print); do
	option=$(sed '/^OPTION=/!d;$d;s/^OPTION=//;s/[[:space:]]*$//' "${CONFDIR}/scripts/${b}")
	# shellcheck disable=SC1083,SC2086
	[ -z "$option" ] || eval test -n \"\${$option}\" -a \"\${$option}\" != \"n\" || continue
	[ -d "${DESTDIR}/scripts/$(dirname "${b}")" ] \
		|| mkdir -p "${DESTDIR}/scripts/$(dirname "${b}")"
	cp -p "${CONFDIR}/scripts/${b}" "${DESTDIR}/scripts/$(dirname "${b}")/"
done

echo "DPKG_ARCH=${DPKG_ARCH}" > "${DESTDIR}/conf/arch.conf"
cp -p "${CONFDIR}/initramfs.conf" "${DESTDIR}/conf"
for i in ${EXTRA_CONF}; do
	copy_file config "${i}" /conf/conf.d
done

# ROOT hardcoding
if [ -n "${ROOT:-}" ]; then
	echo "ROOT=${ROOT}" > "${DESTDIR}/conf/conf.d/root"
fi

if ! command -v ldd >/dev/null 2>&1 ; then
	echo "E: no ldd around - install libc-bin" >&2
	exit 1
fi

# fstab and mtab
touch "${DESTDIR}/etc/fstab"
ln -s /proc/mounts "${DESTDIR}/etc/mtab"

# install wait-for-root binary.
copy_exec /usr/lib/initramfs-tools/bin/wait-for-root /sbin

# module-init-tools

copy_exec /sbin/blkid /sbin
copy_exec /sbin/losetup /sbin
copy_exec /sbin/kpartx /sbin
copy_exec /bin/ntfs-3g /bin
copy_exec /sbin/shutdown /shutdown
touch ${DESTDIR}/etc/initrd-release
touch ${DESTDIR}/version


copy_exec /sbin/modprobe /sbin
copy_exec /sbin/rmmod /sbin
mkdir -p "${DESTDIR}/etc/modprobe.d" "${DESTDIR}/lib/modprobe.d"
for file in /etc/modprobe.d/*.conf /lib/modprobe.d/*.conf ; do
	if test -e "$file" || test -L "$file" ; then
		copy_file config "$file"
	fi
done




run_scripts_optional /usr/share/initramfs-tools/hooks
run_scripts_optional "${CONFDIR}"/hooks

# cache boot run order
for b in $(cd "${DESTDIR}/scripts" && find . -mindepth 1 -type d); do
	cache_run_scripts "${DESTDIR}" "/scripts/${b#./}"
done

# generate module deps
depmod -a -b "${DESTDIR}" "${version}"
rm -f "${DESTDIR}/lib/modules/${version}"/modules.*map

# make sure that library search path is up to date
cp -pPr /etc/ld.so.conf* "$DESTDIR"/etc/
if ! ldconfig -r "$DESTDIR" ; then
	[ "$(id -u)" != "0" ] \
	&& echo "ldconfig might need uid=0 (root) for chroot()" >&2
fi
# The auxiliary cache is not reproducible and is always invalid at boot
# (see #845034)
if [ -d "${DESTDIR}"/var/cache/ldconfig ]; then
	rm -f "${DESTDIR}"/var/cache/ldconfig/aux-cache
	rmdir --ignore-fail-on-non-empty "${DESTDIR}"/var/cache/ldconfig
fi

# Apply DSDT to initramfs
if [ -e "${CONFDIR}/DSDT.aml" ]; then
	copy_file DSDT "${CONFDIR}/DSDT.aml"
fi

case "${MODULES}" in
	netboot|most) add_dns "${DESTDIR}/";;
esac

# Remove any looping or broken symbolic links, since they break cpio.
[ "${verbose}" = y ] && xargs_verbose="-t"
(cd "${DESTDIR}" && find . -type l -printf '%p %Y\n' | sed -n 's/ [LN]$//p' \
	| xargs ${xargs_verbose:-} -rL1 rm -f)

[ "${verbose}" = y ] && echo "Building cpio ${outfile} initramfs"

if [ -s "${__TMPEARLYCPIO}" ]; then
	cat "${__TMPEARLYCPIO}" >"${outfile}" || exit 1
else
	# truncate
	true > "${outfile}"
fi

(
# preserve permissions if root builds the image, see #633582
[ "$(id -ru)" != 0 ] && cpio_owner_root="-R 0:0"

# if SOURCE_DATE_EPOCH is set, try and create a reproducible image
if [ -n "${SOURCE_DATE_EPOCH}" ]; then
	# ensure that no timestamps are newer than $SOURCE_DATE_EPOCH
	find "${DESTDIR}" -newermt "@${SOURCE_DATE_EPOCH}" -print0 | \
		xargs -0r touch --no-dereference --date="@${SOURCE_DATE_EPOCH}"

	# --reproducible requires cpio >= 2.12
	cpio_reproducible="--reproducible"
fi

# work around lack of "set -o pipefail" for the following pipe:
# cd "${DESTDIR}" && find . | LC_ALL=C sort | cpio --quiet $cpio_owner_root $cpio_reproducible -o -H newc | gzip >>"${outfile}" || exit 1
ec1=1
ec2=1
ec3=1
exec 3>&1
eval "$(
	# http://cfaj.freeshell.org/shell/cus-faq-2.html
	exec 4>&1 >&3 3>&-
	cd  "${DESTDIR}"
	{
		find . 4>&-; echo "ec1=$?;" >&4
	} | {
		LC_ALL=C sort
	} | {
		# shellcheck disable=SC2086
		cpio --quiet $cpio_owner_root $cpio_reproducible -o -H newc 4>&-; echo "ec2=$?;" >&4
	} | ${compress} >>"${outfile}"
	echo "ec3=$?;" >&4
)"
if [ "$ec1" -ne 0 ]; then
	echo "E: mkinitramfs failure find $ec1 cpio $ec2 $compress $ec3" >&2
	exit "$ec1"
fi
if [ "$ec2" -ne 0 ]; then
	echo "E: mkinitramfs failure cpio $ec2 $compress $ec3" >&2
	exit "$ec2"
fi
if [ "$ec3" -ne 0 ]; then
	echo "E: mkinitramfs failure $compress $ec3" >&2
	exit "$ec3"
fi
) || exit 1

if [ -s "${__TMPCPIOGZ}" ]; then
	cat "${__TMPCPIOGZ}" >>"${outfile}" || exit 1
fi

exit 0

```



#### 3.4  修改modules文件，删除文件内所有内容，粘贴上下面内容

```
sudo cp /etc/initramfs-tools/modules ~/modules.backup
sudo gedit  /etc/initramfs-tools/modules
```

```
# List of modules that you want to include in your initramfs.
# They will be loaded at boot time in the order below.
#
# Syntax:  module_name [args ...]
#
# You must run update-initramfs(8) to effect this change.
#
# Examples:
#
# raid1
# sd_mod
loop
fuse
dm-mod
overlay
nbd

```



#### 3.5  修改ntfs_3g文件，删除文件内所有内容，粘贴上下面内容

```
sudo cp /usr/share/initramfs-tools/scripts/local-bottom/ntfs_3g  ~/ntfs_3g.backup
sudo gedit /usr/share/initramfs-tools/scripts/local-bottom/ntfs_3g 
```

```
#!/bin/sh

##set -e
##case "${1}" in
##	prereqs)
##		exit 0
##		;;
##esac

if [ "${ROOTFSTYPE}" = ntfs ] || [ "${ROOTFSTYPE}" = ntfs-3g ] || \
   [ "${LOOPFSTYPE}" = ntfs ] || [ "${LOOPFSTYPE}" = ntfs-3g ] || [ -n "$KLOOP" ] || [ -n "$SQUASHFS" ] || [ -n "QEMUNBD" ]
then
	mkdir -p /run/sendsigs.omit.d
	pidof mount.ntfs >> /run/sendsigs.omit.d/ntfs-3g
	pidof mount.ntfs-3g >> /run/sendsigs.omit.d/ntfs-3g
	pidof qemu-nbd >> /run/sendsigs.omit.d/qemu-nbd
fi
#####################################################################
##the following maybe help to resolve the buffer I/O error problem 
##when reboot or halt.
#####################################################################

if [ -d /run/initramfs -a -f /init ]
then
	mkdir -p /run/initramfs/dev /run/initramfs/host /run/initramfs/proc /run/initramfs/root /run/initramfs/run /run/initramfs/sys /run/initramfs/tmp
	rm -rf   /lib/modules
	for xxx in /*
  	do	
	if [ ${xxx} = "/dev" -o ${xxx} = "/host" -o ${xxx} = "/proc" -o ${xxx} = "/root" -o ${xxx} = "/run" -o ${xxx} = "/sys" -o ${xxx} = "/tmp" ];
	then
		:
	else
		cp -a ${xxx} /run/initramfs/  1>/dev/null 2>&1;
	fi
	done
	unset xxx
fi
####################################################################
exit 0



```

### 4. 生成initrd.img文件

```
sudo /usr/sbin/mkinitramfs -o ~/initrd.img-XXXXXXXXX-generic 


sudo /usr/sbin/mkinitramfs -o ~/initrd.img-5.4.0-42-generic 

XXXXXXXXX 为版本号 与/boot 目录下的  vmlinuz-XXXXXXXXX-generic  对应

```



### 5. 打包生成的文件

==注意这里要生成当前系统正在运行的内核所对应的版本的文件，一般都是/boot目录下最新的内核，然后把生成文件和最新内核==

==内核可能正在运行，在root模式下复制然后修改权限后就可以移到真机==





把这里生成的文件 initrd.img--XXXXXXXXX-generic 与 /boot/下 的文件 vmlinuz-XXXXXXXXX-generic
和我们系统存放的vhd文件 一共三个文件拷贝或移动到一个NTFS分区根目录下一个名叫ubuntu子目录中

### 6. 配置ubuntu16  vhd启动项 真机启动

```
grub2菜单引导模板，根据自己实际情况进行修改，grub2引导程序安装请查看 grub2启动引导vhd中的ubuntu.md 文件

menuentry ' UBUNTU-14041.vhd '   {
	insmod gzio
	insmod part_msdos
	insmod part_gpt
	insmod ext2
	insmod ntfs
	insmod probe
	insmod search
	search --no-floppy -f --set=aabbcc /ubuntu/UBUNTU-14041.vhd
	set root=${aabbcc}
	probe -u --set=ddeeff ${aabbcc}
	linux	/ubuntu/vmlinuz-3.16.0-30-generic root=UUID=${ddeeff} kloop=/ubuntu/UBUNTU-14041.vhd kroot=/dev/mapper/loop0p1
	initrd	/ubuntu/initrd.img-3.16.0-30-generic
}
```

### 7. 也有单个vhd文件的启动配置方式

~~~
打开终端输入下列代码，表示用管理员权限打开文件夹
sudo nautilus
把生成的initrd.img-XXXXXXXXX-generic 替换掉/boot 目录下的initrd.img-XXXXXXXXX-generic 文件


此时grub2菜单项设置为
menuentry "test vhd" --class ubuntu {
	insmod gzio
	insmod part_msdos
	insmod part_gpt
	insmod ext2
	insmod ntfs
	insmod probe
	set vhdfile="/ubuntu20gpt/ubuntu20gpt.vhd"
	set root=(hd0,1)
	search --no-floppy -f --set=aabbcc  $vhdfile
	set root=${aabbcc}
	probe -u --set=ddeeff ${aabbcc}
	loopback lp0  $vhdfile
	linux	(lp0,2)/boot/vmlinuz-5.4.0-42-generic  root=UUID=${ddeeff}  kloop=$vhdfile  kroot=/dev/mapper/loop0p2
	initrd	(lp0,2)/boot/initrd.img-5.4.0-42-generic
}

~~~

