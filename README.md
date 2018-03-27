## mac on virtualbox

Resources collected to launch a macOS on VirtualBox VM.


## Getting started: 
You need a macOS Installer ISO for High Sierraa which can only be created from an exsiting mac.  
The one from App Store "macOS High Sierra" installer is 19MB which cannot create bootable disks.  
So you need get [macOS High Sierra Patcher](http://dosdude1.com/highsierra/) and download the full installer.  
Now you'll be able to create a bootable ISO from the following.
```bash
hdiutil create -o /tmp/HighSierra.cdr -size 7316m -layout SPUD -fs HFS+J
hdiutil attach /tmp/HighSierra.cdr.dmg -noverify -mountpoint /Volumes/install_build
sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/install_build
mv /tmp/HighSierra.cdr.dmg ~/Desktop/InstallSystem.dmg
hdiutil detach /Volumes/Install\ macOS\ High\ Sierra
hdiutil convert ~/Desktop/InstallSystem.dmg -format UDTO -o ~/Desktop/HighSierra.iso
```

## Setting up the VM
- Clone this repo or you can just download the `macOS.vbox` file.
- Open the virtual machine from the repo, usually double clicking it will open the VirualBox GUI.
-  Set the HighSierra.iso image created in Getting Started as a disk in the VM's optical drive. 
- Create a new virtual hard disk. Make sure that you do not set the new virtual hard drive as an SSD, otherwise the High Sierra installer will format the drive as APFS, which is not yet recognized by VirtualBox's EFI BIOS and you will be unable to boot from the hard drive.
- Start the VM, and wait for the macOS installer to boot.
- Open Disk Utility, from the View menu enable the option to "Show all devices", and erase the VirtualBox hard disk image.
- Quit Disk Utility, and install macOS to the newly initialized hard drive.
- When the installer completes, reboot. The VM will boot from the Optical drive; remove the disk from the virtual drive and reboot the VM.
- With the Installer ISO image not available to boot from, you will be dumped into the EFI Shell.  
Enter the following at the EFI prompt to boot macOS from the virtual hard drive and finish installation: `FS1:"macOS Install Data\Locked Files\Boot Files\boot.efi"`   
Alternatively, you can enter exit at the prompt to go to the EFI BIOS boot screen, and use the Boot from file option to navigate to boot.efi.

## Requirements
- The video memory has been set to 128MB, this is the max allowed by the VirtualBox VM, changing this isn't necessary. 
- The RAM is set to 4GB, depending on your available RAM you may wish to increase or decrease the amount.


## Installing updates
When installing patch updates like from `10.13.0` to `10.13.3` you'll have some trouble.  
First of all make sure to have a snapshot before rebooting, so it won't download the installer again.  
Then you need to boot from EFI shell since VirtualBox EFI doesn't automatically use the intended EFI.  
- Press F12 repeatedly, so you get into the EFI shell. 
- On the EFI shell, enter the following.  
  `FS1:"macOS Install Data\Locked Files\Boot Files\boot.efi"` 


## Setting Resolution  
```
VBoxManage setextradata “macOS” VBoxInternal2/EfiGraphicsResolution 1280x720
```

Resolutions to choose:  
  1280x720 | 1920x1080 | 2560x1440 | 2048x1080 | 3840x2160 | 5120x2880 | 1280x800 | 1280x1024 |1600x900 | 1440x900  

## Refrences:
Adapted to macOS High Sierra (10.13) from :  
[geerlingguy/macos-virtualbox-vm](https://github.com/geerlingguy/macos-virtualbox-vm) licensed under MIT. See [LICENSE_MIT](MIT)  
[philipnewcomer/macOS-VirtualBox-VM](https://github.com/philipnewcomer/macOS-VirtualBox-VM) licensed under GPL. See [LICENSE_GPL](MIT)  
And various other sources

## License
Dual licensed under MIT/GPL-3.0.