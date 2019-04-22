# Create UEFI USB Drive for Windows Installation

## ... on macOS

```sh
# Download Windows 10 ISO
# Note: might take a while, you can procede with the partitioning and formatting
open https://www.microsoft.com/en-us/software-download/windows10ISO

# Install required tools
brew install dosfstools wimlib

# Find USB drive device path
diskutil list

# Partitioning and formatting of USB drive
# Note: Replace /dev/diskX with the device path of your USB drive
diskutil umountDisk /dev/diskX
# Note: Destroy operation is required to delete an existing GPT partition table
sudo gpt destroy /dev/diskX
# Note: The -f flag is only required to overwrite an existing MBR partition table
sudo gpt create -f /dev/diskX
# Note: Click ignore in case a dialog is shown by macOS
sudo gpt add -t EBD0A0A2-B9E5-4433-87C0-68B6B72699C7 /dev/diskX
sudo /usr/local/opt/dosfstools/sbin/mkfs.fat -F 32 -I -n WININST /dev/diskXs1
diskutil mountDisk /dev/diskX

# Validate results
# Note: FAT32 partition should be mounted at /Volumes/WININST now
diskutil list
mount

# Mount Windows ISO
# Note: Replace the path to the ISO file
hdiutil attach ~/Downloads/Win10_VERSION_LANG_ARCH.iso -mountpoint /Volumes/WINISO

# Copy all files except install.wim
rsync -v -r -I --no-links --no-perms --no-owner --no-group --exclude sources/install.wim /Volumes/WINISO/ /Volumes/WININST/

# Split-copy install.wim
wimsplit /Volumes/WINISO/sources/install.wim /Volumes/WININST/sources/install.swm 1024

# Ensure all data is written and detach all devices
sync
hdiutil detach /Volumes/WINISO
# Note: Replace /dev/diskX with the device path of your USB drive
diskutil umountDisk /dev/diskX
diskutil eject /dev/diskX

```
