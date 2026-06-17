# How to Pair Apple AirPods with a Raspberry Pi 3A+

Step-by-step guide to connect your Apple AirPods wirelessly to a Raspberry Pi 3A+ running Raspberry Pi OS (desktop or headless).

---

## Prerequisites

Ensure you have:

- A Raspberry Pi 3A+ with Raspberry Pi OS (desktop or Lite)
- Keyboard/mouse and monitor **or** SSH access
- Apple AirPods ready for pairing (case open, near the Pi)

![Raspberry Pi 3 with Bluetooth devices](https://d2u1z1lopyfwlx.cloudfront.net/thumbnails/0daf144f-dbce-5fa8-98ad-b186e4ba2669/d7de241d-b4d3-5b87-b906-6af31d77189d.jpg)

---

## 1. Prepare the Pi

Open a terminal on the Pi (or SSH in) and run:

```bash
sudo apt update
sudo apt full-upgrade -y
sudo apt install bluetooth bluez blueman pulseaudio pulseaudio-module-bluetooth -y
```

Then reboot:

```bash
sudo reboot
```

> **Note:** On recent Raspberry Pi OS, Bluetooth is often already enabled; this guarantees audio routing for headsets like AirPods.

---

## 2. Put AirPods in Pairing Mode

1. Open the AirPods case.
2. Press and hold the small button on the back of the case until the LED flashes white (discoverable mode).
3. Keep the case open and close to the Pi.

![Bluetooth headset pairing with Raspberry Pi](https://d2u1z1lopyfwlx.cloudfront.net/thumbnails/657ab5c1-ed0c-5b9b-ae64-f04f315bd3f7/48524073-e4fe-5181-9335-ca051989aed3.jpg)

---

## 3. Pair via GUI (Desktop)

1. Log in to the Raspberry Pi desktop.
2. Click the Bluetooth icon in the top-right panel and choose **Add Device…**
3. Wait for the Pi to scan; you should see **AirPods** appear.
4. Click it, then click **Pair**.
5. Once paired, select it from the Bluetooth menu to **Connect**.

![Raspberry Pi Bluetooth device list](https://d2u1z1lopyfwlx.cloudfront.net/thumbnails/36d60b1d-b01b-57b0-9c1a-c8885742912a/0a458188-d3bc-5c15-8873-a8ac2ee3d90c.jpg)

---

## 4. Pair via Command Line (Headless/SSH)

In a terminal or SSH session:

```bash
bluetoothctl
power on
agent on
default-agent
scan on
```

Wait until you see:

```
[NEW] Device XX:XX:XX:XX:XX:XX AirPods
```

Then:

```bash
pair XX:XX:XX:XX:XX:XX
connect XX:XX:XX:XX:XX:XX
quit
```

> **Note:** Replace `XX:XX:XX:XX:XX:XX` with the AirPods MAC address shown in the scan.

---

## 5. Set AirPods as Audio Output

**On desktop:**

- Click the speaker icon in the top bar → **Output device**
- Select **AirPods: A2DP Sink** (or similar) for stereo audio

**If AirPods don't appear, ensure:**

- `pulseaudio-module-bluetooth` is installed
- PulseAudio is running automatically

**For headless setups, force the sink:**

```bash
pactl list short sinks | grep bluez
pactl set-default-sink bluez_sink.XX_XX_XX_XX_XX_XX.a2dp_sink
```

> **Note:** Replace `bluez_sink.XX_XX_…` with the AirPods sink name shown by `pactl`.

---

## 6. Test the Connection

From a terminal:

```bash
speaker-test -c 2 -t wav
```

Or play a music file through your media player. If you hear audio in the AirPods, pairing and routing are working correctly.

---

## 7. Troubleshooting

- **AirPods connect but no sound:** Ensure you selected **A2DP** (not HSP/Mono) and that the default sink is correct.
- **Unstable Bluetooth:** A small USB Bluetooth 4.0+ dongle can improve reliability on Pi 3A+.
