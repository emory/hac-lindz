# hello 

## lindz

`lindz` is a Sandy Bridge Core i7 2600K that thinks it is a 24" iMac. It's my primary workstation at home until I finish my Haswell workstation, and there have been a few little odds and ends to nail down that I wish to share with others that may find it helpful or useful.

### Hardware

Interesting things about the hardware I'm using.

* GA-Z68X-UD3H-B3 rev 1.1 running UEFI BIOS from Gigabyte. 
* AMD/Sapphire 6870 [^sapphire] 
* AppleYukon-compatible PCI ethernet (1Gbps) (primary interface)
* Marvell on-board ethernet (1Gbps) (secondary interface for storage and virtual machines)
* Various PCI-E USB 3.0 cards of varying stability [^usb3card]
* A USB Bluetooth 4 low-power capable thingy that was a pain in the ass to configure (required Windows) and doesn't enable Handoff
* a PCI Firewire 800/400 card because the Gigabyte board's Firewire 400 was unreliable and there was no Firewire 800 port

#### Sapphire 6870

This has always been a trick. In my clover `config.plist` I have the following, which allows me to use the DVI-D port and DVI port at the same time as expected. The DVI-D port is actually dual-link DVI and driving a 30" Cinema Display at 1280x800 HiDPI. The DVI port is driving a 24" Samsung LED display at 1920x1080. The HDMI port isn't working correctly and I don't know that I'll ever fix it because my new workstation build has an nVidia GTX 760 and I don't expect to have to fight with it. I have no DP hardware to test the DP config.

		<key>ATIConnectorsController</key>
		<string>6000</string>
		<key>ATIConnectorsData</key>
		<string>00040000040300000001000012040401000400000403000000010000220505020008000004020000000100001102030400020000140200000001000000000605</string>
		<key>ATIConnectorsPatch</key>
		<string>00040000040300000001000012040401000800000402000000010000110206040400000004020000000100001102010600020000140200000001000000000305</string>

What I'm doing there is looking for a ConnectorsData portion of the 6000-series AMD Controller kext that matches a string and re-writing the ConnectorsData to my own values. There are some interesting guides and stuff all over the place and it's hard to learn what this sort of thing is all about but if you care you probably found it by now and if you don't care you don't want to read about it here. Use mine and have two working DVI ports if you want.

### Storage

I can go into this more at a future date, but I'm booting from an SSD that contains the OS and applications, and then I have a fusion drive I built with `diskutil` on the CLI that is a 128GB SSD mated to a 1TB magnetic disk. I mount it as `/Users` via `fstab`. I also have a local admin account with a home directory specified on the boot device under `/localusers` for those times I need to offline my Fusion Drive and repair the directory or some nonsense.

I also have a two-disk zpool mirror via OpenZFS on OS X, a 3TB Time Machine disk and a few USB 3.0 and USB 2.0 disks plugged in now and then. I also have a FW800 disk or two that get plopped on sometimes as well. I tend to keep things that want to be really fast on the Fusion Drive and things that I want to be wicked reliable on my zpool. So, for example, my photography work is on my zpool and archived to my network storage and Amazon S3 but I keep the Lightroom catalog files on my Fusion Drive because Time Machine can restore them or I can download them from S3 if they vanish or get destroyed. The Lightroom catalogs are tiny compared to the 4-5 years of photography or home videos I keep locally and essentially I lay out my disks and redundancy based on how much time and effort it takes to recreate them, restore them, or download them again.

I put some of my VMWare disks on the boot SSD and the rest are fine on Fusion Drive or a zvol I carve out of my pool ad hoc. It sounds pretty glamorous but it's just business as usual at Kramerica Industries.

---
[sapphire]: Non-reference card with DVI-D, DVI, DP, and an HDMI port. Will not work with multiple displays unless you alter the AMD6000 controller extension or patch it at boot via Clover. I alter the `Duckweed` personality for mine. I will never buy a Sapphire non-reference card ever again because I really resent having to do this sort of thing.
[usb3card]: The on-board USB 3.0 with my board is not acceptable and causes the Generic USB 3.0 extension to kernel panic in various circumstances mostly involving Calibre or my Leap Motion peripheral. Solution: disable USB 3.0 ports on my Gigabyte board and use a third-party card.
