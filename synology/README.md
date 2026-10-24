# Synology NAS automatic recovery

The Fire TV shell helper stops on reboot. A Synology NAS with an authorised ADB connection can restart it automatically. This recovery arrangement was verified on Fire OS 6.7.1.1 on 24 October 2026.

1. Install the Linux Android platform-tools on your NAS and confirm `adb version` works.
2. From SSH as your chosen DSM task user, run `adb connect FIRETV_IP:5555` and accept **Always allow** on the TV. Confirm `adb devices -l` reports `device`, not `offline`.
3. Extract the source package and copy it to the NAS or another stable NAS path.
4. Test a manual recovery, replacing the addresses and paths:

```bash
FIRETV_DEVICE=192.168.1.50:5555 ADB_BIN=/path/to/adb /bin/bash /path/to/projectivy-fast-home/host/ensure-running.sh
```

5. In DSM **Control Panel → Task Scheduler**, create a **User-defined script** under the user account that authorised ADB. Schedule **Run on the following days → Daily**, from **00:00** to **23:59**, **Every minute**. Use the same command as above with your actual paths.
6. Reboot the Fire Stick. When it reconnects to Wi-Fi, the NAS should restore the Home redirect within the next scheduled check. Verify it by launching an app and pressing Home.

The portable host script is provided in the [source archive](../downloads/Projectivy-Fast-Home-v5-source.tar.xz). The installation has been verified on one NAS and Fire Stick combination only.

To stop automatic recovery, disable the DSM scheduled task. Do not expose ADB port 5555 outside your trusted LAN, and do not publish ADB keys or private settings.
