# Setup

To start attacking the esp32 chip we should see what is in the firmware. To do this we will use esptool to dump.

## Install Esptool

```python
pip install esptool
```

## Identify ESP32 Com Port

Finding the com port can be useful as not all tools can automatically find what you are looking for. It depends on your platform the steps you need to take select from the drop down your os or the ardunio software.

<tabs>
    <tab title="Windows">
        <procedure title="Windows Find Com Port" id="Windows-Find-Com-Port">
            <step>
                <p>Press Windows Key + X</p>
            </step>
            <step>
                <p>Press <shortcut>Device Manger</shortcut> to launch device manger.</p>
                <img src="DeviceMangerLaunch.png" alt="Picture of launching device manger" border-effect="line"/>
            </step>
            <step>
                <p>Scroll down to <shortcut>Ports (COM & LPT)</shortcut> and expand the menu</p>
            </step>
            <step>
                <p>Plug your badge into your computer and turn the badge on.</p>
            </step>
            <step>
                <p>The device tree will refresh and you should see a new device under <shortcut>Ports (COM & LPT)</shortcut>. This will be the communication port used to talk to the device.</p>
                <img src="WINCOMPORT.png" alt="Picture highlighting the new com port that appeared" border-effect="line"/>
            </step>
        </procedure>
    </tab>
    <tab title="Linux">
        <p>This command will list out all active serial devices</p>
        <code-block lang="bash">ls /dev/tty*</code-block>
        <p>You may need to try a couple to see which one works or you can run this following command to see what each one is</p>
        <code-block lang="bash">for dev in /dev/ttyUSB*; do sudo udevadm info -q all -n "$dev" | grep -E 'DEVNAME|ID_MODEL'; done</code-block>
    </tab>
    <tab title="Mac">
       <p>This command will list out all active serial devices</p>
        <code-block lang="bash">ls /dev/cu.*</code-block>
        <p>You may need to try a couple to see which one works or you can run this following command to see what each one is</p>
        <code-block lang="bash">ls /dev/cu.* | grep -i usb | while read dev; do ioreg -p IOUSB -l -w 0 | grep -A 10 "$dev"; done</code-block>
    </tab>
    <tab title="Ardunio IDE">
        <procedure>
            <step>
                <p href="https://www.arduino.cc/en/software">Install the Ardunio IDE</p>
            </step>
            <step>
                <p></p>
                <img src="ardunioportfinder.png" alt="ardunio showing" border-effect="line"/>
            </step>
        </procedure>
    </tab>
</tabs>

## Dumping Flash

### Understanding the Chip

To dump the flash we will need to know two things the port and the size of flash. To find size of flash we can run the following command

```bash
python -m esptool --port {COMPORT} flash_id
```

Sample Output:
```
C:\Users\NSSte>python -m esptool --port COM16 flash_id
esptool.py v4.8.1
Serial port COM16
Connecting...
Detecting chip type... ESP32-S3
Chip is ESP32-S3 (QFN56) (revision v0.2)
Features: WiFi, BLE
Crystal is 40MHz
MAC: 24:58:7c:f4:8e:30
Uploading stub...
Running stub...
Stub running...
Manufacturer: c8
Device: 4017
Detected flash size: 8MB
Flash type set in eFuse: quad (4 data lines)
Flash voltage set by a strapping pin to 3.3V
Hard resetting via RTS pin...
```

We see we have 8MB of flash, and we also have the MAC address in case we lose the display

More commands can be found in the [docs](https://docs.espressif.com/projects/esptool/en/latest/esp32s3/esptool/basic-commands.html#read-flash-contents-read-flash)

## esptool read_flash

Using another command from the [docs](https://docs.espressif.com/projects/esptool/en/latest/esp32s3/esptool/basic-commands.html#read-flash-contents-read-flash) called read flash we will now dump out the flash contents

```bash
python -m esptool -p {COMPORT} -b 460800 read_flash 0 ALL flash_contents.bin
```

We now should have an 8mb flash file
