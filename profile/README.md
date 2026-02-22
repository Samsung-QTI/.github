# Samsung-QTI 

AOSP Development for the **Samsung Galaxy Tab A9+ WiFi (SM-X210)** and **Galaxy Tab A9+ 5G (SM-X216)**, based on the Qualcomm Blair SoC (SM6375) and CODELINARO build configuration 

Previously supported on Holi under LineageOS 22, now migrated to the **Blair platform base** targeting Android 16 


## Devices

|                     | Model   | Codename    | Chipset ID            | 
| ------------------- | ------- | ----------- | ----------------------|
| Galaxy Tab A9+ WiFi | SM-X210 | `gta9pwifi` | P86803AA1             |
| Galaxy Tab A9+ 5G   | SM-X216 | `gta9p`     | P86801AA1 | P86801EA1 |



Kernel: Linux 6.1 GKI
Build system: Bazel / Kleaf (DDK)


**SoC**: Snapdragon 695 5G (SM6375) · 
**Kernel**: Linux 6.1 GKI · **Build system**: Bazel / Kleaf (DDK)

---

## Platform Overview

The so called **Blair-SEC** platform is based on the **LA.VENDOR.14.3.1.r1-04500-MANNAR.QSSI16.0** release tag with BSP Samsung specific additions for display, USB, and touch hardware

```
LineageOS AOSP
      │
      ├── device/samsung/gta9pwifi        ← device tree
      ├── device/qcom/common              ← QC common (forked)
      ├── hardware/qcom/display           ← display HAL
      ├── vendor/qcom/opensource/*        ← BSP OSS HALs
      │
      └── kernel_platform/
            ├── msm-kernel                ← Blair GKI kernel (6.1)
            └── vendor/qcom/opensource/*  ← DDK external modules
```

**Build baseline**:
```
LA.QSSI.16.0.r1-10900-qssi.0
LA.VENDOR.14.3.1.r1-04500-MANNAR.QSSI16.0
KERNEL.PLATFORM.3.0.r19-02500-kernel.0
```

---

## Build System

The platform uses a **three-phase build pipeline**:

- **Phase 1** — Kernel: `build.sh` → `prepare_vendor.sh` → Bazel/Kleaf → `blair_gki` target → staged to `device/qcom/blair-kernel`
- **Phase 2** — DDK modules: `build-ddk.sh` → `build_module.sh` → Bazel DDK per BSP module, dependency-staged
- **Phase 3** — Android image: `mka bacon` → AOSP Make → `vendor_dlkm.img` / `system_dlkm.img`


---
| Org | Contents |
|-----|----------|
| [Samsung-QTI](https://github.com/Samsung-QTI) | BSP HALs, display, audio, platform common |
| [Samsung-QTI-Blair](https://github.com/Samsung-QTI-Blair) | DDK kernel modules, kernel config, build scripts |
