# android_vendor_zte_a2015

Proprietary blobs and firmware for the ZTE Axon A2015 (MSM8994), used by the
LineageOS 22.2 Wi-Fi-only port.

## Status

Empty scaffold. Blobs must be extracted from the stock ROM that matches the
device's current firmware.

## Extraction

1. Get the stock ROM (fastboot package or flashable zip) for the exact build.
2. Mount/extract `system.img` (and `vendor` if present).
3. Run the device tree's `extract-files.py` against a device running the stock
   ROM, or copy the files listed in `proprietary-files.txt` manually.
4. Fix up `a2015-vendor.mk` with the resulting `PRODUCT_COPY_FILES` /
   `PRODUCT_PACKAGES`.

## Wi-Fi-only scope

Do not include RIL / telephony blobs:

- `libril-qc-qmi-1.so`
- `libqcrilMarshal.so`
- `qcrild`
- IMS / VoLTE libraries
- `libqti-perfd` telephony bits

The build sets `ro.radio.noril=1`, so telephony services are not started.

## Minimum blob groups for first boot

- Wi-Fi/BT: WCNSS firmware, `libwcnss*`, `libqcbassboost`, `libcsd-client`
- Display/GPU: Adreno firmware, `libgsl`, `libllvm-glnext`
- Audio: AK4961 / WCD9330 related libs (optional for first boot)
- Sensors: `libsensor*` (optional)
- Camera: `libmmcamera*` (optional)
