# Projectivy Fast Home for Fire TV

Fast, ad-free Home-button redirection to [Projectivy Launcher](https://github.com/spocky/miproja1) on older Amazon Fire TV devices, without root.

**Tested:** Fire TV Stick 4K (2018), model AFTMM (`mantis`), Fire OS 6.7.1.1. The fast Home redirect and Synology-assisted recovery were verified on 24 October 2026. Other devices and firmware versions have not been tested.

## Download

**[Download the verified v5 source package](downloads/Projectivy-Fast-Home-v5-source.tar.xz)**

This is source code, **not a prebuilt APK**. Extract with `tar -xJf Projectivy-Fast-Home-v5-source.tar.xz`. The package contains the Android source, a local APK build script, setup/stop/remove scripts, and Synology recovery documentation. You will need Android build tools and a Java JDK to build the APK yourself.

## How it works

1. A resident Android service watches Fire OS system logs for the Home event (via an ADB-granted `READ_LOGS` permission).
2. A pre-created black overlay conceals the Amazon Home transition.
3. A loopback-only shell-UID helper brings Projectivy forward quickly.
4. The overlay disappears when Projectivy gains focus, with a safety timeout.

No root, disabling Amazon system packages, or Internet-facing ports are required. This does not remove Amazon's operating-system advertising; it bypasses the Amazon Home experience during normal navigation.

## Install

1. Install Projectivy Launcher on the Fire TV and enable ADB debugging.
2. Authorise your computer with `adb connect YOUR_FIRE_TV_IP:5555`.
3. Extract the source package, then from its `projectivy-fast-home` folder:

```bash
cp .env.example .env
# Edit .env to contain FIRETV_DEVICE=YOUR_FIRE_TV_IP:5555
./setup.sh
```

`setup.sh` builds and signs the APK locally, installs the app and starts the on-device shell helper. The build script looks for a working Android SDK and Java JDK. The default script was verified in the original development environment; other build environments may require setting `ANDROID_SDK_ROOT` and `JAVA_HOME`.

To verify or troubleshoot:

```bash
./diagnose.sh
```

## Surviving reboots

The privileged shell process does not survive a Fire TV reboot. You need an authorised ADB client on an always-on computer or NAS to restart it. A Synology NAS running an every-minute scheduled recovery task was successfully tested after a real Fire TV reboot.

See [Synology setup instructions](synology/README.md) **inside the source package**. The generic host script is at `host/ensure-running.sh`. macOS restart support is optional; a Mac is not necessary if you use a NAS.

## Restore Amazon Home

```bash
./stop.sh
```

To completely remove the helper:

```bash
./remove.sh
```

Disable your external scheduled recovery job before removing the helper. Neither command removes Projectivy nor changes protected Amazon packages.

## Important limitations

- Compatibility has only been verified with the tested Fire OS 6 device.
- Fire OS updates may break this method.
- The app requires system log and overlay permissions, so review the source before sideloading.
- No prebuilt APK is provided; each local build is signed with your own key.
- Keep local `.env`, ADB keys, signing keys and device logs private.

## Credits and licence

Original project code is MIT-licensed; the source package also includes the third-party MIT notice for shell-helper techniques adapted from [tv-vibehack-firetv](https://github.com/pulgagr/tv-vibehack-firetv).

Projectivy Launcher is a separate project and must be installed independently.
