# asus-wmi-screenpad
This repository contains a version of the asus-wmi module with support for brightness control on secondary screens ("ScreenPad Plus") on ASUS Zenbook Duo notebooks. It also adds a few keys from the Zenbook Duo keyboard (Camera, /A, screen switch, toggle ScreenPad) so they can be mapped in your desktop environment.

To install and use this module using dkms:

1. Install DKMS using the method of your distribution.
   Debian: `sudo apt install dkms`
   
2. Create a directory for the module and download the source code
   ```
   sudo mkdir /usr/src/asus-wmi-1.0
   cd /usr/src/asus-wmi-1.0
   sudo wget 'https://github.com/Plippo/asus-wmi-screenpad/archive/5.3.zip'
   sudo unzip 5.3.zip
   sudo mv asus-wmi-screenpad-5.3/* .
   sudo rmdir asus-wmi-screenpad-5.3
   sudo rm master.zip
   ```
   Now the source code should be in `/usr/src/asus-wmi-1.0`. It's important that the folder is called exactly like that because DKMS expects that.
   Alternatively you can of course also clone this git repository into that folder.

3. Register the module with DKMS
   ```
   sudo dkms add -m asus-wmi -v 1.0
   ```

4. Build and install the module to the current kernel
   ```
   sudo dkms build -m asus-wmi -v 1.0
   sudo dkms install -m asus-wmi -v 1.0
   ```
   From now on, DKMS will automatically rebuild the module on every kernel update.

5. After rebooting, you should now find a new device in `/sys/class/leds/asus::screenpad`.
   To set the brightness of the screen, simply call
   ```
   echo XXX | sudo tee '/sys/class/leds/asus::screenpad/brightness'
   ```
   where XXX is a value between 0 and 255 (0 turns the screen completely off, 255 sets it to maximum brightness.
   To allow every user to set the brightness without using sudo, call
   ```
   sudo chmod a+w '/sys/class/leds/asus::screenpad/brightness'
   ```
   Now you can set the brightness by simply executing
   ```
   echo XXX > '/sys/class/leds/asus::screenpad/brightness'
   ```
   `chmod` has to be executed again after every reboot, so it is advisable to add the call to a boot script, e.g. `/etc/rc.local`.
   
6. You can now also use the functionality of your Desktop Environment to map the function keys on the keyboard to actions of your choice. For example, you can create a script that toggles the state of the screenpad and map it to the "Toggle ScreenPad" key.
