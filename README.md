# blade-egpu-switcher
Scripts that deal with docking and un-docking an eGPU from the Razer Blade laptop.

*Tested on Elementary OS Loki using a Razer Blade 2017, a Razer Core X Chroma w/ an RTX 2070 Super, and Nvidia/Nvidia Prime driver version 430*

### Requirements

- An Nvidia Graphics card
- An Nvidia optimus laptop
- X display driver
- Nvidia and Nvidia prime drivers >= 430

### Installation

- Open terminal and type `lspci | grep -i "vga"`. Make note of the intel and eGPU's Bus ID's. (the id to the very left. ie. `07:00.0 ` ). You'll need these to configure the xorg file below.

- Open the xorg.conf.egpu file inside the project directory

  - 1. set the Intel Bus ID:

  - ```
    Section "Device"
        Identifier "intel"
        Driver "modesetting"
        BusID "PCI:0@0:2:0" #enter your intel diplay adapter's Bus ID here
        Option "AccelMethod" "None"
    EndSection
    ```

    - **Be sure to maintain the same Bus ID formatting (i.e. Bus ID `00:02.0` would be `PCI:0@0:2:0`)**

  - 2.  set the Nvidia eGPU Bus ID:

    - ```
      Section "Device"
          Identifier "nvidia"
          Driver "nvidia"
          BusID "PCI:7@0:0:0" #enter your egpu's Bus ID here
          Option "ConstrainCursor" "off"
          Option "AllowEmptyInitialConfiguration"
          Option "AllowExternalGpus"
      EndSection
      ```

      - The key here is the `Option "AllowExternalGpus"` option. Without this, the eGPU will not be recognized.

  - 3. Save the file.

- Copy the xorg.conf.epu file into your X11 folder. In my case, my X11 folder lives at `/etc/X11` 

  - `sudo ./cp xorg.conf.egpu /etc/X11/xorg.conf.egpu`

- Copy the `dock-egpu` and `undock-egpu` scripts into your `bin` folder. (`/usr/bin/` in my case.)

- `sudo cp ./dock-egpu /usr/bin/dock-egpu; cp ./undock-egpu /usr/bin/undock-egpu`

### Usage

**If you already have an xorg.conf file, be sure to create a backup before running the scripts below!!**

- Docking the egpu
  - run `dock-egpu`
- Un-docking the egpu
  - run `undock-egpu`

### Uninstall

1. `sudo rm /usr/bin/dock-egpu; rm /usr/bin/undock-egpu` 
2. `sudo rm /etc/X11/xorg.conf.egpu`
3. Restore xorg.conf from backup, if applicable.