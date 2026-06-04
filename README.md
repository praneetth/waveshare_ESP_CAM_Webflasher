# Waveshare ESP32‑S3‑CAM Web Flasher & Setup

This repository provides a **browser‑based firmware flasher** and setup guide for the **Waveshare ESP32‑S3‑CAM‑OVxxxx** board.

It uses **ESP Web Tools** (Web Serial) to flash a pre‑built firmware image.  
After flashing, the firmware starts in Wi‑Fi configuration mode and exposes a local web portal where you can enter your Wi‑Fi credentials and get the **Device ID** used by the **Xiaozhi control panel**.

---

## 1. Requirements

- Waveshare **ESP32‑S3‑CAM‑OVxxxx** board
- USB‑C / micro‑USB cable (data capable)
- A desktop browser with **Web Serial API** support:
  - Recommended: **Google Chrome** or **Microsoft Edge** (latest versions)
- Internet connection (to load this GitHub Pages site and firmware files)

---

## 2. Flashing the Firmware (Web Installer)

1. **Open the Web Flasher page**

   Navigate to the GitHub Pages URL for this repo, for example:

   ```text
   https://<your-username>.github.io/waveshare_ESP_CAM_Webflasher/
   ```

   You should see a page titled **“ESP32‑S3‑CAM Web Flasher”** with a **“Connect & Flash ESP32‑S3‑CAM”** button.

2. **Connect the board**

   - Plug the Waveshare ESP32‑S3‑CAM into your PC using a USB cable.
   - If your board requires it, enter **bootloader mode**:
     - Press and hold **BOOT**.
     - Tap the **EN/RESET** button.
     - Release **BOOT**.

3. **Start flashing**

   - Click **Connect & Flash ESP32‑S3‑CAM**.
   - In the popup, select the correct serial/COM port for the board and click **Connect**.
   - The flasher will:
     - Erase flash (if you confirmed erase).
     - Write all `.bin` files to the correct offsets (bootloader, partition table, OTA data, main firmware, assets).
   - Wait until the progress reaches 100% and the tool reports success.

4. **Reboot the board**

   - Press **EN/RESET** or power‑cycle the board.
   - The firmware will now start in **Wi‑Fi configuration mode**.

---

## 3. Wi‑Fi Configuration Portal (AP Mode)

After a successful flash and reboot:

1. **Board creates its own Wi‑Fi access point**

   - The ESP32‑S3‑CAM starts in **Access Point (AP)** mode.
   - It will broadcast a Wi‑Fi network (SSID) such as:

     ```text
     Xiaozhi-xxxxxx   (example)
     ```

   - The exact SSID depends on your firmware; check your project docs or Serial Monitor if needed.

2. **Connect to the AP**

   - On your phone or laptop, open **Wi‑Fi settings**.
   - Find and connect to the ESP32‑S3‑CAM AP SSID.
   - If a password is required, use the one specified in your firmware documentation; otherwise it may be open.

3. **Open the configuration page**

   - Once connected to the AP, open a browser and go to:

     ```text
     http://192.168.4.1
     ```

     (Default ESP32 soft‑AP IP is `192.168.4.1`.)

   - You should see a configuration page asking for **Wi‑Fi credentials**.

4. **Enter your Wi‑Fi credentials**

   - In the portal, fill in:
     - **Wi‑Fi SSID** (your home/office router name)
     - **Wi‑Fi password**
   - Click **Save** or **Apply**.

5. **Device saves credentials and restarts**

   - The ESP32 stores the Wi‑Fi credentials in flash.
   - It reboots into **station mode** and attempts to connect to your router.
   - On success, the device will register itself and display (or return) a **Device ID** in the portal.

---

## 4. Getting the Device ID

After you save Wi‑Fi credentials:

1. The configuration page will show a **Device ID** assigned to this ESP32‑S3‑CAM.
2. Note this Device ID carefully; you will need it to control the device from the **Xiaozhi control panel**.
3. If you miss it, you can usually:
   - Reconnect to the device portal at `http://192.168.4.1` while it is still in AP/provisioning mode, or
   - Check Serial Monitor logs (depending on how the firmware is implemented).

---

## 5. Xiaozhi Control Panel Setup

Once you have the Device ID:

1. Open the **Xiaozhi control panel** in your browser.
2. Locate the field for **Device ID** (or similar) and enter the Device ID shown by the ESP32 configuration portal.
3. Save/apply the settings in the Xiaozhi panel.
4. When the ESP32‑S3‑CAM is connected to your Wi‑Fi, the control panel will now be able to communicate with this device using that ID.

You can now:

- Configure the **role** / **personality** / **model** of your AI assistant.
- Adjust any application‑specific options exposed in the Xiaozhi interface (e.g., wake words, camera options, response style, etc.).

---

## 6. Typical Workflow Summary

1. **Flash** firmware via the Web Flasher page.
2. **Reboot** the board; it enters AP mode.
3. **Connect** your phone/PC to the board’s Wi‑Fi AP.
4. **Open** `http://192.168.4.1` in a browser.
5. **Enter** your Wi‑Fi SSID and password and **Save**.
6. **Read and note** the **Device ID** from the portal.
7. **Open** Xiaozhi control panel, **enter Device ID**, and configure your AI assistant role/model.
8. Start using the ESP32‑S3‑CAM with Xiaozhi.

---

## 7. Troubleshooting

- **Web flasher cannot detect the board**
  - Use Chrome/Edge on desktop, not mobile.
  - Check USB cable (must support data).
  - Ensure the board is in bootloader mode if flashing fails.
- **Cannot see the AP SSID**
  - Confirm the firmware flashed successfully.
  - Power‑cycle the board and watch the Serial Monitor for status messages.
- **Cannot open `192.168.4.1`**
  - Confirm you are connected to the ESP32 AP, not your home Wi‑Fi.
  - Try a different browser; avoid VPNs or captive‑portal Wi‑Fi.
- **Device does not connect to home Wi‑Fi**
  - Double‑check SSID and password.
  - Make sure the 2.4 GHz band is enabled on your router (ESP32 does not support 5 GHz).

---

This README is intended for end users of your web flasher.  
You can customize the Wi‑Fi AP name, passwords, or any Xiaozhi‑specific text to match your actual firmware behavior.
