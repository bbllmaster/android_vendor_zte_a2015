# android_vendor_zte_a2015

Proprietary blobs and firmware for the ZTE Axon A2015 (MSM8994), used by the
LineageOS 22.2 Wi-Fi-only port.

## Status

`a2015-vendor.mk` has been generated from the stock B16 ROM:

- 80 files extracted, about 83 MiB
- 2 optional 64-bit modules were not present:
  `system/lib64/hw/camera.msm8994.so`,
  `system/vendor/lib64/hw/sound_trigger.primary.msm8994.so`
- `proprietary/` is gitignored; the blobs stay local/private
- `a2015-vendor.mk` contains the `PRODUCT_COPY_FILES` mapping

## Stock ROM facts

- Package: `20161226142336257.zip` → `update.zip`
- Build: `ZTE_A2015V1.0.0B16_M` (Android 6.0.1, MMB29M)
- `system/vendor` is about 233 MiB; HAL modules are in
  `system/lib/hw` and `system/lib64/hw` (non-Treble layout).
- Wi-Fi/BT firmware: `system/etc/firmware/wlan/qca_cld/WCNSS_cfg.dat`,
  `system/etc/wifi/WCNSS_qcom_wlan_nv.bin`, `nvm_tlv*`, `rampatch_tlv*`.

## Extraction

1. Get the stock ROM (flashable zip) for the exact build.
2. Extract `update.zip`; it contains a `system/` tree and `boot.img`.
3. Run the device tree's extractor:

   ```bash
   cd device/zte/a2015
   ./extract-files.py --rom /path/to/update.zip --vendor ../../../vendor/zte/a2015
   ```

4. Review the generated `a2015-vendor.mk`; firmware destinations may need
   adjustment during bring-up.

> Legal note: the blobs are ZTE/Qualcomm proprietary. Do not publish them in a
> public repository without checking the redistribution terms; keep the vendor
> repo private or extract locally.

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
