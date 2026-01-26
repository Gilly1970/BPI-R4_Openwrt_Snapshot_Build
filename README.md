# **BPI-R4 - OpenWrt SnapShot Build Script**

1. You can change branches "openwrt-24.10" , "master" , "main" etc...

2. If you want to build with the latest openwrt kernel leave this empty OPENWRT_COMMIT="" empty.

3. Added a new directories to place all patches and files into "openwrt-patches".
     * Inside the direcory you drop in all your patches (openwrt-patches)
	 * Inside the directory there is two files "openwrt-add-patch" and "openwrt-remove"
	 * To add or remove a file or patch just enter the full target path into the file - target/linux/generic/backport-6.6/999-some.patch
	 * The cp -f function works likes this.. "Some-Makefile:package/base-files/Makefile"

4. You can place any custom .config files in side the "config" directory to use.

5. Added an option that prompt the user during the build process to use the "make menuconfig" to add what ever packages or changes you need.
     * When prompted either enter (yes/no): The default is 'no' or let it time out after 10 seconds and it will continue use the existing .config in the config folder.
	 * If 'yes' enter into the make menuconfig and make the changes you need and save, it will continue the build process with your new .config changes.
	 * A new .config.new file will be saved in the config directory.. To make it the default config to use for your next build, just rename it from .config.new to .config

6. Error Checks - All scripts and patches will be auto checked with dos2unix and corrected if needed. 

7. Permissions - All scripts, patches and folders used will have the correct permissins applied during the build process.

## **Please Note- .gitkeep files and patches**

Please note - No directory with sub folders on github can be empty, so the use of blank ".gitkeep" files are used to create empty sub directories..
            - All .gitkeep files cloned will be deleted on the first excution of the script.. Or just delete them manually if you wish.
			- When using patches. If you have a build fail, read the fail messages and remove the patch causing the error.. 
			- With every new commit files are changed and patches that work now might not work with next new commit.

## **How to Use**

1. **Prerequisites**: Ensure you have a compatible build environment, such as **Ubuntu 24.04 LTS**. You will also need to install `dos2unix`:  
   `sudo apt update`
   
   `sudo apt install build-essential clang flex bison g++ gawk gcc-multilib g++-multilib gettext git libncurses5-dev libssl-dev python3-distutils rsync unzip zlib1g-dev file wget dos2unix`

2. **Clone repo**:

   `git clone https://github.com/Gilly1970/BPI-R4_Openwrt_Snapshot_Build.git`
   
   `sudo chmod 775 -R BPI-R4_Openwrt_Snapshot_Build`

3. **Run the Script**:  
   * Make the script executable:  
     `chmod +x Openwrt_Snapshot.sh`
     
   * Execute the script:  
     `./Openwrt_Snapshot.sh`

## **Notes**

- 26.01.2026 - Added updated eeprom containing zeros patch ```9999-mt76-eeprom-linked-fix.patch```. 
  - 2GHz and 5GHz: Check for missing (0x00) or uninitialized (0xFF) data. 
  - If 2G/5G are valid, we assume this is a "Type A" card that is handled correctly by the standard driver logic. 
  - If 2G/5G are invalid, we assume this is a "Type B" card with the corrupt eeprom which is the target of this patch.
  
  ```csharp
  bash
  dmesg | grep mt7
  ```  
  
  - and look for the below message in the log...
  
  ```csharp
  [   11.316363] mt7996e 0000:01:00.0: Corrupted EEPROM detected (Type B). Restoring all bands to safe defaults.
  ```
  - If you don't see this message in the log then you have card type A and the patch hasn't applied.

- 26.01.2026 - Added test patch from https://patchwork.kernel.org/project/linux-wireless/patch/e92213f793f9cb9f509c26205e7ecb2be6ce77aa.1769156922.git.ryder.lee@mediatek.com/

  - Patch notes: The maximum power value may exist in data or backoff field. To respond the correct value of txpower, mt76 should also consider these values in sku table.
  
  - To remove this patch from the build delete this line ```package/kernel/mac80211/patches/build/9999-mt76-fix-backoff-fields.patch``` from the openwrt-add-patch file.