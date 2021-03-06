# How to Flash

There are two parts to flash:

* The LCD resources
* The mainboard (custom Marlin firmware)

It is recommended to flash first the LCD resources.

## Part 1 - Flashing the LCD resources

**Note**: *The LCD resources are hosted on their own GitHub project: [ADVi3pp-LCD](https://github.com/andrivet/ADVi3pp-LCD).*

### LCD Step 1 - Prepare a microSD card

**IMPORTANT**: You have to use a microSD card with a maximum capacity of **8GiB**. If you use a microSD card with a greater capacity, the results are **unreliable** (sometimes it flashes, sometimes not). This is a limitation of the LCD display itself.

You have two possibilities to flash:

#### LCD Step 1 - Option 1 - Manual copy

* Download the LCD resources: [ADVi3pp-LCD-x.x.x.zip](https://github.com/andrivet/ADVi3pp-Marlin/releases)
* Unzip the file somewhere
* Copy manually all the files and folders in the uncompressed zip file to the root of a microSD card. The microSD card **has** to be formatted with the following parameters: FAT32, 4096 bytes per cluster (i.e. 8 sectors).
* To format under Linux (and macOS with the `dosfstools` Homebrew package):

```
mkfs.fat -F 32 -n SD -s 8 -v /dev/disk2
```

Of course, replace `/dev/disk2` with the right value.

* To format under Windows (Command Prompt):

```
format G: /FS:FAT32 /V:LCD /A:4096
```
Of course, replace `G:` with the right volume letter.

#### LCD Step 1 - Option 2 - SD image

* Download the microSD card image: [ADVi3pp-LCD-x.x.x.img.zip](https://github.com/andrivet/ADVi3pp-Marlin/releases)
* Unzip the `.img.zip` file and use either `dd` (Linux, macOS) or [Etcher](https://etcher.io) (Windows, Linux, macOS). For example with `dd`:

```
unzip ADVi3pp-LCD-1.0.0.img.zip
sudo dd if=ADVi3pp-LCD-1.0.0.img of=/dev/disk2 bs=64K
```

Of course, replace `/dev/disk2` with the right value.

If you prefer graphical applications, [Etcher](https://etcher.io) is a great multi-platform tool.

### LCD Step 2 - Install the new version

- Disconnect the printer from power
- Remove the two screws located on the front and loose the two M3 screws on the top

![front-panel-screws](https://user-images.githubusercontent.com/981049/31637200-e3ecc438-b2cd-11e7-888f-aad32bc96676.jpg)

- Remove the front panel carefully (don't break the flat cable)
- If you are lucky, you can insert the microSD card on the left of the panel (this is the case on the Monoprice clone)
- Otherwise, remove the four M3 screws and remove the cover
- Insert the microSD card in the slot

![lcd-board-microsd](https://user-images.githubusercontent.com/981049/31637212-f5511148-b2cd-11e7-958a-9f496205c498.jpg)

- Turn on the printer (either by connecting it to power or by connecting the USB slot to the computer)
- The screen turns blue and then every image will appear one by one
- After around 2 or 3 minutes, no more images appear
- Turn off the printer and remove the microSD card
- Re-assemble the front panel, do not forget the two M3 screws on the top
- Turn the printer on. You know have the new version of the LCD images

### LCD Enclosure

![](https://cdn.thingiverse.com/renders/08/b4/6b/6a/29/3e776fd38ccd98efaf288bf79aeab604_preview_featured.jpg)

From [Thingiverse "Wanhao Duplicator I3 Plus LCD enclosure" by bosbessenbasje](https://www.thingiverse.com/thing:2369322):

>  The standard enclosure puts the LCD in a 90 degrees angle towards the table and that makes it hard to read. This enclosure will put the LCD in 60 degrees angle which improves readability at the cost of a slightly large space on the table.

> Note that the new enclosure allows you to insert a **micro SD in the LCD to upgrade the LCD firmware** if you want to. You will probably need some pliers to do it though.

I highly recommend this LCD enclosure. It will simplify a lot your life.

## Part 2 - Flashing the mainboard firmware

There are several ways to flash the mainboard firmware. The first step is to download the firmware from the **Releases** page on [GitHub](https://github.com/andrivet/ADVi3pp-Marlin/releases):

[ADVi3pp-Mainboard-x.x.x.hex](https://github.com/andrivet/ADVi3pp-Marlin/releases)

Then choose the option you are the most comfortable with.

**Note**: *It is not possible to flash the mainboard using the SD card slot of the printer.*

### Mainboard Option 1 - Flashing using Cura

* if net yet done, download Cura. I recommend either [Cura for Wanhao](http://www.wanhao3dprinter.com/Down/ShowArticle.asp?ArticleID=56) (if you directly connect the printer to your computer with a USB cable) or [Ultimaker Cura 3](https://ultimaker.com/en/products/ultimaker-cura) (if, for example, you are using OctoPrint to print)
* Start **Cura**
* In the top menu, under **Settings** &#8594; **Printer**, select **Manage Printers**
* Select your printer or **Add** your printer if it is not already done
* Select **Upgrade Firmware** and then **Upload custom Firmware**
* Select the downloaded file `ADVi3pp-Mainboard-x.x.x.hex` and click on **Open**

### Mainboard Option 2 - Flashing using OctoPrint

You need `advdude` and the development build of the **Firmware Updater** plugin.

To install `advdude` on a Raspberry Pi:

* Connect to the Raspberry (for example through SSH) and enter the command:

```
sudo apt update; sudo apt install avrdude
```

To install the development build of the **Firmware Updater** plugin

* Open a navigator and connect to **OctoPrint**
* **Login** and click on the wrench icon in the toolbar
* Select **Plugin Manager** &#8594; **Get More...**
* Under **from URL** enter `https://github.com/OctoPrint/OctoPrint-FirmwareUpdater/archive/devel.zip` and click on **Install**
* Restart **OctoPrint**

To flash the firmware:

![firmwareupdater](https://user-images.githubusercontent.com/981049/31636354-65b72dfe-b2ca-11e7-8c7d-7279477906d0.png)

* When **OctoPrint** in rebooted and the UI reloaded, click on the wrench icon in the toolbar
* Under **Plugins**, choose **Firmware Updater**
* Click on the wrench icon, and after **Path to advdude**, enter `/usr/bin/avrdude` and click **Save**
* Be sure your USB port appears after **Serial Port**
* After **... from file**, click on **Browse** and select the firmware you have downloaded such as `ADVi3pp-Mainboard-x.x.x.hex`
* Click on **Flash from File**
* The flashing process may take around 30 seconds
* When it is finished, a message appears saying "Flashing successful". Click on **Save**
* Reconnect the printer

### Mainboard Option 3 - Flashing using Arduino IDE

* Connect your printer to your PC using the USB cable
* Download [Arduino IDE](https://www.arduino.cc/en/Main/Software)
* Open Arduino IDE
* Under **Tools** &#8594; **Board**, select **Arduino/Genuino Mega or Mega 2560**
* Under **Tools** &#8594; **Processor**, select **ATMega2560 (Mega 2560)**
* Under **Tools** &#8594; **Port**, select the port your printer uses

![](assets/ArduinoIDE.png)

* Under **File** &#8594; **Open**, select `Marlin.ino` located in the **Marlin** folder
* Click on the **Upload** button, on press `Ctrl` + `U`

The firmware is compiled and uploaded. When the uploading is finished, the printer reboots.

### Mainboard Option 4 - Flashing using `avrdude`

All the previous options are using [`avrdude`](http://www.nongnu.org/avrdude/) underneath. `avrdude` is an utility to download/upload/manipulate the ROM and EEPROM contents of AVR microcontrollers. It can be downloaded from your favorite repository or from [Savannah Non-GNU web site](http://download.savannah.gnu.org/releases/avrdude/).

* For Debian/Ubuntu:

```
sudo apt install avrdude
```

* For macOS (Homebrew):

```
brew install avrdude
```

* For Windows, download `avrdude-6.3-mingw32.zip` or a higher version.

The command line is the following:

```
avrdude -v -p m2560 -c wiring -P <printer_port> -U flash:w:<firmware.hex>:i -D
```

Replace `<printer_port>` and `<firmware.hex>` with the right values. For example (Windows):

```
avrdude -v -p m2560 -c wiring -P COM3 -U flash:w:ADVi3pp-Mainboard-2.0.0.hex:i -D
```

or (Linux):

```
avrdude -v -p m2560 -c wiring -P /dev/ttyUSB0 -U flash:w:ADVi3pp-Mainboard-2.0.0.hex:i -D
```
