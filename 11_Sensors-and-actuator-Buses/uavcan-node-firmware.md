# UAVCAN 固件升级

# UAVCAN Firmware Upgrading

## Vectorcontrol ESC Codebase (Pixhawk ESC 1.6 and S2740VC)

Download the ESC code:

<div class="host-code"></div>

```sh
git clone https://github.com/thiemar/vectorcontrol
cd vectorcontrol
```

### Flashing the UAVCAN Bootloader

Before updating firmware via UAVCAN, the Pixhawk ESC 1.6 requires the UAVCAN bootloader be flashed. To build the bootloader, run:

<div class="host-code"></div>

```sh
make clean && BOARD=px4esc_1_6 make -j8
```

After building, the bootloader image is located at `firmware/px4esc_1_6-bootloader.bin`, and the OpenOCD configuration is located at `openocd_px4esc_1_6.cfg`. Follow [these instructions](uavcan-bootloader-installation.md) to install the bootloader on the ESC.

### Compiling the Main Binary

<div class="host-code"></div>

```sh
BOARD=s2740vc_1_0 make && BOARD=px4esc_1_6 make
```

This will build the UAVCAN node firmware for both supported ESCs. The firmware images will be located at `com.thiemar.s2740vc-v1-1.0-1.0.<git hash>.bin` and `org.pixhawk.px4esc-v1-1.6-1.0.<git hash>.binn`.

## Sapog Codebase (Pixhawk ESC 1.4)

Download the Sapog codebase:

<div class="host-code"></div>

```sh
git clone https://github.com/PX4/sapog
cd sapog
git submodule update --init --recursive
```

### Flashing the UAVCAN Bootloader

Before updating firmware via UAVCAN, the Pixhawk ESC 1.4 requires the UAVCAN bootloader be flashed. The bootloader can be built as follows:

<div class="host-code"></div>

```sh
cd bootloader
make clean && make -j8
cd ..
```

The bootloader image is located at `bootloader/firmware/bootloader.bin`, and the OpenOCD configuration is located at `openocd.cfg`. Follow [these instructions](uavcan-bootloader-installation.md) to install the bootloader on the ESC.

### Compiling the Main Binary

<div class="host-code"></div>

```sh
cd firmware
make sapog.image
```

The firmware image will be located at `firmware/build/org.pixhawk.sapog-v1-1.0.<xxxxxxxx>.bin`, where `<xxxxxxxx>` is an arbitrary sequence of numbers and letters.

## Zubax GNSS

Please refer to the [project page](https://github.com/Zubax/zubax_gnss) to learn how to build and flash the firmware.
Zubax GNSS comes with a UAVCAN-capable bootloader, so its firmware can be updated in a uniform fashion via UAVCAN as described below.

## Firmware Installation on the Autopilot

The UAVCAN node file names follow a naming convention which allows the Pixhawk to update all UAVCAN devices on the network, regardless of manufacturer. The firmware files generated in the steps above must therefore be copied to the correct locations on an SD card or the PX4 ROMFS in order for the devices to be updated.

The convention for firmware image names is:

  ```<uavcan name>-<hw version major>.<hw version minor>-<sw version major>.<sw version minor>.<version hash>.bin```

  e.g. ```com.thiemar.s2740vc-v1-1.0-1.0.68e34de6.bin```

However, due to space/performance constraints (names may not exceed 28 charates), the UAVCAN firmware updater requires those filenames to be split and stored in a directory structure like the following:

  ```/fs/microsd/fw/<node name>/<hw version major>.<hw version minor>/<hw name>-<sw version major>.<sw version minor>.<git hash>.bin```

 e.g. ```s2740vc-v1-1.0.68e34de6.bin```

The ROMFS-based updater follows that pattern, but prepends the file name with ```_``` so you add the firmware in:

  ```/etc/uavcan/fw/<device name>/<hw version major>.<hw version minor>/_<hw name>-<sw version major>.<sw version minor>.<git hash>.bin```

## Placing the binaries in the PX4 ROMFS

The resulting finale file locations are:

- S2740VC ESC: `ROMFS/px4fmu_common/uavcan/fw/com.thiemar.s2740vc-v1/1.0/_s2740vc-v1-1.0.<git hash>.bin`
- Pixhawk ESC 1.6: `ROMFS/px4fmu_common/uavcan/fw/org.pixhawk.px4esc-v1/1.6/_px4esc-v1-1.6.<git hash>.bin`
  - Pixhawk ESC 1.4: `ROMFS/px4fmu_common/uavcan/fw/org.pixhawk.sapog-v1/1.4/_sapog-v1-1.4.<git hash>.bin``
  - Zubax GNSS v1: `ROMFS/px4fmu_common/uavcan/fw/com.zubax.gnss/1.0/gnss-1.0.<git has>.bin`
  - Zubax GNSS v2: `ROMFS/px4fmu_common/uavcan/fw/com.zubax.gnss/2.0/gnss-2.0.<git has>.bin`

Note that the ROMFS/px4fmu_common directory will be mounted to /etc on Pixhawk.

### Starting the Firmware Upgrade process

<aside class="note">
When using the [PX4 Flight Stack](concept-flight-stack.md), enable UAVCAN in the 'Power Config' section and reboot the system before attempting an UAVCAN firmware upgrade.
</aside>

Alternatively UAVCAN firmware upgrading can be started manually on NSH via:

```sh
uavcan start
uavcan start fw
```