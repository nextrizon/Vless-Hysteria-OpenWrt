## Installing OpenWall2(Vless/Hysteria2) on a GL-iNet MT3000 Router with Fork version of OpenWrt
___

> [!NOTE]
> As of writing of this guide, GL-iNet newest fork for OpenWrt is 24.10 and have not release a fork of 25.12. So we will be using the older **opkg** package installer instead of the newer **apk** installer.

> [!WARNING]
> I'm not responsible if this guide brick your router. If it does happen, you can try using [this guide](https://docs.gl-inet.com/router/en/3/tutorials/debrick/) to restore the firmware for your GL-iNet router.

> [!IMPORTANT]
> I setup this up on a **GL-iNet MT3000(Beryl AX) Travel WiFi router**. But this guide should also work for GL-iNet MT2500A/MT6000 and any OpenWrt router using an ARM Cortex-A53 processor with at least 256MB of flash storage. The router will have use up of total ~220MB of flash storage after setting up the Vless/Hysteria2 client node.

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

Step 4. Install required kernel modules packages.

```bash
opkg install kmod-nft-tproxy kmod-nft-socket
```

Step 5. Add the public key from Sourceforge repo for the need Passwall packages.

```bash
wget https://master.dl.sourceforge.net/project/openwrt-passwall-build/ipk.pub -O passwall.pub
opkg-key add passwall.pub
```

Step 6
