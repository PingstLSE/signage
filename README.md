# Simple Signage with Raspberry Pi 4

A digital signage solution using Raspberry Pi 4 and FullPageOS to display a Google Calendar and Instagram feed in vertical orientation.

## Hardware Requirements

- [Raspberry Pi 4 4GB Starter Kit](https://www.netonnet.se/art/datorkomponenter/barebone/raspberry-okdo-raspberry-pi-4-4gb-starter-kit/1054739.9477)
- MicroSD card (minimum 8GB recommended)
- Display with HDMI input
- Power supply (included in starter kit)
- Network connection (WiFi or Ethernet)

## Software Requirements

- FullPageOS (latest version)
- Raspberry Pi Imager
- Text editor (like Notepad++, not Windows Notepad)

## Installation Steps

### 1. Prepare FullPageOS

1. Download and install [Raspberry Pi Imager](https://www.raspberrypi.com/software/)
2. Download [FullPageOS](https://github.com/guysoft/FullPageOS#where-to-get-it)
3. Use Raspberry Pi Imager to flash FullPageOS (latest Nightly build) to your MicroSD card

### 2. Configure FullPageOS

After flashing the image, but before ejecting the SD card into the Raspberry PI:

1. Open the `boot` partition
2. Edit `fullpageos.txt`:
   ```
   # Replace with your GitHub Pages URL
   https://yourusername.github.io/signage/
   ```

3. For WiFi setup, edit `wifi.nmconnection`:
   ```
   [connection]
   id=wifi
   uuid=593819b8-135a-4a3e-9611-c36cdeadbeef
   type=wifi
   interface-name=wlan0

   [wifi]
   mode=infrastructure
   ssid=YOUR_WIFI_NAME

   [wifi-security]
   auth-alg=open
   key-mgmt=wpa-psk
   psk=YOUR_WIFI_PASSWORD

   [ipv4]
   method=auto

   [ipv6]
   addr-gen-mode=default
   method=auto

   [proxy]
   ```

### 3. Setup Web Content

You can host your web content anywhere you like - any web server or hosting service will work. This repository uses GitHub Pages as a free hosting solution.

#### Using GitHub Pages (Free Solution)
1. Fork this repository
2. Enable GitHub Pages in your repository settings:
   - Go to Settings > Pages
   - Select your main branch as source
   - Your site will be available at `https://yourusername.github.io/signage/`
3. Update the following in `index.html`:
   - Google Calendar embed URL (replace `pingstkyrkan.lycksele@gmail.com` with your calendar email)
   - Instagram feed URL (replace the SnapWidget embed code)
   - Adjust refresh intervals if needed (currently set to 5 minutes for widgets, 30 minutes for page)

#### Alternative Hosting Options
- Self-hosted web server
- Cloud hosting services (AWS, Azure, etc.)
- Static site hosting (Netlify, Vercel, etc.)
- Local hosting on the Raspberry Pi itself

Remember to update the URL in `fullpageos.txt` to point to wherever you host your content.

### 4. Final Setup

1. Insert the MicroSD card into your Raspberry Pi
2. Connect display, power, and network
3. Power on the Raspberry Pi
4. Wait for initial boot and configuration (may take a few minutes)

## Optional: Configure Display Rotation

If you want to use your display in portrait mode (vertical orientation), follow these steps:

1. Before inserting the SD card into the Raspberry Pi, edit `config.txt` in the boot partition
2. Add/modify these settings:

```
[pi4]
# IMPORTANT: Disable all dtoverlay configurations
#dtoverlay=vc4-fkms-v3d
max_framebuffers=2

[all]
# Enable raspicam
start_x=1
disable_splash=1

# Increase GPU memory for display rotation
gpu_mem=256

# Rotate HDMI screen (1=90°, 2=180°, 3=270°)
display_hdmi_rotate=1
```

⚠️ **IMPORTANT**: For display rotation to work properly:
- Disable all dtoverlay settings, especially `dtoverlay=vc4-kms-v3d` and `dtoverlay=vc4-fkms-v3d`
- These OpenGL drivers do not allow display rotation via config.txt
- Make sure GPU memory is set to at least 256MB

## Customization

### Display Settings
- The current setup uses a 1080x1920 resolution (portrait mode)
- Calendar height: 720px
- Instagram feed height: 1060px
- Adjust these values in `index.html` if needed for your display

### Refresh Intervals
Current settings in `index.html`:
- Widgets refresh: Every 5 minutes
- Page refresh: Every 30 minutes

Modify these values in the JavaScript section if needed:
```javascript
setInterval(function() {
  // Widget refresh
}, 300000); // 5 minutes

setInterval(function() {
  // Page refresh
}, 1800000); // 30 minutes
```

## Troubleshooting

1. **Black Screen**: If screen stays black after boot, check:
   - HDMI connection
   - Power supply
   - Display rotation settings

2. **No Network**: Verify:
   - WiFi credentials in `fullpageos-wpa-supplicant.txt`
   - Network availability
   - Try Ethernet connection

3. **Display Not Rotating**: Ensure:
   - All dtoverlay settings are disabled
   - GPU memory is set to at least 256MB
   - Correct rotation value in `config.txt`

## Contributing

Feel free to submit issues and enhancement requests!

## License

[MIT License](LICENSE)