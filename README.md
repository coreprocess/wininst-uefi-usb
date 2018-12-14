# Create UEFI USB Disk for Windows Installation

## ... on macOS

```sh
# install required tools
brew install dosfstools wimlib

# find usb drive
diskutil list

# partitioning and formatting of usb drive
# (replace /dev/diskX with disk number of your USB disk)
diskutil umountDisk /dev/diskX
sudo gpt destroy /dev/diskX
sudo gpt create -f /dev/diskX
sudo gpt add -t EBD0A0A2-B9E5-4433-87C0-68B6B72699C7 /dev/diskX # click ignore in case a dialog is shown by macOS
sudo /usr/local/opt/dosfstools/sbin/mkfs.fat -F 32 -I -n WININST /dev/diskXs1
diskutil mountDisk /dev/diskX

# validate results
# (FAT32 partition should be mounted at /Volumes/WININST)
diskutil list
mount

# ...
```
