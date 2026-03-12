# Exynos9810 starlte bring-up status and hardware enablement plan

## What is likely to work today

Based on the current DTS/driver state, the following subsystems are most likely to be functional or close to functional:

- Boot to userspace with framebuffer handoff and key input.
- Basic display path (DECON/DSIM + S6E3HA8 panel wiring).
- Touch controller wiring (S6SY761).
- UFS storage path and board-level UFS rails/reset wiring.
- PMIC base topology (MAX77705 + S2DOS05) and battery/charger modeling.
- Basic panic persistence via ramoops.

Likely not working or intentionally left disabled in DTS:

- Wi-Fi / Bluetooth nodes are still disabled.
- NFC and IMU are still disabled.
- USB-C connector child node is still disabled.
- Modem, GNSS, FM, camera sensors, fingerprint, wireless charging and several vendor-specific blocks are not fully upstream-ready.

## “Out-of-box” expectations

### Android GKI + GSI

- Low probability of out-of-box daily usability.
- Even with a good DT, several Samsung-specific userspace integrations (modem/camera/audio/sensor stack, proprietary firmware interactions) are usually required.

### Mainline kernel + regular Linux distribution (postmarketOS)

- Moderate probability for "boots + display + touch + storage + keys" once image/rootfs are matched.
- Low probability for complete phone functionality out-of-box (telephony, camera, advanced audio, GNSS, NFC secure element, wireless charging).

## Prioritized bring-up plan

Priority scale:
- **P0** = must-have for stable development and basic device usability
- **P1** = high-value hardware for practical use
- **P2** = advanced/long-tail features

### P0 — Stabilize core boot path

1. **[DONE ✅] Persistent crash logging baseline**
   - Ensure ramoops captures both kernel console and userspace pmsg data.
   - Validate data survives warm reboots/panics.
   - Outcome: reliable diagnostics for all next steps.

2. **[DONE ✅] Power/reset robustness checks**
   - Validate PMIC IRQ behavior under suspend/resume.
   - Ensure watchdog/reset paths are deterministic.
   - Outcome: avoid hard-to-debug hangs during bring-up.

3. **[DONE ✅] Boot UX baseline**
   - Confirm stable simplefb handoff and input keys early in boot.
   - Outcome: repeatable boot/debug loop.

### P1 — Core usability hardware

4. **Wi-Fi bring-up**
   - Enable node only after confirming SDIO power rails/IRQ behavior and firmware/NVRAM paths.
   - Risk: medium.

5. **Bluetooth bring-up**
   - Add proper transport/wakeup sequencing and verify coexistence.
   - Risk: medium.

6. **USB-C data/power modeling cleanup**
   - Confirm real hardware path for VBUS control/switching before finalizing DT modeling.
   - Risk: medium-high if hardware path is guessed.

7. **NFC + base sensors**
   - Enable incrementally (NFC, IMU, magnetometer, barometer) and validate IRQ polarity/timing.
   - Risk: medium.

### P2 — Feature-complete phone stack

8. **Audio completeness**
   - Finalize machine routing/topology for codec + amp scenarios.
   - Risk: high.

9. **Cellular modem / telephony**
   - Integrate modem control/data path and userspace stack.
   - Risk: very high (usually largest gap).

10. **Camera stack**
    - Sensor/ISP/media graph integration and tuning.
    - Risk: very high.

11. **Wireless charging / SAFEOUT follow-ups**
    - Add only where upstream driver and binding support is clear.
    - Risk: high.

## Notes on using downstream drivers

When upstream coverage is incomplete, use downstream components selectively:

- Prefer upstream DT bindings and property names first.
- Isolate downstream-only drivers behind minimal, reviewable enablement patches.
- Keep board DT clean from vendor-only properties unless there is a documented upstream migration path.
- Upstream one hardware slice at a time with measurable functionality and logs.
