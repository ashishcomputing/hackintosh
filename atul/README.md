# atul

### Hardware
- [Intel(R) Core(TM) i7-4790K CPU (Devil's Canyon)](http://ark.intel.com/products/80807/Intel-Core-i7-4790K-Processor-8M-Cache-up-to-4_40-GHz)
- [Samsung SSD 840 EVO 250GB](http://www.samsung.com/global/business/semiconductor/minisite/SSD/global/html/ssd840evo/overview.html)
- [NVIDIA GeForce GTX 760 2048 MB (Kepler?)](http://www.geforce.com/hardware/desktop-gpus/geforce-gtx-760)
- [Gigabyte Z97X-UD5H-BK](http://www.gigabyte.com/products/product-page.aspx?pid=5378#ov)
  - [ALC1150](http://www.realtek.com.tw/products/productsView.aspx?Langid=1&PFid=28&Level=5&Conn=4&ProdID=328)
  - Marvell 88SE9172 Controller GSATA3 6~7
  - Qualcomm® Atheros Killer E2201 LAN
  - Intel® GbE LAN (Intel 82580 ?, AppleIGB or AppleIntelE1000e ?)

### Required Drivers
https://www.tonymacx86.com/resources/categories/kexts.11/
- [AppleALC.kext](https://github.com/vit9696/AppleALC) with [Lilu.kext](https://github.com/vit9696/Lilu)
- HFSPlus.efi (alternative to VBoxHfs-64.efi): https://github.com/JrCs/CloverGrowerPro/blob/master/Files/HFSPlus/X64/HFSPlus.efi
- FakeSMC.kext (from HWSensors)
- GenericUSBXHCI

### Setup

1. Manully format USB partition at HFS. If you don't do this, the next step will create a read only partition. See: [Building the USB Installer](https://www.reddit.com/r/hackintosh/comments/68p1e2/ramblings_of_a_hackintosher_a_sorta_brief_vanilla/)

Replace `disk#` with the disk number from `diskutil list`. For example, `disk2`.

```
diskutil partitionDisk /dev/disk# GPT JHFS+ "USB" 100%
```

2. Use `createinstallmedia`.
```
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/USB --applicationpath /Applications/Install\ macOS\ Sierra.app
```

3. Install clover on to the USB. See configuration options from [Installing Clover](https://www.reddit.com/r/hackintosh/comments/68p1e2/ramblings_of_a_hackintosher_a_sorta_brief_vanilla/).

4. Replace VBoxHfs-64.efi with HFSPlus.efi on USB.
```
rm EFI/CLOVER/drivers64UEFI/VBoxHfs-64.efi
wget "https://github.com/JrCs/CloverGrowerPro/raw/master/Files/HFSPlus/X64/HFSPlus.efi" -P EFI/CLOVER/drivers64UEFI
```

5. Download FakeSMC.kext from hwsensors.
```
cd ~/Downloads
wget http://www.hwsensors.com/content/01-releases/45-release-1426/HWSensors.6.25.1426.Binaries.dmg
hdiutil attach ~/Downloads/HWSensors.6.25.1426.Binaries.dmg
mv "/Volumes/HWSensors Binaries v6.25.1426/FakeSMC.kext/" 
```

6. Download a bunch of other texts... They are all stored in the kexts folder in this repo.

7. Create / modify config.plist.

...

1. https://github.com/Piker-Alpha/ssdtPRGen.sh
