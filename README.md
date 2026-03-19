# homebridge-garage-door-wsensor

A [Homebridge](https://homebridge.io) plugin for controlling a garage door opener via a Raspberry Pi GPIO relay, with optional door state sensor support.

Compatible with **Homebridge 1.x** and **Homebridge 2.0**.

---

## Requirements

- Raspberry Pi (any model with GPIO)
- Node.js >= 14.15.3
- Homebridge >= 1.1.7 (including 2.0)
- A relay module wired to a GPIO output pin
- (Optional) A magnetic reed sensor or similar wired to a GPIO input pin

---

## Installation

### Option 1: Homebridge UI (recommended)

Search for `homebridge-garage-door-wsensor` in the Homebridge UI plugin tab and click **Install**.

### Option 2: Command line

```bash
npm install -g homebridge-garage-door-wsensor
```

Or, to install from a local copy of this repository:

```bash
cd /path/to/this/folder
npm install
npm link
```

---

## Homebridge Configuration

Add the following to the `accessories` section of your Homebridge `config.json`:

```json
{
    "accessory": "Garage Door Opener",
    "name": "Garage Door",
    "doorRelayPin": 11,
    "doorSensorPin": 13,
    "duration_ms": 500,
    "invertDoorState": false,
    "invertSensorState": false,
    "input_pull": "up"
}
```

### Configuration Options

| Option             | Type    | Required | Default | Description |
|--------------------|---------|----------|---------|-------------|
| `name`             | string  | Yes      | —       | Name of the accessory as it appears in HomeKit |
| `doorRelayPin`     | integer | Yes      | —       | GPIO pin number (board numbering) connected to the relay |
| `doorSensorPin`    | integer | Yes      | —       | GPIO pin number (board numbering) connected to the door sensor |
| `duration_ms`      | integer | No       | `0`     | How long (in milliseconds) to hold the relay closed. Set to `0` to hold indefinitely |
| `invertDoorState`  | boolean | No       | `false` | Invert the relay output logic (HIGH/LOW) for the door |
| `invertSensorState`| boolean | No       | `false` | Invert the sensor reading logic |
| `input_pull`       | string  | No       | `"none"`| Internal pull resistor for sensor pin: `"up"`, `"down"`, or `"none"` |

---

## Wiring

### Relay (door trigger)
- Connect the relay signal wire to the GPIO pin specified by `doorRelayPin`.
- Connect relay COM/NO terminals in parallel with the garage door button.

### Door Sensor (optional but recommended)
- Connect one leg of the sensor to the GPIO pin specified by `doorSensorPin`.
- Connect the other leg to ground (if using `input_pull: "up"`) or to 3.3V (if using `input_pull: "down"`).

> **Tip:** Use the Raspberry Pi board pin numbers (not BCM numbers) when configuring `doorRelayPin` and `doorSensorPin`.

---

## Running Homebridge

Start Homebridge normally — the plugin will be loaded automatically:

```bash
homebridge
```

Or with the service manager (if configured as a service):

```bash
sudo systemctl start homebridge
```

To view logs:

```bash
sudo journalctl -fu homebridge
```

---

## Troubleshooting

- **"You must provide a config value for 'doorRelayPin'"** — Ensure `doorRelayPin` is set in your `config.json`.
- **"You must provide a config value for 'doorSensorPin'"** — Ensure `doorSensorPin` is set in your `config.json`.
- **Relay fires but door doesn't move** — Check your wiring and try toggling `invertDoorState`.
- **Sensor always shows wrong state** — Try toggling `invertSensorState` or changing `input_pull`.
- **Permission denied on GPIO** — Run Homebridge as root or ensure the user is in the `gpio` group: `sudo usermod -aG gpio $USER`

---

## License

ISC

## Author

skirkpatrick88
