| Component              | Hardware                      | Mainline status                                                  |
| ---------------------- | ----------------------------- | ---------------------------------------------------------------- |
| SoC                    | Exynos 9810                   | ✅ `arch/arm64/boot/dts/exynos/exynos9810.dtsi`<br>✅ `arch/arm64/boot/dts/exynos/exynos9810-starlte.dts`<br>✅ `arch/arm64/boot/dts/exynos/exynos9810-pinctrl.dtsi`  |
| CPU                    | 4 x Exynos M3 "Meerkat" (ARMv8.0)</br>4 x Cortex-A55 (ARMv8.2)| ✅ |
| GPU                    | Mali-G72 MP18                 | ✅ `drivers/gpu/drm/panfrost/`                                  |
| Display Panel	         | Samsung AMB622NP01            | ✅                                                             |
| Touchscreen controller | Samsung S6SY761X              | ✅ `drivers/input/touchscreen/s6sy761.c`                        |
| Display Driver Integrated Circuit | Samsung S6E3HA8    | ✅ `drivers/gpu/drm/panel/panel-samsung-s6e3ha8.c`              |
| Wi-Fi / Bluetooth      | Broadcom BCM43570             | ✅ `drivers/net/wireless/broadcom/brcm80211/brcmfmac/`          |
| FM transceiver / Tuner | RichWave RTC6213N             | ❌                                                     |
| NFC module name	       | Samsung 82LBXS2               | ❌                                                     |
| NFC controller         | Samsung S3NRN82               | ✅ `drivers/nfc/s3fwrn5`                                         |
| NFC secure element     | Samsung S3FV9RRP              | ❌                                                     |
| GNSS                   | Broadcom BCM47752             | ❌                                                     |
| Modem / RF transceiver | Shannon 965                   | ❌                                                     |
| Front-End Module       | AFEM-9090                     | ❌                                                     |
| Power amplifier        | Skyworks SKY77365-11          | ❌                                                     |
| Power amplifier        | Murata fL05B                  | ❌                                                     |
| Envelope tracking      | Shannon 735                   | ❌                                                     |
| PMIC                   | MAX77705F                     | ✅ `drivers/mfd/max77705.c`<br>✅ `drivers/regulator/max77705-regulator.c`<br>✅ `drivers/power/supply/max77705_charger.c`<br>✅ `drivers/power/supply/max17042_battery.c` |
| PMIC for modem         | Shannon 560                   | ❌                                                     |
| PMIC for display       | Samsung S2DOS05               | ✅ `drivers/regulator/s2dos05-regulator.c`                       |
| PMIC for camera        | Samsung S2MPB02               | ❌                                                     |
| Wireless charging      | IDT P9320S                    | ❌                                                     |
| 6-Axis Gyroscope & Accelerometer | STMicroelectronics LSM6DSL | ✅ `drivers/iio/imu/st_lsm6dsx`                           |
| 3-Axis Electronic Compass / Magnetometer | AKM AK09916 | ✅ `drivers/iio/magnetometer/ak8975.c`                              |
| Pressure / Barometer   | STMicroelectronics LPS22HB    | ✅ `drivers/iio/pressure/st_pressure_core.c`                        |
| Heart rate sensor      | Maxim MAX86917                | ❌                                                     |
| Fingerprint Sensor     | Egis Technology ET510A        | ❌                                                     |
| Audio codec            | Cirrus Logic CS47L93          | ✅ `drivers/mfd/madera.c`<br>✅ `drivers/sound/soc/codecs/madera.c`    |
| Audio amplifier        | MAX98512                      | ✅ `drivers/sound/soc/codecs/max98512.c`                            |
| Rear wide-angle camera | Samsung S5K2L3SX              | ❌                                                     |
| Rear telephoto camera  | Samsung S5K3M3SM              | ❌                                                     |
| Front Camera           | Samsung S5K3H1SX              | ❌                                                     |
| Iris Scanner           | Samsung S5K5F1SX              | ❌                                                     |
| RAM                    | Samsung K3UH6H60AM-AGCJ       | ✅                                                              |
| NAND storage           | Samsung KLUCG2K1EA-B0C1 (UFS) | ✅ `drivers/ufs/`                                                   |
