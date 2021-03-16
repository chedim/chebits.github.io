# Flashng QMK on Android

This is just a note that, if you have a rooted android phone with termux then you can use it to flash QMK from your phone:

* Install a linux distribution using termux-proot (I used Kali Linux)
* Use this script (replace `<USER>` with your username) to launch installed distro (please note that it runs as root and uses chroot instead of proot):
```
#!/data/data/com.termux/files/usr/bin/bash -e
unset LD_PRELOAD
# thnx to @j16180339887 for DNS picker
addresolvconf ()
{
  android=$(getprop ro.build.version.release)
  if [ ${android%%.*} -lt 8 ]; then
  [ $(command -v getprop) ] && getprop | sed -n -e 's/^\[net\.dns.\]: \[\(.*\)\]/\1/p' | sed '/^\s*$/d' | sed 's/^/nameserver /' > ${PREFIX}/share/TermuxAlpine/etc/resolv.conf
  fi
}
addresolvconf
sudo proot --link2symlink -0 -r ${PREFIX}/var/lib/proot-distro/installed-rootfs/nethunter -b /dev/ -b /sys/ -b /proc/ -b /sdcard -b /storage -b $HOME -w /home/<USER> /usr/bin/env TMPDIR=/tmp HOME=/home/<USER> PREFIX=/usr SHELL=/bin/sh TERM="$TERM" LANG=$LANG PATH=/bin:/usr/bin:/sbin:/usr/sbin /usr/bin/sudo -u <USER> /bin/bash --login
```

