# Keyboard illumination control for Tuxedo Stellaris Gen. 6 (and maybe others) 

## Switches the keyboard illumination  on with a certain brightness and switches it off again after an adjustable period of inactivity.

## Why this little script?
Because by default the keyboard illumination is only switched off when the screen saver appears. This time span was too long for me personally.
This script turnes the keyboard illumination after a configurable timespan `--timeout ` off. It remembers the current brightness and color settings. If the keyboard illumiation is turned off it will be set to the vaules given in `--brightness` and `--colour`.
The keyboard illumination is turned using various effects (kids all like it). This can be turned off using `--noeffects`


## Other Hardware
Adaptation to other hardware that also follows the sysfs standard for keyboard illumination should be feasible without any problems.

## Features

- Turns on backlight when mouse/keyboard activity is detected
- Automatic shutoff after configurable inactivity period
- Customizable RGB colors and brightness levels
- Persistent system service with automatic restart
- Logging to `/var/log/keyboardlight-idle.log`
- If the backlight brightness is set to 0 (keyboard illumination is turned off),
  the default settings are used

## Requirements

- Linux with sysfs LED support
- Python 3.7+
- systemd init system
- Root privileges for installation

## Installation

1. **Clone the repository**
```
git clone https://github.com/dolorosus/TuxedoKeyboardLighting.git
cd keyboardlight-idle
```

2. **Build and install executable**
```
bash ./install.sh
```

3. **Verify service status**
```
systemctl status keyboardlight-idle
```

## Configuration

Edit the service file to modify startup parameters:
```
sudo nano /etc/systemd/system/keyboardlight-idle.service
```

**Recommended parameters:**
```
ExecStart=/usr/local/bin/keyboardlight-idle \
  --timeout 20 \
  --brightness 4 \           
  --colour "#0000ff"  
```
**Parameters:**
  `--timeout `     Seconds until keyboard illumination is turned off  
  `--brightness `  (0-50) default keyboard illumination brightness  
  `--colour `      default colour  
  `--noeffect `    Turning light off without effect

  The keyboard illumiation is reset to default brightness and colour, if the keyboard illumiation is turned off. 

## Service Management

| Command | Description |
|---------|-------------|
| `sudo systemctl start keyboardlight-idle` | Start service |
| `sudo systemctl stop keyboardlight-idle` | Stop service |
| `sudo systemctl enable keyboardlight-idle` | Enable autostart |
| `sudo journalctl -u keyboardlight-idle` | View logs |

## Build Details

The installation script:
- Creates isolated Python virtual environment
- Installs PyInstaller and dependencies
- Builds single-file executable
- Deploys to `/usr/local/bin`
- Configures logging rotation

## Notes 
- Requires compatible RGB keyboard with sysfs interface
- Debug using `journalctl -u keyboardlight-idle -f`
- Uninstall by removing:
  - `/usr/local/bin/keyboardlight-idle`
  - `/etc/systemd/system/keyboardlight-idle.service`

If you're using Plasma, you can define keyboard shortcuts for controlling the keyboard illumination brightness e.g. `meta+F1` and `meta+F2`.

To set the default colour and brightness, just turn the keyboardlight off. After reaching the timeout, the keyboard will be initialized with the setting from `--brightness` and `--colour`

## License

CC BY-NC/CC BY-NC-SA

