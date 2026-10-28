# MyOri NFC Station

Desktop app for the MyOri SmartSecure production line: it writes, reads and locks NFC tags at four
stations (Writer, Reader, Locker 1, Locker 2) and can signal the line's PLC.

This repository only holds the installers. Installed copies of the app check it for new versions
and update themselves.

## Download

Go to **[Releases → Latest](https://github.com/faizulMyori/myori-nfc-station-download/releases/latest)**
and download the file ending in **`_x64-setup.exe`**.

The `.msi` file is the same app, for IT departments that deploy software with Group Policy. The
`.sig` and `latest.json` files are used by the app's updater; you don't need them.

## Requirements

- Windows 10 or 11, 64-bit
- A PC/SC NFC reader (for example ACS ACR1252U or ACR122U) with its driver installed
- NTAG213 tags
- For PLC signalling: the PLC connected to a COM port (optional)

The installer adds Microsoft Edge WebView2 if the PC doesn't have it yet.

## Install

1. Run the `_x64-setup.exe` you downloaded.
2. If Windows shows **"Windows protected your PC"**, click **More info**, then **Run anyway**.
   The installer isn't code-signed with a commercial certificate yet, so Windows doesn't recognise
   it.
3. Follow the steps. The app appears in the Start menu as **MyOri NFC Station**.

## First start

1. **Encryption key.** Enter the 32-character key in the *Encryption key* panel and click
   **Save key**. Get the key from your supervisor; it is never shared in this repository. It is
   saved on this PC, so you only enter it once. Until a key is saved, the Writer station stays
   locked.
2. **Readers.** Plug in the NFC readers, then choose a reader for each station from its dropdown.
3. **Write settings.** Fill in the initials, start number and total, then click **Set Write Data**
   before starting the Writer.

### PLC (factory PCs only)

The PLC is off by default. To turn it on, close the app and edit
`%APPDATA%\my.myori.nfcstation\config.json`:

```json
{ "plc_enabled": true, "plc_port": "COM9", "plc_baud": 9600 }
```

Set `plc_port` to the COM port the PLC uses, save, and start the app again. The top bar shows the
PLC status.

## Updates

The app checks for a new version every time it starts. When one is available, the button beside
the logo changes to **Download x.y.z**. Click it, confirm, and the app downloads the update,
installs it and reopens. You can also click **Check for updates** at any time.

Processed tags are saved before updating, so nothing is lost.

## Your data

Everything stays on the PC, in `%APPDATA%\my.myori.nfcstation\`:

| File | What it holds |
| --- | --- |
| `station.db` | the saved encryption key and every processed tag, grouped by session |
| `config.json` | PLC settings |

Past sessions can be downloaded again as Excel from the **Sessions** panel. If this folder is still
there after uninstalling, delete it by hand to remove the data.

## Uninstall

**Settings → Apps → Installed apps → MyOri NFC Station → Uninstall.**

## Problems

- **No readers in the dropdown:** check the reader is plugged in and its driver is installed.
  Readers show up a moment after they are plugged in.
- **"Reader disconnected" on a station:** the reader was unplugged. Plug it back in and the
  station carries on.
- **"Could not check for updates":** the PC has no internet connection. The app works offline;
  only updates need the internet.
- **Excel won't save:** close the file if it is open in Excel, then try again.
