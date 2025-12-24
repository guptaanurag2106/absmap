# absmap

`absmap` is a straightforward remapper for absolute value input devices, primarily designed and tested for graphics tablet touch strips.

## What it does

This tool listens for `ABS_RX` or `ABS_RY` events (like those from a touch strip) and, based on the direction and speed of your input, triggers configured actions. These actions can be:
- Emitting key presses (e.g., `PAGEUP`, `PAGEDOWN`)
- Running shell commands (e.g., `pactl` for volume, `brightnessctl` for screen brightness, or `ydotool` for more complex key combos).

## Quick Start

1.  **Clone this repo:**
    ```bash
    git clone https://github.com/guptaanurag2106/absmap.git
    cd absmap
    ```

2.  **Make it executable:**
    ```bash
    chmod +x absmap.py
    ```

3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```
    *Note: `evdev` typically requires root access or specific udev rules to read input devices. `ydotool` needs to be installed separately if you plan to use it for commands.*

4.  **Run with your config:**
    ```bash
    ./absmap.py your_config.yml
    ```

## Example Configuration (`basic.yml`)

```yaml
device:
  # Find your device name using `ls -l /dev/input/by-id` or `cat /proc/bus/input/devices`
  name: HUION Huion Tablet_H952 Touch Strip # Or use 'path: /dev/input/eventX'

axis: ABS_RX # The axis your touch strip emits events on (e.g., ABS_RX, ABS_RY)

settings:
  velocity_threshold: 3.0 # How fast you need to move for a gesture to register
  acceleration: false     # Enable/disable acceleration-based sensitivity adjustment
  cooldown: 300           # Cooldown in milliseconds between repeated actions
  key_delay: 10           # Delay in milliseconds between key press and release for emitted keys
  history_size: 6         # Number of samples to keep for velocity/acceleration calculation
  grab: true              # Grab exclusive access to the device (recommended)

gestures:
  up:
    action:
      keys: [KEY_PAGEUP] # Emits the PageUp key
  down:
    action:
      command: "ydotool key 109:1 109:0" # Example ydotool command for PageDown
```

Feel free to open `examples/basic.yml` and `examples/advanced.yml` for more ideas!
Adjusting the settings will require some experiment, based on your device values and usage.
