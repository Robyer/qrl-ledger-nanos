# QRL App Manual Installation for Ledger Nano S (Firmware 2.x.x)

A guide for manually installing the QRL application on Ledger Nano S devices with firmware 2.x.x, which no longer officially support it through Ledger Live.

## ⚠️ Important Warnings

- **This guide is for Windows only.** While it's possible to make this work on Linux or Mac, different steps are required.
- **This process requires access to your unlocked Ledger Nano S device** (so you need to know the PIN code).
- **Basic command line knowledge is required.**

## Alternative Solution

> **💡 Note:** If you don't have access to your Ledger device but have your 24-word BIP39 recovery phrase, you can use the [QRL Ledger Recovery Tool](https://github.com/Robyer/qrl-ledger-recovery) to extract your QRL keys directly. Note that this method requires exposing your recovery phrase to a computer and should only be used if you understand the security implications.



## Prerequisites

Before starting, ensure you have:

- [ ] Ledger Nano S device with firmware 2.x.x
- [ ] USB cable to connect Ledger to computer
- [ ] PIN code for your Ledger device
- [ ] Basic familiarity with command line/terminal
- [ ] Sufficient free space on your Ledger (uninstall other apps if needed)



## Installation Steps

### Step 1: Install Python 3.12.2

> **Note:** Python 3.14 (latest version) does not work with this process. You must use Python 3.12.2.

1. Download Python 3.12.2 from the official source:  
   **Direct link:** https://www.python.org/ftp/python/3.12.2/python-3.12.2-amd64.exe

2. Run the installer

3. **IMPORTANT:** Check the box **"Add python.exe to PATH"** at the bottom of the installer screen

4. Click **"Install Now"** at the top

5. Verify the installation:
   - Open Terminal (Command Prompt)
   - Run: `python --version`
   - You should see: `Python 3.12.2`

### Step 2: Install Microsoft C++ Build Tools

In the next step, we'll install `ledgerblue`, a Python tool that allows communication with Ledger devices. This tool requires Microsoft Visual C++ 14.0 or greater to compile some of its native components.

1. Download Microsoft C++ Build Tools:  
   **Direct link:** https://aka.ms/vs/17/release/vs_BuildTools.exe  
   **Info page:** https://visualstudio.microsoft.com/visual-cpp-build-tools/

2. Run the installer

3. Install using the default predefined options (do not change anything)

4. Wait for the installation to complete (this may take several minutes)

### Step 3: Set Up Python Environment and Install ledgerblue

1. Open Terminal (Command Prompt)

2. You should see your prompt in your user directory:
   ```
   C:\Users\YOUR_USERNAME>
   ```

3. Create a new Python virtual environment:
   ```bash
   python -m venv qrl
   ```
   This creates a folder named `qrl` in your current directory.

4. Activate the virtual environment:
   ```bash
   qrl\Scripts\activate
   ```

5. If successful, your prompt will change to:
   ```
   (qrl) C:\Users\YOUR_USERNAME>
   ```
   The `(qrl)` prefix indicates the environment is active.

6. Install the `ledgerblue` package:
   ```bash
   pip install ledgerblue
   ```

7. Wait for the installation to complete. This will likely take several minutes as it downloads and compiles the program and its dependencies. You should see a success message when finished.

### Step 4: Verify ledgerblue and Ledger Communication

> **Important:** Make sure Ledger Live is not running before proceeding. Close it if it's open.

1. **Connect your Ledger Nano S** to your computer via USB

2. **Unlock your device** by entering your PIN

3. **Stay on the dashboard** (do not open any application)

4. Test that ledgerblue works and can communicate with your Ledger:
   ```bash
   python -m ledgerblue.checkGenuine --targetId 0x31100004
   ```

5. **On your Ledger device:** You will see a prompt asking you to confirm use of the **unsafe manager**. Navigate to "Allow" and confirm.

6. **In the terminal:** You should see output similar to:
   ```
   Product is genuine
   ...version information...
   ```

If you see this, the connection is working correctly and you can proceed to the next step.

### Step 5: Download and Prepare QRL App Binary

1. Download the QRL app binary archive from Releases page:  
   **Direct Link:** https://github.com/Robyer/qrl-ledger-nanos/releases/download/qrl-1.4.4-nanos/qrl-ledger-nanos.zip

2. Extract the downloaded ZIP archive

3. Inside the archive, you'll find a `bin` folder containing 4 files:
   - `app.apdu`
   - `app.elf`
   - `app.hex`
   - `app.sha256`

4. Copy the entire `bin` folder to your Python environment directory:
   ```
   C:\Users\YOUR_USERNAME\qrl\bin\
   ```

5. Verify the files are in the correct location:
   ```
   C:\Users\YOUR_USERNAME\qrl\bin\app.apdu
   C:\Users\YOUR_USERNAME\qrl\bin\app.elf
   C:\Users\YOUR_USERNAME\qrl\bin\app.hex
   C:\Users\YOUR_USERNAME\qrl\bin\app.sha256
   ```

### Step 6: Install QRL App on Ledger

1. Ensure your Ledger is still:
   - Connected to the computer
   - Unlocked (PIN entered)
   - On the home screen/dashboard

2. Run the installation command:
   ```bash
   python -m ledgerblue.runScript --scp --fileName qrl/bin/app.apdu --elfFile qrl/bin/app.elf
   ```

3. **On your Ledger device:** You will be prompted to confirm the installation. Review and approve the prompts.

4. Wait for the installation to complete. You should see success messages in the terminal.

5. The QRL app should now appear in your Ledger's app list.

### Step 7: Initialize and Use QRL App

1. **On your Ledger device:** Open the newly installed QRL application

2. **Initialize your XMSS tree(s)** following the on-screen prompts on the Ledger

3. **After initialization is complete:**
   - Launch the **QRL Wallet** desktop application on your computer
   - Go to **"Open Wallet"** section
   - Select **"Ledger Nano"**
   - Click the **"OPEN LEDGER NANO"** button

4. **Set the correct OTS key index:**
   - In QRL Wallet, go to the **"Tools"** section
   - Click on **"Set XMSS Index"**
   - Set the suggested index (loaded from blockchain) to your Ledger device
   - This ensures you don't reuse keys

5. **Transfer your QRL funds:**
   - Create a new QRL wallet (desktop/mobile/web wallet)
   - **Save the new wallet's mnemonic phrase securely**
   - Transfer all QRL from your Ledger to the new wallet

> **⚠️ CRITICAL - Memory Freeze Issue:**  
> Due to memory restrictions on Ledger Nano S, the device may freeze on the transaction confirmation screen. **To avoid this, you must confirm the transaction WITHOUT scrolling through the whole address.** This is a known issue with Ledger S memory limitations.

> **Recommendation:** After successfully moving your funds, consider upgrading to a newer Ledger device (Nano S Plus or Nano X) that officially supports current firmware and applications.



## Known Issues

### Issue 1: Python Not Found

**Error message:**
```
'python' is not recognized as an internal or external command
```

**Solution:**  
1. You likely forgot to check "Add python.exe to PATH" during Python installation
2. Uninstall Python
3. Reinstall Python 3.12.2 and make sure to check the PATH checkbox
4. Restart your terminal after reinstallation



### Issue 2: Microsoft Visual C++ Error

**Error message:**
```
error: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools"
```

**Solution:**  
Ensure you have completed Step 2 (Install Microsoft C++ Build Tools) before running `pip install ledgerblue`. If you've already installed the build tools, try restarting your terminal and running the pip command again.



### Issue 3: Invalid Status 5103

**Error message:**
```
ledgerblue.commException.CommException: Exception : Invalid status 5103 (Unknown reason)
```

**Meaning:**  
Status code 5103 indicates your Ledger device does not have enough free storage space for the QRL app installation.

**Solution:**  
1. On your Ledger device, uninstall other applications you don't immediately need
2. Free up at least 200 KB of space or more
3. Try the installation command again

You can reinstall other apps later after you've moved your QRL funds.



## FAQ

**Q: Why can't I just use Ledger Live?**  
A: Ledger has deprecated support for firmware 2.x.x on Nano S devices. The QRL app is no longer available through Ledger Live for these devices.

**Q: Is this process safe?**  
A: Yes, when following these steps. You are manually installing a precompiled QRL app binary (with source code modified to work with latest firmware) using the standard ledgerblue installation tool.

**Q: Will this work on Linux or Mac?**  
A: While it's possible to make this work on Linux or Mac, different steps are required, particularly for Python environment setup, build tools, and Ledger device drivers. This guide is only tested on Windows.

**Q: What if I don't have my PIN anymore?**  
A: If you don't have your PIN but have your 24-word BIP39 recovery phrase, use the [QRL Ledger Recovery Tool](https://github.com/Robyer/qrl-ledger-recovery) instead. Be aware of the security implications of exposing your recovery phrase to a computer.

**Q: Can I keep using my Ledger with QRL after this?**  
A: The app will work normally on current firmware, but it's not recommended for long-term use. Future Ledger firmware updates could break support for this app even further. It's important to keep your mnemonic phrase safe so you can always access your coins later. Consider moving funds to a new wallet or newer Ledger device.



## 🎁 Donate

If you found this guide helpful, you can donate to my QRL address:

`Q0105005f60f40b22fe9034c68ab85a3081c86ad7cc644bd1fa2075f87bac1e9d838a9640c08040`



## Resources

- [QRL Website](https://www.theqrl.org)
- [QRL Docs - Ledger Wallet](https://docs.theqrl.org/use/wallet/ledger)



## Disclaimer

This is an unofficial guide for recovering access to QRL funds on unsupported Ledger devices. Use at your own risk. The app will work on current firmware but future updates may break compatibility. Always maintain secure backups of your recovery phrases.