# **BPI-R4 - OpenWrt SnapShot Build Script**

1. You can change branches "openwrt-24.10" , "master" , "main" etc...

2. If you want to build with the latest openwrt kernel leave this empty OPENWRT_COMMIT="" empty.

3. Added a new directories to place all patches and files into "openwrt-patches".
     * Inside the direcory you drop in all your patches (openwrt-patches)
	 * Inside the directory there is two files "openwrt-add-patch" and "openwrt-remove"
	 * To add or remove a file or patch just enter the full target path into the file - target/linux/generic/backport-6.6/999-some.patch
	 * The cp -f function works likes this.. "Some-Makefile:package/base-files/Makefile"
	 * The mkdir -p function works like this.. Add the tree with the new dir  "Some-script.sh:files/etc/uci-defaults/new.sh" or "files/etc/uci-defaults/new.sh" in the correct add file.
	 * The script will search each of the files at the start of the build and process all entries applying them to the targets.. (or removing them)

4. You can place any custom .config files in side the "config" directory to use.

5. Added an option that prompt the user during the build process to use the "make menuconfig" to add what ever packages or changes you need.
     * When prompted either enter (yes/no): The default is 'no' or let it time out after 10 seconds and it will continue use the existing .config in the config folder.
	 * If 'yes' enter into the make menuconfig and make the changes you need and save, it will continue the build process with your new .config changes.
	 * A new .config.new file will be saved in the config directory.. To make it the default config to use for your next build, just rename it from .config.new to .config

6. Error Checks - All scripts and patches will be auto chacked with dos2unix and corrected if needed. 

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

Updated 9999-new-tx-power-eeprom-0s.patch

Please note the "9999-original-EEPROM-0s.patch" is just the two old patches combined into one and not recommended as a fix.