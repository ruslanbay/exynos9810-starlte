# Exynos9810 (Galaxy S9 SM-G960F) mainline DT analysis report — Part 2

## Borrowing matrix (cross-SoC references)

### Core pinctrl constants / style
- **`exynos-pinctrl.h`**
  - Reuse the canonical pin function + drive-strength constants (e.g., `EXYNOS_PIN_FUNC_*`,
    `EXYNOS7_PIN_DRV_LV*`, FSYS-specific drive levels) so new pinctrl groups stay consistent
    with upstream naming and electrical-level style.

## Exynos2200 family
- **`exynos2200-g0s.dts`**
  - Good template for board-level `regulator-fixed` placeholders and clean key/pinctrl wiring
    while PMIC support is incomplete.

- **`exynos2200-pinctrl.dtsi`**
  - Rich HSI2C pin group definitions and modern pinmux style you can mirror for missing 9810
    bus groups (function/pud/drv consistency).

- **`exynos2200.dtsi`**
  - Strong reference for complete USI/HSI2C plumbing: `clocks`, `clock-names`,
    `samsung,sysreg`, and `pinctrl-0` on child buses; this is exactly where 9810 is still
    sparse.

## Exynos850 family
- **`exynos850-e850-96.dts`**
  - Useful board patterns for connector modeling, `regulator-fixed` use,
    `reserved-memory`/`ramoops`, and pragmatic USB bring-up on dev boards.

- **`exynos850-pinctrl.dtsi`**
  - Reuse naming/layout patterns for HSI2C pin groups and pull/drive conventions in pinctrl
    nodes.

- **`exynos850.dtsi`**
  - One of the best references for USI+HSI2C completeness (sysreg offsets + dual clocks +
    child hsi2c clocks/pinctrl), ideal for 9810 cleanup.

## Exynos8895 family (closest generation to 9810 phone lineage)
- **`exynos8895-dreamlte.dts`**
  - Borrow SD tuning triplet (`ciu-div`, `ddr/sdr timing`) and SD slot wiring style; this maps
    well to starlte SD enablement quality goals.

- **`exynos8895-pinctrl.dtsi`**
  - Use as style reference for bank organization and per-function pin states in phone boards
    (especially fsys/peric groups).

- **`exynos8895.dtsi`**
  - Critical reference for older Samsung USI composition with `samsung,sysreg` and mixed-mode
    children (I2C/UART/SPI) under one USI parent.

## Current 9810 files (what to keep / improve)
- **`exynos9810-pinctrl.dtsi`**
  - Keep current functional groups strategy; can still expand with missing bus states using
    850/8895/2200 patterns.

- **`exynos9810-starlte.dts`**
  - Keep staged bring-up structure (gpio-keys, reserved-memory, panel, UFS/MMC/USB); fix
    topology inconsistencies (notably Wi-Fi placement and connector consistency) before
    upstream posting.

- **`exynos9810.dtsi`**
  - Keep CMU/syscon skeleton and broad SoC node bring-up; enrich USI/HSI2C with missing
    clocks/sysreg plumbing borrowed from newer SoCs.

## Exynos990 family
- **`exynos990-c1s.dts`**
  - Borrow modern phone-board baseline pattern: `simple-framebuffer`, `reg_dummy`, reserved
    carveouts, gpio-keys, and temporary USB supply hookup during early bring-up.

- **`exynos990-pinctrl.dtsi`**
  - Good pinctrl bank IRQ wiring template plus useful board-facing states (UART single-bus
    pinset, wlan wake pin) for naming and organization cues.

- **`exynos990-r8s.dts`**
  - Same bring-up template as c1s (dummy regulator + simplefb + keys + USB supply
    placeholders), useful for consistency across Samsung phone DTS style.

- **`exynos990-x1s-common.dtsi`**
  - Useful shared-board include pattern for sibling devices (common
    reserved-memory/keys/reg placeholders in one file).

- **`exynos990-x1s.dts`**
  - Borrow variant-split strategy (`#include` common + minimal per-variant deltas like
    memory/model).

- **`exynos990-x1slte.dts`**
  - Same as x1s: very clean variant layering pattern for closely-related products.

- **`exynos990.dtsi`**
  - Borrow alias/pinctrl block organization and clock-tree modeling structure for maintainable
    SoC-level layout.

## ExynosAuto v9 / v920 family
- **`exynosautov9-pinctrl.dtsi`**
  - Good reference for systematic pin state definitions, including output/eint/function
    variants you can mirror in 9810 pinctrl expansion.

- **`exynosautov9-sadk.dts`**
  - Borrow board enablement style for UFS + key input + per-node status bring-up with minimal
    noise.

- **`exynosautov9.dtsi`**
  - Useful for SoC-scale organization and clocked peripheral patterns; helps keep 9810 dtsi
    additions structured as they grow.

- **`exynosautov920-pinctrl.dtsi`**
  - Excellent source for UFS reset/refclk pin states and HSI bank organization (very relevant
    to 9810 UFS pinctrl cleanup).

- **`exynosautov920-sadk.dts`**
  - Clean example of key/pwm/uart minimal board activation and `samsung,clkreq-on` usage for
    USI UART mode.

- **`exynosautov920.dtsi`**
  - One of the strongest USI references: explicit compatible stacking, `samsung,sysreg`
    offsets, and clock pairs (`pclk`/`ipclk`) on USI parents.

## Tesla FSD set (cross-vendor but useful design patterns)
- **`tesla/Makefile`**
  - Borrow the clean DTB target inclusion style when adding new board DTBs to a family
    Makefile.

- **`tesla/fsd-evb.dts`**
  - Good example of board-level peripheral enablement overlays (`&hsi2c_*`, codec nodes with
    `reset-gpios`, etc.).

- **`tesla/fsd-pinctrl.dtsi`**
  - Borrow pinctrl naming discipline for UFS reset/refclk states and consistent drive-strength
    tuning blocks.

- **`tesla/fsd-pinctrl.h`**
  - Borrow the idea of SoC-family-local pinctrl constants header when per-family levels differ
    from Exynos defaults.

- **`tesla/fsd.dtsi`**
  - Useful template for SoC alias structuring, reserved-memory organization, and IOMMU node
    placement conventions at scale.

## Recommendations for what to borrow most aggressively next (for Exynos9810)
1. **USI/HSI2C plumbing**
   - Borrow from `exynos850.dtsi` + `exynos2200.dtsi` + `exynosautov920.dtsi`.

2. **Board layering / placeholders**
   - Borrow from `exynos990-*` board files (common + per-variant pattern, dummy rails,
     simplefb/reserved-memory hygiene).

3. **MMC/SD tuning baseline**
   - Keep using dreamlte values as conservative known-good Samsung-phone defaults unless
     9810-specific data disproves them.


# Below is a prioritized list based on:

- likely hardware impact, and
- implementation effort in upstream-friendly DTS terms.


## High Priority

1) [DONE ✅] SDIO Wi‑Fi real supplies (remove placeholder rails)

    Component / subsystem: SDIO Wi‑Fi power (vmmc-supply/vqmmc-supply)

    Where it exists in references: Current starlte DTS still uses reg_placeholder for both SDIO rails; that’s explicitly temporary.

    Why it may apply to Exynos9810: This is a direct functional blocker for realistic Wi‑Fi bring-up.

    Estimated difficulty: Medium

    Expected impact: High

    Draft high-level commit message:
    `arm64: dts: exynos: starlte: replace SDIO placeholder supplies with board rails`

2) [DONE ✅] S2DOS05 regulator electrical properties (active-discharge first)

    Component / subsystem: S2DOS05 regulator behavior

    Where it exists in references: Binding example explicitly uses regulator-active-discharge; starqltechn also carries stronger electrical hints on S2DOS05 rails.

    Why it may apply to Exynos9810: Same PMIC + same rail voltages already used on starlte panel/touch rails.

    Estimated difficulty: Low

    Expected impact: Medium–High (cleaner rail behavior)

    Draft high-level commit message:
    `arm64: dts: exynos: starlte: add S2DOS05 regulator discharge settings`

## Medium Priority

3) [DONE ✅] S2DOS05 regulator ramp-delay tuning (if justified by bring-up logs)

    Component / subsystem: S2DOS05 timing characteristics

    Where it exists in references: starqltechn sets regulator-enable-ramp-delay on S2DOS05 rails.

    Why it may apply to Exynos9810: Shared board component suggests transferable timing hints, but this is less universal than active-discharge.

    Estimated difficulty: Low

    Expected impact: Medium

    Draft high-level commit message:
    `arm64: dts: exynos: starlte: tune S2DOS05 rail ramp delays`

4) [DONE ✅] MAX77705 IRQ pinctrl/wakeup electrical state cleanup

    Component / subsystem: MAX77705 IRQ stability across suspend/resume

    Where it exists in references: starlte has IRQ lines wired but minimal state modeling; MAX77705 binding example includes explicit pinctrl use.

    Why it may apply to Exynos9810: Better wake/IRQ behavior and less noisy interrupts.

    Estimated difficulty: Medium

    Expected impact: Medium

    Draft high-level commit message:
    `arm64: dts: exynos: starlte: add MAX77705 IRQ pinctrl state`

## Low Priority

5) [DONE ✅] USB-C VBUS regulator modeling (if hardware switch exists)

    Component / subsystem: USB Type-C power rail modeling

    Where it exists in references: USB connector binding supports vbus-supply and pd-disable.

    Why it may apply to Exynos9810: Only meaningful if we can map a real board VBUS switch/regulator path.

    Estimated difficulty: Medium

    Expected impact: Medium (conditional)

    Draft high-level commit message:
    `arm64: dts: exynos: starlte: describe USB-C VBUS supply for non-PD mode`
