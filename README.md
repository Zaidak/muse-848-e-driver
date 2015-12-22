# muse-e-driver
experimental repo for ubuntu drivers for the bluetooth headset


Reference links: 
To setup the Muse SDK on Linux: 
http://atlantsembedded.com/b/getting-muse-sdk-work-linux#skip-to-install
  $chmod a+x musesdk-3.4.1-linux-installer.run
  $./musesdk-3.4.1-linux-installer.run

Install the necessary required packages (but may not be complete - if so, post in the comments)

sudo apt-get install zlib1g-dev zlib1g-dev:i386 lib32gcc1 libc6-dev-i386 libbluetooth3-dev:i386
libbluetooth3-dev libstdc++6:i386 libstdc++6 liblo-dev:i386 liblo7:i386 icedtea-7-jre-jamvm bluez bluez-utils bluez-tools gtk2-engines-pixbuf:i386 gtk2-engines-murrine:i386 libcanberra-gtk-module:i386 unity-gtk2-module:i386
Next, install liblsl C libraries:

wget ftp://sccn.ucsd.edu/pub/software/LSL/SDK/liblsl-C-C++-1.11.zip 
unzip liblsl-C-C++-1.11.zip -d liblsl-bin
cd liblsl-bin/bin/
sudo cp liblsl32.so  /lib32/liblsl.so
sudo ldconfig
Great- we should be able to install the MUSE package itself now:

rbrash@ackbar:~$ ./musesdk-3.4.1-linux-installer.run
If a graphical window pops up - click through it and it will install everything inside ~/Muse

MUSE-SDK package installer

Next navigate into the Muse directory:

rbrash@ackbar:~$ cd ~/Muse/
Now do you have USB Bluetooth device installed - particularly a CSR one.  Plug it in and run the command: dmesg

rbrash@ackbar:~/Muse$ dmesg
[ 2693.640879] usb 1-2: USB disconnect, device number 2
[ 2694.799486] usb 1-2: new full-speed USB device number 3 using xhci_hcd
[ 2695.120756] usb 1-2: New USB device found, idVendor=0a12, idProduct=0001
[ 2695.120760] usb 1-2: New USB device strings: Mfr=0, Product=2, SerialNumber=0
[ 2695.120761] usb 1-2: Product: CSR8510 A10
If you have output like the above - congrats... onto our next step which is to see what our devices name is and to scan for our MUSE:

rbrash@ackbar:~$ hciconfig
hci0:    Type: BR/EDR  Bus: USB
    BD Address: 00:1A:7D:DA:71:13  ACL MTU: 310:10  SCO MTU: 64:8
    UP RUNNING PSCAN 
    RX bytes:612 acl:0 sco:0 events:37 errors:0
    TX bytes:942 acl:0 sco:0 commands:37 errors:0
Above - you can see that hci0 is the plugged in USB Bluetooth device.  Perform the MUSE reset proceedure as per the manual here , then run the command below:

rbrash@ackbar:~$ hcitool -i hci0 scan
Scanning ...
    00:06:66:6F:4C:FA    Muse-4CFA
I have found my MUSE with a human readable name: Muse-4CFA and a MAC (think hardware address) 00:06:66:6F:4C:FA and this step removes the pairing security functionality so that the headset is easily connectable.

sudo hciconfig hci0 sspmode 1
Now start the MuseLab application

rbrash@ackbar:~$ cd Muse/
rbrash@ackbar:~/Muse$ ./MuseLab
Now open the Port to 5000 TCP (this isn't the most optimal system, but it works for the purpose of this document) - See screenshot, column on left will be blank until the next step.

MUSE Labs in Linux - open port

Next run the muse-io app to communicate to the MuseLab application:

rbrash@ackbar:~/Muse$ ./muse-io --device 00:06:66:6F:4C:FA
See screenshot for success

MUSE-io working: note port
