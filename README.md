# **BPI-R4 - OpenWrt SnapShot Build Script**

This is my test script for compling the latest OpenWRT SnapShot builds which I thought someone else might find usefull so though I'd share it here...

The Sctipt is very similar to all my other auto scripts, except this script you set the branch and it will automaticly find the latest commit on that branch to build.

Its a set and forget script, once set with all your custom settings all you have to do is just run it to get the latest up to date snapshot.

# **Updated SnapShot Build Script**

One of the openwrt forum members (Jimmy_D) asked if I could simplify the way patches and scipts are incorperated into the script.. ?

So here it is, a more simplified version of my original build script, without having to modify anything in the actual script itself.. to add or delete patches etc

1. You can change branches "openwrt-24.10" , "master" , "main" etc...

2. Added the option to build a specific commit if needed..

3. Created a new "openwrt-patches" directory which mirrors the "openwrt" directory tree, any patch or file placed inside will be applied to the same openwrt patch directory, removing the need to edit the script.. 
     * e.g.. "openwrt-patches/target/linux/mediatek/patches-6.6/some.patch" will be applied to the "openwrt/target/linux/mediatek/patches-6.6/some.patch" directory.
     * If you want to remove a patch just remove it from the openwrt-patches directory.. e.g. "openwrt-patches/target/linux/mediatek/patches-6.6/some.patch".
	 * Add any custom wireless, network config files to "openwrt-patches/files/etc/config/wireless" and it will be included in the built image.
	 * Add any custom uci-defaults script into "openwrt-patches/files/etc/uci-defaults/" and it will be built into the image.

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
			- With every new commit files are checnged and patches that work now might not work with next new commit.
			
    - I've left my default patches I use in the repo as examples so delete what you don't want to kept.. before first build
    - If you have the faulty BE14 card with the EEPROM issues.. The two patches are located in "openwrt-patches/package/kernel/mt76/patches/" directory already.
	- If you don't have the faulty BE14 card delete the two patches located in the "openwrt-patches/package/kernel/mt76/patches/" directory.

## **How to Use**

1. **Prerequisites**: Ensure you have a compatible build environment, such as **Ubuntu 24.04 LTS**. You will also need to install `dos2unix`:  
   `sudo apt update`
   
   `sudo apt install build-essential clang flex bison g++ gawk gcc-multilib g++-multilib \\`
   
   `gettext git libncurses5-dev libssl-dev python3-distutils rsync unzip zlib1g-dev \\`
   
   `file wget dos2unix`

2. **Clone repo**:

   `git clone https://github.com/Gilly1970/BPI-R4_Openwrt_Snapshot_Build.git`
   
   `sudo chmod 776 -R Openwrt_Snapshot.sh`

3. **Run the Script**:  
   * Make the script executable:  
     `chmod \+x Openwrt_Snapshot.sh`
     
   * Execute the script:  
     `./Openwrt_Snapshot.sh`

## **Notes**
Please note - I use this script for testing new snapshots etc... Snapshot builds can be unstable and problematic using them on a main router.
