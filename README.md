## Installing Openwall2 on a GL-iNet MT3000 Router with Fork version of OpenWrt
___

> [!NOTE]
> As of writing of this guide, GL-iNet newest fork for OpenWrt is 24.10 and have not release a fork of 25.12. So we will be using the older **opkg** package installer instead of the newer **apk** installer.
> 25.12 has an issue with it's public key system that treat all 3rd party repos' package as untrusted even after public key is saved to the OpenWrt's key folder. I will write a 25.12 guide once the public key system has been fixed.

> [!WARNING]
> I'm not responsible if this guide brick your router. If it does happen, you can try using [this guide](https://docs.gl-inet.com/router/en/3/tutorials/debrick/) to restore the firmware for your GL-iNet router.

> [!IMPORTANT]
> I setup this up on a **GL-iNet MT3000(Beryl AX) Travel WiFi router**. But this guide should also work for GL-iNet MT2500A/MT6000 or any OpenWrt router with at least 256MB of flash storage. After the install, my MT3000 shows it has about 40% of memory left, so routers with 128MB will not have enough memory.

### Prerequisites
___
1. Download the latest version of GL-iNet's fork of OpenWrt firmware at https://dl.gl-inet.com/router/mt3000/openwrt24.
2. Log into the GL-Inet's Web Admin Panel at http://192.168.8.1
3. From the `System` menu, select `Upgrade`, and upload downloaded firmware from `Firmware Local Upgrade` menu.
4. Once finish installing the OpenWrt firmware and after the router will reboot, and log back into router.
5. Got to the `Advanced Settings` menu and select `Install Now` to install OpenWrt's LuCI Web Admin.

### Installation
___
Step 1. Ssh into your router at 192.168.8.1 as _root_ user with **PuTTY** for Windows or using the built in ssh client in Windows Command Prompt/MacOS Terminal.
Step 2. Run update for OpenWrt packages.

```bash
opkg update
```
> [!NOTE]
>If you haven't install LuCI from the Admin Panel webpage, use the following command to install it.
```bash
opkg install luci-app-opkg
```

Step 3. Remove the default dnsmasq and install dnsmasq-full package.

```bash
opkg remove dnsmasq
opkg install dnsmasq-full
```

Step 4. Install required kernel modules packages. *Might be already installed, but just case for routers that didn't.*

```bash
opkg install kmod-nft-tproxy kmod-nft-socket
```

Step 5. Add the public key from Sourceforge repo for the need Passwall packages.

```bash
wget https://master.dl.sourceforge.net/project/openwrt-passwall-build/ipk.pub -O passwall.pub
opkg-key add passwall.pub
```

Step 6. Create a custom feed for the Passwall packages from Sourceforge repo

```bash
read release arch << EOF
$(. /etc/openwrt_release ; echo ${DISTRIB_RELEASE%.*} $DISTRIB_ARCH)
EOF

for feed in passwall_packages passwall2; do
echo "src/gz $feed https://master.dl.sourceforge.net/project/openwrt-passwall-build/releases/packages-$release/$arch/$feed" >> /etc/opkg/customfeeds.conf
done
```

Step 7. Run opkg update again for Passwall2 repo

```bash
opkg update
```

Step 8. Install Passwall2 and couple of other proxy protocols

```bash
opkg install luci-app-passwall2 sing-box v2ray-plugin hysteria naiveproxy chinadns-ng dns2socks
```

Step 9. Reboot router for all Passwall2 to be fully working.

```bash
reboot
```

> [!NOTE]
>You should now see `Passwall 2` sub-menu under the `Services' main menu.

##Optional: Install Argo Theme for LuCI web admin

I personally think Passwall2 looks better in Argon theme than the default bootstrap theme. Use the following command lines to install it.

```bash
opkg install luci-compat
opkg install luci-lib-ipkg
wget --no-check-certificate -O luci-theme-argon.ipk https://github.com/jerrykuku/luci-theme-argon/releases/download/v2.3.2/luci-theme-argon_2.3.2-r20250207_all.ipk
opkg install luci-theme-argone.ipk
```



