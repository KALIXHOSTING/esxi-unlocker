Making VM ISO 

Commands you'll need - 
Creating the macOS ISO BigSur

hdiutil create -o /tmp/bigsur -size 15000m -volname bigsur -layout SPUD -fs HFS+J
hdiutil attach /tmp/bigsur.dmg -noverify -mountpoint /Volumes/bigsur
sudo /Applications/Install\ macOS\ BigSur.app/Contents/Resources/createinstallmedia --volume /Volumes/bigsur —nointeraction
hdiutil detach /volumes/Install\ macOS\ BigSur
hdiutil convert /tmp/bigsure.dmg -format UDTO -o ~/Desktop/BigSur.cdr
mv ~/Desktop/Monterey.cdr ~/Desktop/BigSur.iso

===============================================================================================================

Commands you'll need - 
Creating the macOS ISO Monterey

hdiutil create -o /tmp/monterey -size 15000m -volname monterey -layout SPUD -fs HFS+J
hdiutil attach /tmp/monterey.dmg -noverify -mountpoint /Volumes/monterey
sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/monterey —nointeraction
hdiutil detach /volumes/Install\ macOS\ Monterey
hdiutil convert /tmp/monterey.dmg -format UDTO -o ~/Desktop/Monterey.cdr
mv ~/Desktop/Monterey.cdr ~/Desktop/Monterey.iso

===============================================================================================================
Commands you'll need - 
Creating the macOS ISO Ventura

hdiutil create -o /tmp/ventura -size 15000m -volname ventura -layout SPUD -fs HFS+J
hdiutil attach /tmp/ventura.dmg -noverify -mountpoint /Volumes/ventura
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/ventura —nointeraction
hdiutil detach /volumes/Install\ macOS\ Ventura
hdiutil convert /tmp/Ventura.dmg -format UDTO -o ~/Desktop/Ventura.cdr
mv ~/Desktop/Ventura.cdr ~/Desktop/Ventura.iso

===============================================================================================================

Commands you'll need - 
Creating the macOS ISO Sonoma

hdiutil create -o /tmp/sonoma -size 15000m -volname sonoma -layout SPUD -fs HFS+J
hdiutil attach /tmp/sonoma.dmg -noverify -mountpoint /Volumes/sonoma
sudo /Applications/Install\ macOS\ Sonoma.app/Contents/Resources/createinstallmedia --volume /Volumes/sonoma —nointeraction
hdiutil detach /volumes/Install\ macOS\ Sonoma
hdiutil convert /tmp/Sonoma.dmg -format UDTO -o ~/Desktop/Sonoma.cdr
mv ~/Desktop/Ventura.cdr ~/Desktop/Sonoma.iso

===============================================================================================================

DataStore In ESXI
cd /hdd/iso
===============================================================================================================
upload to iso dir in Datastore

Unlocker and Patching esxi
https://github.com/erickdimalanta/#es...#esxi-unlocker-master.zip
- unzip esxi-unlocker-master.zip
- chmod 775 -R esxi-unlocker-301/
- cd esxi-unlocker-301/
- ./esxi-install.sh
- ./esxi-smctest.sh

===============================================================================================================



macOS Unlocker V3.0.2 for VMware ESXi
=====================================

1. Introduction
---------------

Unlocker 3 for ESXi is designed for VMware ESXi 6.5, 6.7 and 7.0

The patch code carries out the following modifications dependent on the product
being patched:

* Fix vmware-vmx to allow macOS to boot
* Fix libvmkctl to allow vSphere to control the guest

The code is written in Python as it makes the Unlocker easier to run and
maintain on ESXi.

+-----------------------------------------------------------------------------+
| IMPORTANT:                                                                  |
| ==========                                                                  |
|                                                                             |
| Always uninstall the previous version of the Unlocker before using a new    |
| version. Failure to do this could render VMware unusable.                   |
|                                                                             |
+-----------------------------------------------------------------------------+

2. Installation
---------------
Copy the distribution file to the ESXi host datastore using scp or some other
data transfer system. If you want to use the source version (i.e. from GIT) see
"5. Building" fist.

Decompress the file from the ESXi console or via SSH:

    tar xzvf esxi-unlocker-xxx.tgz

(xxx - will be the version number, for example, 300)

Run the command from the terminal:

    ./esxi-install.sh

Finally reboot the server.

3. Uninstallation
-----------------
Open the ESXi console or login via SSH and change to the folder where the files were extracted.

Run the command from the terminal:

    ./esxi-uninstall.sh

Finally reboot the server.

4. Notes
--------
A. There is a command added called esxi-smctest.sh which can show if the patch is successful. It must be run from a
terminal or SSH session. The output should be:

/bin/vmx
smcPresent = true
custom.vgz     false   32486592 B

Note: The uncompressed size reported for custom.vgz will vary depending on the ESXi version.

B. The unlocker can be temporarily disabled during boot by editing the boot options and adding "nounlocker".

5. Building
-----------
If you want to use a version which is not availbale as a distribution (e.g. the code from "master" branch)
you need to first build the package.

Checkout the repository:

    git clone https://github.com/shanyungyang/esxi-unlocker.git

(if you don't have git installed you can download ZIP archive from GitHub instead)

Enter the directory and build:
    
    cd esxi-unlocker
    ./esxi-build.py

If everything went correctly the ouput should be:

    ESXi-Build for macOS

    Timestamping files...

    Creating unlocker.tgz...
    etc/
    etc/rc.local.d/
    etc/rc.local.d/unlocker.py

    Creating esxi-unlocker-301.tgz...
    unlocker.tgz
    esxi-install.sh
    esxi-uninstall.sh
    esxi-smctest.sh
    readme.txt

The package you need to copy in the example above is esxi-unlocker-301.tgz (NOT unlocker.tgz!).

6. Thanks
---------

Thanks to Zenith432 for originally building the C++ unlocker and Mac Son of Knife
(MSoK) for all the testing and support.

Thanks also to Sam B for finding the solution for ESXi 6 and helping me with
debugging expertise. Sam also wrote the code for patching ESXi ELF files and
modified the unlocker code to run on Python 3 in the ESXi 6.5 environment.

History
-------
26/09/18 3.0.0 - First release
01/05/20 3.0.1 - Fix for ESXi 7.0
10/18/20 3.0.1 - Fix for ESXi 7.0 U1 (7.0.1)

(c) 2011-2018 Dave Parsons
