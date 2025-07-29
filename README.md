## My Linux Experience
### Legend
- ✔️ **Completed**
- ⚠️ **Possible but too difficult to do**
- 🚫 **Not possible on linux**

### Challenges
1. Play a car game with wheel ✔️ **(Use Oversteer)**
2. Play a rockstar game ✔️ **(Use Steam)**
3. Play a ubisoft game ✔️ **(Use Steam)**
4. Minecraft modding ✔️ **(Use Prism Launcher)**
5. Do pirated windows games work ✔️ **(Use Bottles)**
6. Record that feature ✔️ **(Use GPU Screen Recorder)**
7. Fallout modding ✔️ **(Use Bottles with Nyxus mod manager)**
8. VR 🚫 **Only works through AirLink**
9. HDR ✔️ **(Plasma 6 with Wayland)**
10. Play Gamepass game 🚫
11. Wallpaper Engine 🚫
12. GPU Overclocking 🚫
13. OpenHardwareMonitor 🚫
14. Nvidia DLSS and AMD FSR ✔️ **(Steam Proton Now Supports it!)**

### Pros
- Alt Tab with fullscreen games fast asf
- No Microsoft updates
- No ads baked into the OS
- More privacy
- More open source programs available
- Can literally customize anything
- Less bloat

### Cons
- Easy to brick the OS itself or specfic hardware drivers, if you go messing with files.
- More of a learning curve than using MacOS *Terminal simulator
- The shit I coded will probably need to be reprogrammed, ex WADDLE, Q.U.A.C.K.E.D and AutoPirate
- GSync gets disabled when recording
- VR *Not Supported yet
- HDR *Not Supported yet
- No Xbox App:
    - No parties
    - No Gamepass games
- No Wallpaper engine
- No GPU Overclocking
- No Openhardware Monitor
- No Discord activity

## Tips

- Use BTRFS filesystem
- **DO NOT USE ANY DRIVE WITH 'NTFS', linux can read and write but there could be problems**
- Use KDE partition manager to set mount points for external drives ex `/media/USER/DRIVE_NAME`

### **Konsole**

- To run `fastfetch` on a new Konsole window every time then open `~/.bashrc` and add `fastfetch` to the end 

### Proton

- If a Proton game crashes, try and turn off steam overlay before anything else

### Random Linux Commands

- Set Dolphin as default file manager `xdg-mime default org.kde.dolphin.desktop inode/directory`
- Make sh file executable `chmod +x`
- Remove directory with terminal `rm -rf`
- See how long your system takes to boot `systemd-analyze`
- See what is taking so long for your system to boot `systemd-anyalze blame`

### Bluetooth

#### Device not shown?

```bash
sudo nano /etc/bluetooth/main.conf
```
- Change `Controler Mode=` then test all options `dual`,`bredr`,`le`

#### Disable Handsfree and Headset profiles

1. ```bash
   # disable Bluetooth’s “switch to headset profile when an input stream appears”
   wpctl settings --save bluetooth.autoswitch-to-headset-profile false
   ```

2. Create a file at `~/.config/wireplumber/policy.lua.d/11-disable-headset.lua` with:

   ```lua
   -- disable “use headset profile” completely
   bluetooth_policy.policy["media-role.use-headset-profile"] = false
   ```

3. Create a file at ```~/.config/wireplumber/wireplumber.conf.d/51-stop-swapping-profiles.conf``` with:

   ```lua
   ## Disable automatic switch to HSP/HFP
   wireplumber.settings = {
     bluetooth.autoswitch-to-headset-profile = false
   }
   
   ## Only enable A2DP roles (remove HFP/HSP entirely)
   monitor.bluez.properties = {
     bluez5.roles = [ a2dp_sink a2dp_source ]
   }
   ```

   - This completely hides HFP/HSP so you’ll only ever see the high-fidelity A2DP options 

4. Restart WirePlumber

   ```bash
   systemctl --user restart wireplumber
   ```

### Errors and Fixes

#### Flatpak

- Fix `apply_extra script` error
    ```bash
    flatpak repair
    systemctl restart flatpak-system-helper.service
    flatpak update
    ```
- Fix `No remote refs found for ‘flathub’` error
    ```bash
    flatpak remote-add --user --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
    
    flatpak remote-add --system --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
    ```

#### HDR & Adaptive Sync

- Must Use Plasma 6 and Wayland
- Enable it through Display Settings

#### Discord Rich Presence(RPC) Full Experience

**Overlayed Does Not Work With Vesktop, Use The Native Discord App Instead**

- Use `DiscordRPC.sh` script 
  - **You may need to edit it to point to correct file locations**

- Or Follow Steps

  1. `Overlayed` **Delay 10s**

  2. `Vesktop` **(Delay by 15s)**
      1. Enable arRPC
         - Settings -> Vesktop Settings -> Rich Presence -> On
      2. Disable Notifications
  


#### Startup Scripts

- You must have ```#!/usr/bin/bash``` at the top of every startup script

#### Delay Autostart apps

1. Goto `~/.config/autostart/` and edit the `.desktop` file you want to delay
2. In `Exec=` set it to `sh -c "sleep Xs"; exec APP"` 
   - **replace X with amount of seconds and APP with the app you want to launch**
3. Add `X-KDE-AutostartCondition=true` to the end of the `.desktop` file


## Nvidia Drivers

- Follow [this](https://ubuntu.com/server/docs/nvidia-drivers-installation) to update **NVIDIA drivers**
    - Fix cant write to xorg file `sudo chmod u+x /usr/share/screen-resolution-extra/nvidia-polkit`
    - To enable all nvidia x server settings run `sudo nvidia-xconfig --cool-bits=value` value=28 for Fermi GPUs and up
    - If Gsync is not an option in Nvidia-X-Server-Settings make sure your selected GPU driver is an open one ex. `nvidia-driver-xxx-open` **Wayland has adaptive sync built into its System Settings use that if no Gsync**

## KDE Settings
### Appearance
- Wallpaper
    - KDE Glitch Wallpaper
- Cursor
    - Sweet Cursors
- Splash Screen
    - Breeze
- Boot Splash Screen
    - Kubuntu Logo
- Desktop Effects
    - Settings -> Window Management -> Desktop Effects
        - Set Fall apart windows on close
        - Set Wobbly Windows
        - Set Transluncy
- Set appearance color to orange
    - Settings -> Appearance -> Colors
- Auto hide Task Bar
    - Right click Task Bar -> Show Panel Config -> Visibility -> Auto Hide
- Custom Fonts
    1. Open Font Management
    2. Drag and drop font folder into All Fonts group
    3. Goto System Settings -> Text & Fonts -> Adjust All Fonts
### Notifications
- Set notifications to overlay on fullscreen windows
    - Settings -> Notifications -> Visibility Conditions
- Set notifications hide after
    - Settings -> Notifications -> Hide after -> 10 secs
### Other
- Set Session restore with empty session
    - Settings -> Startup and Shutdown -> Desktop Session
- Auto mount storage devices
    - System Settings -> Hardware -> Removable Storage -> Removable Devices -> All Devices
- Auto open video files with VLC
    - Settings -> File Associations -> Video
    - Then mark all video formats with VLC
- Update Software Manually
    - Settings -> Software update
    - Enable Update software Manually
    - Enable Apply System updates after rebooting

## Usage
**Set Super + t as new terminal**

- Settings -> Keyboard -> Shortcuts -> Konsole

**Set Dolphin Keyboard Shortcuts**

- Dolphin -> Hamburger Icon -> Configure -> Configure Keyboard Shortcuts

## Linux Programs Installations
| Linux               | Install |                             Notes                                                                                              |
|---------------------|---------------------------------------------|----------------------------------------------------------------------------------------------------|
| nala                | `sudo apt install nala`        | apt but better                                                                |
| fastfetch  | `sudo nala install fastfetch` | Quickly get system information                                               |
| flatpak             | `sudo nala install flatpak`        | Package installer                                                         |
| Balenca Etcher             | https://etcher.balena.io/#download-etcher | Creates Bootable USB's|
| Bottles             | `flatpak install flathub com.usebottles.bottles` | Run Windows applications |
| Brave Browser | `curl -fsS https://dl.brave.com/install.sh | sh` | Chromium based and blocks ads |
| Flatseal            | `flatpak install flathub com.github.tchx84.Flatseal` | Package permission modifer GUI                          |
| Discord             | `flatpak install flathub com.discordapp.Discord`                         |  |
| DSD-FME             | Read Instructions Below                         | Digital Voice Decoder  |
| Filezilla           | `sudo nala install filezilla`                                            |   |
| Gminer              | https://github.com/develsoftware/GMinerRelease    | Crypto Miner  |
| Git                 | `sudo nala install git`    |   |
| Grid Tracker        | Read Instrcutions Below    | Program used to mark Ft8 locations  |
| Gpredict            | `sudo nala install gpredict`    | Program used to track satelites  |
| Chirp               | Read Instructions Below    | Radio programming application  |
| Earth               | https://www.google.com/earth/about/versions/ |   |
| Handbrake             | `flatpak install flathub fr.handbrake.ghb`   | Video compression tool  |
| Jackett             | `sudo git clone https://github.com/Jackett/Jackett.git /usr/share/Jackett/`   |   |
| Linux Gamemode      | Read Instructions Below                | Enables gamemode on linux  |
| LM Studio | https://lmstudio.ai/download | Lets you host LLMs from huggingface and other places |
| NOAA-APT            | https://noaa-apt.mbernardi.com.ar/download.html | Decodes APT wav files  |
| Overlayed | https://github.com/overlayeddev/overlayed/releases | Gives you voice activity overlay for Discord |
| Playback          |   Read instructions below                            | Program for the Gameboy cart reader  |
| player-ctl          | `sudo nala install playerctl`                               | Fixes media keys on keyboard, Read Instructions  |
| Plex HTPC           | `flatpak install flathub tv.plex.PlexHTPC`                               |   |
| Plex AMP           | `flatpak install flathub com.plexamp.Plexamp`                               |   |
| Qbittorent          | `flatpak install flathub org.qbittorrent.qBittorrent`                    |   |
| Radiosonde Auto RX  | Read Instructions Below                             | Radiosonde tracker  |
| Rustdesk            | https://github.com/rustdesk/rustdesk/releases                                                | VNC Server and Viewer  |
| SatDump             | Read Instructions Below                             | Satelite Decoding Program  |
| SDR++               | Read Instructions Below                             | SDR monitoring program  |
| SDRangel            | Read Compile Instructions [Here](https://github.com/f4exb/sdrangel/wiki/Compile-from-source-in-Linux)                             | All in one SDR program  |
| Spotify             | `flatpak install flathub com.spotify.Client`                             |   |
| spotdl             | Read Instrcutions Below                             |   |
| Steam               | `flatpak install flathub com.valvesoftware.Steam`     | Open Flatseal and add the location of external drives to the filesystem section of Steam, This makes it so steam can use those drives. |
| Typora | https://typora.io/#linux | Markdown Editor |
| Visual Studio Code  | https://code.visualstudio.com/download |   |
| VLC                 | `sudo nala install vlc`                                                  |   |
| Virtual Box         | `sudo nala install virtualbox --install-suggests`           |   |
| Video Trimmer         | `flatpak install flathub org.gnome.gitlab.YaLTeR.VideoTrimmer`           | Lets you easly trim videos  |
| Vencord | ```sh -c "$(curl -sS https://raw.githubusercontent.com/Vendicated/VencordInstaller/main/install.sh)"``` | Lets you use custom themes on Discord |
| Vesktop | https://github.com/Vencord/Vesktop/releases **USE DEB PACKAGE** | Lets you use RPC on Discord (Must have [arRPC](https://github.com/OpenAsar/arRPC)) and custom themes |
| Windscribe          | https://windscribe.com/download/ |   |
| yt-dlp          | `sudo snap install --edge yt-dlp`                | Download youtube videos through the terminal  |

## Linux Alternatives

| Windows                 | Linux               | Install                                                   | Notes                                                                                              |
|-------------------------|---------------------|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Logitech G Hub (Wheel)  | Oversteer           | `flatpak install flathub io.github.berarma.Oversteer`                  | Lets you change steering wheel config                                                              |
| Logitech G Hub (Mouse)  | Piper               | `sudo nala install piper`             | Lets you change mouse config. Run `sudo nala install ratbagd` if Piper doesnt work                                                                      |
| Nvidia Shadow Play      | GPU screen recorder | `flatpak install flathub com.dec05eba.gpu_screen_recorder`  | For "Record that" feature, and simple screen recording/screenshots <br><br> **If you want AV1 encoding then you have to build from source**                                 |
| Curse Forge (Minecraft) | Prism Launcher      | `flatpak install flathub org.prismlauncher.PrismLauncher`   | Lets you download mods and modpacks from Curseforge                                                |
| Equalizer APO | Easy Effects      | `flatpak install flathub com.github.wwmm.easyeffects`   | Equalizer                                                |
| MSI Afterburner (Overlay)| Goverlay           | `sudo nala install goverlay`                                | Overlay of system information <br> If using flatpak steam run these commands <br><br> Add `xdg-config/MangoHud:ro` To flatseal in the Steam filesystem section<br><br>`flatpak install flathub org.freedesktop.Platform.VulkanLayer.MangoHud` <br><br> `flatpak override --user --env=MANGOHUD=1 com.valvesoftware.Steam` <br><br> `sudo flatpak override --filesystem=xdg-config/MangoHud:ro` <br><br> Enable `Global Enable` in Goverlay                                                                    |
| Vortex | Bottles with Vortex      | Read instructions below   | Lets you download mods and modpacks from Vortex                                                |
| Grand Perspective | Disk Usage Analyzer | ```flatpak install flathub org.gnome.baobab``` | Lets you see your disk space more easily. |

### Edit Swap File

	- Swap file is like the page file for windows, if your ram gets full itll use storage as ram

```bash
sudo swapoff /swap/swapfile
sudo rm /swap/swapfile
sudo truncate -s 0 /swap/swapfile
sudo chattr +C /swap/swapfile
sudo fallocate -l 6G /swap/swapfile
sudo chmod 600 /swap/swapfile
sudo mkswap /swap/swapfile
sudo swapon /swap/swapfile
```

- Edit `/etc/fstab` and change the swap line to `/swap/swapfile  none swap sw 0 0`

  ```bash
  sudo nano /etc/fstab
  ```

- Check Swappiness

  ```bash
  cat /proc/sys/vm/swappiness
  ```

- Set Swappiness to 10

  - Create and edit a file `/etc/sysctl.d/10-swappiness.conf` add `vm.swappiness=10`

    ```bash
    sudo nano /etc/sysctl.d/10-swappiness.conf
    sudo sysctl --system
    ```

- The swap file will then only be used when my RAM usage is around **80** or **90** percent.

### RSYNC

- Moves all files from one directory to another and validates it with checksum
```bash
rsync -achP [FROM]/ [TO]
```

### Linux Gamemode
```bash
sudo nala install meson libsystemd-dev pkg-config ninja-build git libdbus-1-dev libinih-dev build-essential systemd-dev
sudo git clone https://github.com/FeralInteractive/gamemode.git ~/gamemode/
cd ~/gamemode/
git checkout 1.8.2
./bootstrap.sh
gamemoded -t # Run this to verify it actually works
```
- If you get the error `PermissionError: [Errno 13] Permission denied: '/home/donovan/gamemode/builddir'` then make sure the gamemode directory is writable ex `sudo chmod 777 ~/gamemode/`
  
- If gamemode cant set CPU governor then follow this
  
    - 1:
        - Goto Settings -> Power Management -> Other Settings -> Switch Power Profile -> Performance
        - Reboot
    
    - 2:
        - Check which governor modes your CPU supports `cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors`
        - See current mode selected `cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor`
        - Change governor for all cores `echo "performance" | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor`

- Set games on Steam to use gamemode
    ```bash
    gamemoderun %command%
    ```

### Playback
1. Download Playback app image https://www.epilogue.co/downloads
2. Run these
```bash 
sudo usermod -a -G dialout $USER
sudo usermod -a -G plugdev $USER 
```
3. Reboot/Logout Playback will now have acess to USB devices
4. Run these commands
```bash
sudo touch /etc/udev/rules.d/99-epilogue.rules
sudo nano /etc/udev/rules.d/99-epilogue.rules
```
5. Add this to udev rules
```bash
# Operator Core
SUBSYSTEM=="usb", ATTR{idVendor}=="16d0", ATTR{idProduct}=="123B", MODE="0666"

# Operator Sync
SUBSYSTEM=="usb", ATTR{idVendor}=="16d0", ATTR{idProduct}=="123C", MODE="0666"

# GB Operator (release)
SUBSYSTEM=="usb", ATTR{idVendor}=="16d0", ATTR{idProduct}=="123D", MODE="0666"

# SN Operator
SUBSYSTEM=="usb", ATTR{idVendor}=="16d0", ATTR{idProduct}=="123E", MODE="0666"

# Legacy or internal prototypes
SUBSYSTEM=="usb", ATTR{idVendor}=="1d50", ATTR{idProduct}=="6018", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="1209", ATTR{idProduct}=="db42", MODE="0666"
```
6. Save the file[ctrl+x] and restart

### player-ctl Media Keys Fix
1. Install player-ctl `sudo nala install playerctl`
2. Goto Settings -> Shortcuts -> Custom Shortcuts -> Edit -> New -> Global Shortcut
3. Set the trigger to whatever you want
4. Set command to one of the following
    ```bash
    playerctl play-pause
    playerctl next
    playerctl previous
    playerctl stop
    ```

### Bottles Steam and Vortex Modding Instructions:
1. Open flatseal and add External Drive locations to the Bottles section of flatseal E.G Other Files
2. run `flatpak override --user com.usebottles.bottles --filesystem=~/.var/app/com.valvesoftware.Steam/data/Steam`
3. Enable Proton in Bottles. Settings -> General -> Intergrations -> Steam Proton Prefix -> On
4. Open Bottles and click on a game to mod **If game isnt in list launch it in steam first, and reopen bottles.**
5. Click run executable and select the vortex setup exe
6. Once done installing click on `Add shortcuts`
7. Add Vortex * Normally its installed here `/steamapps/compatdata/STEAMID/pfx/drive_c/Program Files/Black Tree Gaming Ltd/Vortex/Vortex.exe`
8. Run Vortex and mod! *Drag and drop zip files into Vortex to install mods*
- Fallout New Vegas
    - **NVSE**
        - Install it using Vortex
        - Go to game dir and rename `nvse_loader.exe` to `FalloutNVLauncher.exe`

### spotdl
```bash
sudo nala install pipx
pipx install spotdl
# To Run
pipx run spotdl
```

## SDR Programs

### RTL SDR Setup
```bash
sudo nala purge ^librtlsdr
sudo rm -rvf /usr/lib/librtlsdr* /usr/include/rtl-sdr* /usr/local/lib/librtlsdr* /usr/local/include/rtl-sdr* /usr/local/include/rtl_* /usr/local/bin/rtl_*
sudo apt-get install libusb-1.0-0-dev git cmake pkg-config
git clone https://github.com/rtlsdrblog/rtl-sdr-blog
cd rtl-sdr-blog
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON
make
sudo make install
sudo cp ../rtl-sdr.rules /etc/udev/rules.d/
sudo ldconfig
echo 'blacklist dvb_usb_rtl28xxu' | sudo tee --append /etc/modprobe.d/blacklist-dvb_usb_rtl28xxu.conf
```

### SDR++
#### Deb Package
https://github.com/AlexandreRouma/SDRPlusPlus/releases/tag/nightly

#### Compile
```bash
sudo nala install libfftw3-dev libglfw3-dev libvolk-dev libzstd-dev libairspyhf-dev libiio-dev libad9361-dev librtaudio-dev libhackrf-dev
sudo nala install libairspy-dev
git clone https://github.com/AlexandreRouma/SDRPlusPlus.git ~/SDRPlusPlus
cd ~/SDRPlusPlus/
mkdir build
cd build
cmake ..
make -j<N>
sudo make install
```

### Radiosonde Auto RX
#### Compile
```bash
sudo nala update
sudo nala upgrade
sudo nala install python3 python3-venv sox git build-essential libtool cmake usbutils libusb-1.0-0-dev rng-tools libsamplerate-dev libatlas3-base libgfortran5 libopenblas-dev
sudo nala install rtl-sdr
sudo wget -O /etc/udev/rules.d/20-rtlsdr.rules https://raw.githubusercontent.com/osmocom/rtl-sdr/master/rtl-sdr.rules
echo "Paste This into the next file"
echo "blacklist dvb_usb_rtl28xxu
blacklist rtl2832
blacklist rtl2830
blacklist dvb_usb_rtl2832u
blacklist dvb_usb_v2
blacklist dvb_core"
sleep 5
sudo nano /etc/modprobe.d/rtlsdr-blacklist.conf
git clone https://github.com/projecthorus/radiosonde_auto_rx.git ~/radiosonde_auto_rx
cd ~/radiosonde_auto_rx/auto_rx
./build.sh
cp station.cfg.example station.cfg
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```
- To run **Radiosonde Auto RX**
    ```bash
    cd ~/radiosonde_auto_rx/auto_rx/
    python3 -m venv venv
    source venv/bin/activate
    python3 auto_rx.py
    ```

### HorusModGUI
#### Compile
```bash

```

### Chirp
#### Compile
```bash
sudo nala install python3-wxgtk4.0 pipx
pipx install --system-site-packages ./dep/chirp-20240618-py3-none-any.whl
pipx ensurepath
sudo usermod -a -G tty $USER
# Reboot
```

- To run **Chirp**
    ```bash
    chirp
    ```

### Grid Tracker
#### Compile
```bash
cp -R packages/GridTracker-1.24.0512-linux-x64/ ~/
```

- To run **Grid Tracker**
    ```bash
    ~/GridTracker-1.24.0512-linux-x64/GridTracker
    ```

### SatDump
#### Compile
```bash
# Install dependencies on Debian-based systems:
sudo nala install git build-essential cmake g++ pkgconf libfftw3-dev libpng-dev libtiff-dev libjemalloc-dev   # Core dependencies
sudo nala install libvolk-dev libcurl4-openssl-dev                                                           # If this package is not found, use libvolk-dev or libvolk1-dev
sudo nala install libnng-dev                                                                                  # If this package is not found, follow build instructions below for NNG
sudo nala install librtlsdr-dev libhackrf-dev libairspy-dev libairspyhf-dev                                   # All libraries required for live processing (optional)
sudo nala install libglfw3-dev zenity                                                                         # Only if you want to build the GUI Version (optional)
sudo nala install libzstd-dev                                                                                 # Only if you want to build with ZIQ Recording compression
# (optional)
sudo nala install libomp-dev                                                                                  # Shouldn't be required in general, but in case you have errors with OMP
sudo nala install ocl-icd-opencl-dev                                                                          # Optional, but recommended as it drastically increases speed of some operations. Installs OpenCL.
sudo nala install intel-opencl-icd                                                                            # Optional, enables OpenCL for Intel Integrated Graphics

# If libnng-dev is not available, you will have to build it from source
git clone https://github.com/nanomsg/nng.git
cd nng
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=ON -DCMAKE_INSTALL_PREFIX=/usr ..
make -j4
sudo make install
cd ../..
rm -rf nng

# Finally, SatDump
git clone https://github.com/altillimity/satdump.git
cd satdump
mkdir build && cd build
# If you do not want to build the GUI Version, add -DBUILD_GUI=OFF to the command
# If you want to disable some SDRs, you can add -DPLUGIN_HACKRF_SDR_SUPPORT=OFF or similar
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr ..
make -j`nproc`

# To run without installing
ln -s ../pipelines .        # Symlink pipelines so it can run
ln -s ../resources .        # Symlink resources so it can run
ln -s ../satdump_cfg.json . # Symlink settings so it can run

# To install system-wide
sudo make install
```

- To run **SatDump**
    ```bash
    ./satdump-ui
    ```

### DSD-FME
#### Compile
```bash
wget https://raw.githubusercontent.com/lwvmobile/dsd-fme/audio_work/download-and-install-ubuntu2404lts.sh
sh download-and-install-ubuntu2404lts.sh
```

- To run **DSD-FME**
    ```bash
    dsd-fme
    ```