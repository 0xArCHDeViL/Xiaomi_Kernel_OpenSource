# Xiaomi Pad 7 (uke) Kernel Analysis

- **Finding**: CPU topology and cluster layout
- **Source**: Not found in source (no specific DT for uke, sm7675, or pineapple found in `arch/arm64/boot/dts/qcom`. The provided repo `android_device_xiaomi_uke` is an Android/LineageOS device tree, not a kernel device tree containing `.dts` files).
- **Status**: ❌ NOT FOUND
- **Notes**: The source contains generic build configs (`uke.bzl`, `xiaomi_sm8650_common.bzl`, `pineapple.bzl`), but specific kernel device tree sources for CPU topologies are missing in this codebase.

- **Finding**: CPU frequency tables (OPP tables or cpufreq driver)
- **Source**: `arch/arm64/configs/vendor/pineapple_GKI.config:21` (`CONFIG_ARM_QCOM_CPUFREQ_HW=m`)
- **Status**: ⚠️ PARTIAL
- **Notes**: The `qcom-cpufreq-hw` driver handles frequency scaling by querying the hardware/EPSS. Explicit OPP tables are not found in the source (usually populated via DT bindings or hardware/SCMI queries).

- **Finding**: CPU governor availability and default
- **Source**: `drivers/cpufreq/Kconfig:63`
- **Status**: ✅ CONFIRMED
- **Notes**: `CONFIG_CPU_FREQ_DEFAULT_GOV_SCHEDUTIL` selects `schedutil` as the default governor for ARM64. Other governors (performance, powersave, userspace, ondemand, conservative) are available depending on the configuration.

- **Finding**: GPU governor and frequency tables
- **Source**: `drivers/gpu/drm/msm/msm_gpu_devfreq.c:16` (devfreq framework) & `drivers/gpu/drm/msm/adreno/adreno_gpu.c:113` (queries `qcom,gpu-freq` via DT)
- **Status**: ⚠️ PARTIAL
- **Notes**: The GPU driver (`adreno`) uses `devfreq`. Explicit GPU freq tables (OPP) or specific governor tunings for Adreno 735 are missing due to the absence of the specific device tree and `adreno_device.c` not containing specific Adreno 73x definitions in this snapshot.

- **Finding**: Thermal zones and trip points
- **Source**: `drivers/thermal/qcom/`
- **Status**: ⚠️ PARTIAL
- **Notes**: Subsystem drivers like `bcl_pmic5.c`, `tsens.c`, and `cpu_hotplug.c` exist, but actual DT thermal zones and specific trip points are missing.

- **Finding**: WALT/schedutil tunables exposed via sysfs
- **Source**: `kernel/sched/walt/penalty.c:117`
- **Status**: ✅ CONFIRMED
- **Notes**: WALT sysfs attributes (e.g., related to penalty scaling) are dynamically created under the WALT parameter kobject.

- **Finding**: Power management subsystems present
- **Source**: `arch/arm64/configs/vendor/pineapple_GKI.config:39`
- **Status**: ✅ CONFIRMED
- **Notes**: PM subsystems like `CONFIG_CPU_IDLE_GOV_QCOM_LPM=m` and generic devfreq (`CONFIG_PM_DEVFREQ_EVENT=y` in GKI config) are confirmed present.

- **Finding**: uperf/PowerHAL hint paths
- **Source**: Not found in source
- **Status**: ❌ NOT FOUND
- **Notes**: There are no references to PowerHAL or uperf hints within this kernel codebase.
