SoftEther VPN for OpenWrt
=
Your router is if ar71xx, You do not need build steps.  
You can get binary package from http://b.mikomoe.jp/.

If need compile for OpenWrt 12.09, See old [README.MD](https://github.com/el1n/OpenWRT-package-softether/blob/7dc4c4ce19da9aa7dc2330e2dbbdc4d3e4dd4fcc/README.md)

If you are japanese or could read japanese, Visit [my blog](http://elin.mikomoe.jp/index.php?entry=OpenWRT%E3%81%A7SoftEther-VPN%E3%82%92%E5%8B%95%E3%81%8B%E3%81%99).  
This entry too old.  
If possible, Please read this README.MD.

Note
-
+ No more need customized libopenssl for Jan 15, 2015 or later version.

+ Every SoftEther VPN packages did integrated for Feb 10, 2015 or later version. 
Binary has function of vpnserver/vpnclient/vpnbridge/vpncmd like busybox now. 
Uninstall all the SoftEther VPN packages if you will update from old version.

Compile and Install
-
1. Install the packages required to compile

  Example for debian.
  ```
  apt-get install -y subversion make gcc g++ libncurses5-dev libghc-zlib-dev libreadline-dev libssl-dev gawk bzip2 patch xz-utils sudo
  ```

2. Get OpenWrt SDK and prepare for compile
  ```
  svn co svn://svn.openwrt.org/openwrt/branches/barrier_breaker
  cd barrier_breaker
  ```

  Add following line to feeds.conf file.
  If file not exists, Add line after copy feeds.conf.default to feeds.conf.
  ```
  src-git softethervpn https://github.com/el1n/OpenWRT-package-softether.git
  ```

  Update feeds and Install SoftEther VPN.
  ```
  ./scripts/feeds update
  ./scripts/feeds install softethervpn
  ```

3. SDK settings
  ```
  make defconfig
  make menuconfig
  ```
  Change "Target System" for your OpenWrt.  
  Visit "Network/VPN/SoftEther VPN", Check what you need.

4. Build the package
  ```
  make prepare
  make package/softethervpn/compile V=99
  ```
  You can find packages from ./bin directory if compile succeed.

5. Install to OpenWrt

  SoftEther VPN need some packages.
  + zlib
  + libpthread
  + librt
  + libreadline
  + libncurses
  + libiconv-full
  + kmod-tun
  + libopenssl

6. Execute

  Press "Start" from "System -> Startup" in the LuCI.

  If you want run SoftEther VPN in a shell, You need set the LANG environment variable and execute the SoftEther VPN.
  ```
  /usr/bin/env LANG=en_US.UTF-8 /usr/bin/vpnserver start
  ```
