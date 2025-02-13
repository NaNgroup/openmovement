# AX3/AX6 Troubleshooting

## Connection Troubleshooting

1. **What is the connection/charging arrangement between the device and the computer?**

    Devices should only be connected directly to a computer or a USB hub that is externally powered with its own supply -- not a *passive* hub without its own power, and not left on a hub that is later removed from power. The devices can become discharged if left connected to a USB power source is not providing sufficient power, such as multiple devices connected to a passive USB hub, or a powered hub that is no longer powered, or left attached to a computer that goes to sleep and might provide less power.

2. **What exactly does the LED light do when you connect the device to a computer?**

    It should first flash through various colours for a short while, then settle to just a slowly pulsing yellow or white light.

    If it does not (e.g. no light, or goes to a solid yellow or green, or momentarily disappears in a way that is not part of a gradual fade off/on) there is likely a connection or communication problem, please check:

    * Is there a gentle click when you firmly insert the connector? (Without this, it may not make a proper connection to the data pins.)
   
    * Does trying another cable make a difference? (Cables/contacts can easily be broken or may have different tolerances.)
   
    * Does connecting to a different USB port (directly on the computer) make a difference? (Windows drivers are run for a specific device and port, using another port can sometimes fix a temporary glitch.)
   
    * Completely power-off/shutdown everything, wait a little, then restart -- does this make a difference? (This is not just standby/suspend/hibernate but a full power-off/restart, and any external USB hubs should also be disconnected from power to reset them. Although a cliché, this does often fix many temporary issues, e.g. if a USB port has gone "over current" it may have a temporary thermal fuse "blown" until power is disconnected).
   
    * By closely looking inside the device connector, is there any debris (such as grit or fluff) or grease or damage?  If so: does cleaning with a sharp blow or gentle use of a fine point make a difference? (But do not connect while there is moisture in the port, as this could corrode the contacts).
   
    * By gently moving the device around in the light, you should see the five shiny rectangular contacts on the central part of the connector (shorter side) -- is there is any sign of corrosion or grease preventing contact? (If the device has been externally connected to power while conductive liquid was in the port, such as non-pure water, it would be possible to cause corrosion on the connector). 

3. **Does trying the device and software on a completely different computer make a difference?**

    In particular: if it's not working on an organization-managed PC, does trying on a personal laptop make a difference?

4. **Does the device LED slowly pulse yellow/white for a sustained amount of time?** (Check over at least 15 seconds)

    If not, and everything else above was already checked, there may be an issue with it.  For further diagnosis, please note exactly what the device LED _is_ doing 
   
    _The following points are only likely to be worth considering if the device LED is behaving normally in this way_.

5. Under Windows Explorer's *This PC* where you see your computer's drives, **does the AX device appear as a drive?**

    * If so: you can manually copy off the data file `CWA-DATA.CWA`, if required.
   
    * If not: might your computer have any strict anti-virus software or security policies about access to removable USB drives? 

6. Open *Device Manager*, **does an entry appear listed under the *Ports* category for the device?** (Or sometimes under *Portable Devices*) 
   
    If not, then there may be a driver issue, please try the OmGUI installation again, ensuring it is as a user with administrative rights as this attempts to install a driver (not usually be needed if you're running Windows 10) -- did the installation (with driver) complete without any issues?

7. If the device appears as a drive and a "port", then the software should be able to communicate with it.  Please follow the *OmGui Software Troubleshooting* guide below.


## OmGui Software Troubleshooting

The standard connection software is [OmGui](https://github.com/digitalinteraction/openmovement/wiki/AX3-GUI#downloading-and-installing).

1. **What version of OmGui software are you using?**

    Does installing [the current version](https://github.com/digitalinteraction/openmovement/blob/master/Downloads/AX3/AX3-GUI-revisions.md) (including any alpha/beta versions that may be available) make a difference? 

2. **Has the computer been restarted?**

    Although a bit of cliché, it is really worth restarting the computer and trying again, as this can clear any issues at the driver or operating system levels.

3. **Does trying the device and software on a completely different computer make a difference?**

    If it's not working on an organization-managed PC (perhaps from restrictive security software or settings), does trying on a personal laptop make a difference?

    Also note that *OmGui* is a Windows application and, although it may run under virtualization technology (such as *Parallels* under *macOS*), it is not tested for such configurations.

4. **Is the workspace on a network drive, or is there a restricted quota or limited drive storage space?**

    Some issues with transferring data may occur if the workspace is set to a network (shared) drive.  (This is often how virtualization programs such as *Parallels* map to the host computer's files).  It may be more reliable to use a local folder as a workspace, and to transfer the files off afterwards.
   
    In addition, be sure that the workspace folder you choose has sufficent free drive storage space (and is not restricted by a quota).

5. **Standard Log**

    Select *View*/*Log* (<kbd>Alt</kbd>+<kbd>V</kbd>, <kbd>L</kbd>), the log window will appear at the very bottom.  Resize it to be a little larger by dragging the bar just above it.  Perform the actions that you are troubleshooting.  If anything interesting appears in the log window, click in the Log window and select all of the text and copy it to the clipboard (<kbd>Ctrl</kbd>+<kbd>Home</kbd>, <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>End</kbd>, <kbd>Ctrl</kbd>+<kbd>C</kbd>).  Please paste the log output (<kbd>Ctrl</kbd>+<kbd>V</kbd>) into a document to save it.

    * If you see a message `EXCEPTION: System.Xml.XmlException:` <!-- ` Root element is missing.` -->, the file that stores the previous recording settings may have been corrupted.  You can try another working folder, or remove the configuration:

        a. Open the workspace folder you are using in *Explorer*: in the *Workspace* bar, press the open folder icon *Open working folder*

        b. Locate the file: `recordSetup.xml`
      
        c. Rename the file to something else (e.g. put a `_` character at the start)

        d. Close and restart OmGui.

    * If you see a message `EXCEPTION: System.Configuration.ConfigurationErrorsException: System.Configuration.ConfigurationErrorsException:
Configuration system failed to initialize`, the program configuration has become corrupted and can be reset:

        a. Make sure OmGui is not running

        b. Open the *Start*/*Run* window (<kbd>Windows</kbd>+<kbd>R</kbd>)

        c. Copy or type (followed by <kbd>Enter</kbd>): %LOCALAPPDATA%

        d. Single-click to select a folder in there named: `Newcastle_University,_UK`

        e. Rename that folder (<kbd>F2</kbd>) to something else (e.g. put a `_` character at the start)

        f. Now restart OmGui

6. **Detailed log?**

    If the above suggestions have not resolved the issue, please obtain an [OmGui Detailed Log](#omgui-detailed-log) as described in the next section.

7. **Simplest recording**

    If the device is not making a recording as you expect, please try the simplest configuration to record *Immediately on Disconnect* at *100 Hz* and *+/- 8 *g**, disabled gyroscope (AX6 only), and select *Flash during recording*.  If the configuration is successful, remove the device for 15 seconds -- does the LED flash green, and is there any data on the device when you connect it afterwards?  

8. **Device log**

    If the device is not making a recording as you expect, you can obtain a detailed log from the attached device, which should explain why it has stopped, by downloading the archive: [AX3-Utils-Win-3.zip](https://raw.githubusercontent.com/digitalinteraction/openmovement/master/Downloads/AX3/AX3-Utils-Win-3.zip), then extracting/unzipping all of the files from it to a folder, opening that folder, ensuring only the one device is attached, then double-clicking `log.cmd` to run the tool.  Copy the output it gives (there may be some unexpected letters or unusal symbols at the end of some of the lines, these can be ignored) -- this should be a timestamped record of the device's "stop reasons".
    
    If you have problems using the `log.cmd` above, you could alternatively try:

    * Open a browser that supports *Web Serial*, such as *Google Chrome*.
    * Visit this page: https://googlechromelabs.github.io/serial-terminal/
    * Connect the device and wait around 10 seconds.
    * In the *Port* dropdown, select the serial port the device is on.
    * Click the *Connect* button
    * Click in the black terminal area of the page
    * Type the following, followed by the Enter key (note that you will not see anything appear until you press Enter):
        `LOG`
    * Select the output lines of text and press Ctrl+C to copy it to the clipboard
    * Paste (Ctrl+V) into a text document so that you have a copy of the log

9. **Resetting the device**

    (Advanced) If you are having trouble programming a device, you can [manually reset the device](#resetting-the-device) by following the instructions below. 


### OmGui Detailed Log

Please try the following to extract a detailed log from *OmGui* about what it can see of the device:

1. Ensure *OmGui* is initially closed and that no devices are attached.
   
2. Open the *Run* window (press <kbd>Windows</kbd>+<kbd>R</kbd>)
   
3. Copy the line below and paste it in the Run box and press Enter to start OmGui with more verbose log messages (this assumes OmGui was installed in the default location):

   ```cmd
   CMD /C "SET OMDEBUG=2 && START "" "%ProgramFiles(x86)%\Open Movement\OM GUI\OmGui.exe""
   ```
   
4. Use OmGui as before until the problem occurs.
   
5. Select *View*/*Log* (<kbd>Alt</kbd>+<kbd>V</kbd>, <kbd>L</kbd>), the log window will appear at the very bottom.  Resize it to be a little larger by dragging the bar just above it.

6. Now perform the actions that you are troubleshooting, for example, one or more of the steps:
   * Attach the device (wait around 10 seconds for it to fully connect)
   * Optional: Attempt to download data from the device
   * Optional: Attempt to configure the device
   
7. Click in the Log window and select all of the text and copy it to the clipboard (<kbd>Ctrl</kbd>+<kbd>Home</kbd>, <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>End</kbd>, <kbd>Ctrl</kbd>+<kbd>C</kbd>)
   
8. Please paste the log output (<kbd>Ctrl</kbd>+<kbd>V</kbd>) into a document to save it.

Checking the log output:

* Device ID consistency: If it contains any `LOG: - MISMATCH:` lines, please see the next section to manually verify the device ID.

* If it shows an issue with a configuration file (in particular, if you are having an issue when trying to configure a device), see above section: *Standard Log*.


## Manually Verify Device ID Consistency

If you receive an error `The correct download file name cannot be established (device identifier not verified)` please obtain an *OmGUI Detailed Log* as described above (if not already done so).  The log entry may contain `LOG: - MISMATCH:`, indicating an issue with device ID.  

To manually verify device IDs, with a single connected device, please check the following four numbers (these should be the same):

1. *External ID:* The number written on the side of the device (AX3: the number after the `17-` or `18-` prefix; AX6: 7-digit numbers should start with a `60`)

2. *USB ID:* In *Device Manager*, under *Ports*, and the *COM* device under that, right-click / *Properties* / *Details* / *Property: Parent* -- the number that appears as the *Value* after the first part of the address `USB\VID_04D8&PID_0057\#####_` (where `#####` is `CWA17` or `AX664`)

3. *Filesystem ID:* Locate the disk drive that appears under *This PC* when you connected the device, right-click the drive and select *Properties*, the highlighted field, note the *Volume Label*, it should start `AX#_` (where `#` is `3` or `6`)

4. *Data-file ID:* The ID in the current data file on the device.  This is a bit difficult to extract! Press <kbd>Windows</kbd>+<kbd>R</kbd> to open the *Run* box, and copy and paste the following command to open a window and give you a number (replace `D:` with the drive letter for your device from above if necessary):

   ```cmd
   PowerShell -Command "& {$s=[System.IO.File]::OpenRead('D:\CWA-DATA.CWA');$b=New-Object byte[] 13;$c=$s.Read($b,0,13);$s.close();Write-Output(16777216*$b[12]+65536*$b[11]+256*$b[6]+$b[5]);[Console]::ReadKey()}"
   ```

If these numbers are inconsistent, you could try *resetting the device* (including the device ID) in the next section.


## Resetting the device

**NOTE:** This step is for advanced use only, and should only be performed if necessary.

**IMPORTANT:** This will *reformat* the device, deleting any existing data on there.  Please be certain it does not have the only copy of any data you'd like to keep.  You can manually move off data from the drive by locating the device's drive letter in *File Explorer* and move the `CWA-DATA.CWA` file to a safe location.  
 
1. Download the .ZIP file: [AX3-Bootloaders](https://github.com/digitalinteraction/openmovement/blob/master/Downloads/AX3/AX3-Bootloaders.zip?raw=true)

2. Open the .ZIP file and extract the program `HidBootLoader.exe`.

3. Ensure no devices are connected and that OmGui is NOT running.

4. Run (double-click) `HidBootLoader.exe`.

5. Check the *Port* field is clear (if not, make a note of what it says)

6. Connect the device that you’d like to reset, and which does not contain any data you need to keep (this procedure will wipe the drive), then wait a second or so.

7. If enabled, press the *Run* button and wait a couple of seconds or so.

8. Check the *Port* field now displays a `COM` port (if it had a value before, use the drop-down arrow if necessary to ensure that it now has a different value)

9. In the *Command* field, copy and paste one of the lines below.
  
    * **Not changing the device ID:** If you are just resetting the device state (and not the device ID):

       ```
       TIME 2020-01-01 00:00:00|FORMAT QC|LED 5
       ```

    * **Changing the device ID:** If you are also resetting the device ID (if it appears, from the above troubleshooting, that the device ID has somehow become incorrectly programmed):

       ```
       DEVICE=12345|TIME 2020-01-01 00:00:00|FORMAT QC|LED 5
       ```
           
       ...and you must change `12345` to match the ID number on the outside of the device case (after any "17-" or "18-" prefix).

10. Press *Send*

11. Wait several seconds while the device LED is red, it should eventually turn *Magenta* (a purple blue/red mixture).

12. Disconnect the device

13. Close the bootloader software

14. The device should now be in a reset state.

If you have problems using the `HidBootLoader.exe` program above, you could alternatively try:

1. Open a browser that supports *Web Serial*, such as [Google Chrome](https://google.com/chrome).
2. Visit this page: https://googlechromelabs.github.io/serial-terminal/
3. Connect the device and wait around 10 seconds.
4. In the Port dropdown, select the serial port the device is on.
5. Click the Connect button
6. Click in the black terminal area of the page
7. Type `ECHO 1` and press <kbd>Enter</kbd> -- you will not see anything appear until you have typed everything.
8. Now type the *Command* from the previous section except, after the last command and *instead* of any `|` symbol, press the <kbd>Enter</kbd> key. For example, the lines (remembering to replace `12345` with your device's ID):
    ```
    DEVICE=12345
    TIME 2020-01-01 00:00:00
    FORMAT QC
    LED 5
    ```

## Removing a Mount Point

**NOTE:** This step is for advanced use only, and unlikely to apply to your device and/or unlikely to work.

Perhaps an old *mount point* is interfering somehow - you could try clearing the mount point:
 
1. Make sure OmGui is not running, and the problematic device IS connected

2. Start an *elevated* command prompt (with administrative permission), *either* of these methods:

   * Press Start, type `Command`, right-click the *Command Prompt* search result and select *Run as Administrator*
   
   * Press <kbd>Windows</kbd>+<kbd>R</kbd>, type `cmd`, <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>Enter</kbd>, select *Yes*
   
3. Type (or copy/paste) followed by <kbd>Enter</kbd>:

   ```cmd
   cmd /k mountvol
   ```

4. The command may list the device's current volume, e.g.: `C:\Mount\AX3_#####\`

5. If so, type followed by <kbd>Enter</kbd>: (replace the path with whatever the previous command showed)

   ```cmd
   mountvol C:\Mount\AX3_#####\ /D 
   ```

6. Assuming no error message was shown, the mount point was removed: you can disconnect the device and close the window.


## Battery

### Charging Arrangement

Ensure devices are only charged by directly connecting to a computer or a USB hub that is always externally powered, and not connected through a "passive" hub without its own power, and also not left on a hub or computer that is later removed from power or allowed to sleep or hibernate.


### Battery Percentage Estimate

The battery percentage shown is only a rough estimate based on the voltage measurements and an idealized discharge curve - the actual discharge curve depends on the battery/age/components/tolerances, and is highly non-linear – so variation is expected from this estimate.

It quite normal for this value to initially jump when connected, to vary by a few percent once the battery is fully charged (i.e. it discharges slightly before recharging), or to estimate a maximum reading that falls short of a 100% estimate.  In all cases, the battery will be at its maximum capacity when connected to a sufficient power source for up to 2 hours.

The same estimates apply to battery discharge from a data file.  The data preview graph in OmGui allows you to select additional lines on the right-hand side to chart (you will need to resize the graph to access them all), and one of these is the estimated battery percentage.


### Battery Health

Devices have the following notice about maintaining battery health:

> Battery Conditioning: In order to protect the Lithium Ion battery in this product, devices should be stored in a fully charged state in low ambient temperatures. Devices in prolonged storage should be recharged to this level every three months.

Related to this points, there are two battery health messages in OmGUI:
 
* A "Caution: Device May Have Fully Discharged" message is based on a heuristic that the software notices a connected device had lost track of time, as this implies that the battery became fully discharged.  This does not necessarily mean there is a problem, but is primarily a reminder to explain that devices should not be left fully discharged for extended periods as lithium ion batteries could become damaged if they are stored completely depleted for a significant time - and that devices should be charged periodically to ensure this doesn't happen. 
 
* A "Warning: Device Possibly Damaged" message is also based on a heuristic, however, it is a generally reliable indicator that the device may be damaged.  It is given when a device appears to have been reset recently, which should only happen if the battery was fully discharged, and yet the battery is already reporting a high level of charge.  This situation has been observed if the battery has become damaged to the point of holding very little charge, but it might be possible that it could occur for other reasons, so the device should still be tested as describe below.

The message should be cleared once the device's clock is successfully configured to the correct time, and the easiest way to do this is configure any recording with the device.  Note that the configuring software may also remember (while it is kept running) which devices caused the caution even if they're disconnected/reconnected, so you might also have to restart the software too if you're immediately plugging a device back in.  

When given a battery health warning, it is advisable to fully charge the device then run a test recording (this can be just at static recording) for the duration that you'd typically want the devices to record for - this will establish the performance expected for subsequent recordings.  


## Filesystem or data problems

If you receive a warning from the operating system about the filesystem (e.g. *Error Checking* / *Repair this drive* / *Do you want to scan and fix*) you should initially ignore the message -- do not allow the operating system to try to fix anything as this can cause problems -- and attempt to continue as normal:

* If the device contains useful data, try to download the data as usual.  If this does not work, it may be possible to recover the data -- try the [cwa-recover](https://github.com/digitalinteraction/openmovement/blob/master/Software/AX3/cwa-recover/) process.

* If the device does not contain useful data, try to configure the device as normal.  If this doesn't work, follow the troubleshooting instructions above, up to [Resetting the Device](https://github.com/digitalinteraction/openmovement/blob/master/Docs/ax3/ax3-troubleshooting.md#resetting-the-device) if required.

You should check that you do not have any software unnecessarily writing to the removable drives, or interfering with their operation (potentially some antivirus/security software).


## Installation

If you have installation issues, consider the "no installer" variants listed at [OmGui Revisions](https://github.com/digitalinteraction/openmovement/blob/master/Downloads/AX3/AX3-GUI-revisions.md).  These packages are .ZIP archives containing executable content, so you may need to "download anyway".  You may also need to install the [AX Driver](https://github.com/digitalinteraction/openmovement/blob/master/Downloads/AX3/AX3-Driver-Win-5.zip) to use the devices (in particular, on older versions of Windows).  You will also need to ensure *.NET 3.5* componment is enabled (see below).


### .NET 3.5

OmGui requires the *.NET 3.5* Windows component to be enabled on the system.  

If it is not already enabled, there is more information on installing and troubleshooting .NET 3.5 at: [Install the .NET Framework 3.5](https://learn.microsoft.com/en-us/dotnet/framework/install/dotnet-35-windows)

There are various ways to enable the *.NET 3.5* component:

1. Open *Windows Features* by pressing <kbd>Windows</kbd>+<kbd>R</kbd>, and entering: `appwiz.cpl` -- then click *Turn Windows features on or off* and select *.NET Framework 3.5 (includes .NET 2.0 and 3.0)*, and press *OK*.
2. Manually with:
  * Open *Task Manager* (<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>Esc</kbd>)
  * Select *(Run) New task* (under the *File* menu or on the toolbar)
  * Type: `DISM.EXE /Online /Add-Capability /CapabilityName:NetFx3`
  * Click *Create this task with administrative privileges*, and press *OK*.
3. Alternatively, through the online installer: https://www.microsoft.com/en-gb/download/details.aspx?id=21
8. Alternatively, through the offline installer: https://dotnet.microsoft.com/en-us/download/dotnet-framework/net35-sp1

If you receive error `0x800f0906`, `0x800f0907`, `0x800f081f`, or `0x800F0922`, see: [.NET Framework 3.5 installation errors](https://learn.microsoft.com/en-GB/troubleshoot/windows-client/application-management/dotnet-framework-35-installation-error)

In addition to the above information: if you get error `0x800f081f` try the *Manual* method above; if you get error `0x800f0906`, this may be related to your managed computer having a specific system update source.  This is an issue for your IT administrators.  If you have Administrator permissions, and are allowed to do so, you could change your system update source:

1. Press <kbd>Windows</kbd>+<kbd>R</kbd>, type: `gpedit.msc`
2. Select *Computer Configuration* / *Administrative Templates* / *System* / *Specify settings for optional component installation and component repair* / *Enabled* / *Contact Windows Update directly to download repair content instead of Windows Server Update Services (WSUS)*
3. Open *Task Manager* (<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>Esc</kbd>)
4. Select *(Run) New task* (under the *File* menu or on the toolbar)
5. Type: `gpupdate /force`
6. Click *Create this task with administrative privileges*, and press *OK*.

