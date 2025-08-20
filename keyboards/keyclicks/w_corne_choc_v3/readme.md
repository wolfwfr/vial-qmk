# keyclicks w-corne-choc-v3

![keyclicks](imgur.com image replace me!)

_A short description of the keyboard/project_

- Keyboard Maintainer: keyclicks
- Hardware Supported: _The PCBs, controllers supported_
- Hardware Availability: _Links to where you can find this hardware_

Make example for this keyboard (after setting up your build environment):

    make keyclicks/w_corne_choc_v3:vial

Upon successful compilation this should create a `.vfw` file among others that
you can use to flash the board using a local instance of vial.

Pay attention to the firmware size! The STM32F103C8T6 inside the dongle only has
`64kB of flash memory`. Any firmware exceeding that size will not fit.

See the [build environment
setup](https://docs.qmk.fm/#/getting_started_build_tools) and the [make
instructions](https://docs.qmk.fm/#/getting_started_make_guide) for more
information. Brand new to QMK? Start with our [Complete Newbs
Guide](https://docs.qmk.fm/#/newbs).

## Helpful notes on the w-corne-choc-v3 and other keyclicks boards

> [!WARNING]
> The `v3` versions of keyclicks boards are configured with `16MHz` clock
> frequencies instead of `8MHz` as for the non-v3 versions. Be careful when
> flashing your board. In my case, the 16MHz presents no problem but your mileage
> may vary. This setting can be changed in `mcuconf.h`

> [!NOTE]
> The w-corne-choc-v3 has a curious matrix and layout definition in the source
> code. First, the right-hand side is represented in reverse. So your top row
> with the `YUIOP` buttons need to be configured as `POIUY` if you configure
> your layout in the source code. Second, the original layout in `info.json` was
> pretty garbled up, as will be evidenced when you have a look at the output of
> `qmk info -l -kb "keyclicks/w_corne_choc_v3"`. But worse than just producing a
> bad image, it caused compilation errors due to the QMK tooling not accepting
> the one negative number in the layout definition. Through a little bit of
> trial and error I have rearranged the bottom keys in the layout in `info.json`
> in order to better reflect the look of the keyboard. The important thing
> though is that this new version (now renamed to `keyboard.json` by the way, to
> reflect modern QMK idioms) compiles successfully. I am not entirely sure, but
> I think, at least when relying on vial, there is no functional consequence to
> altering the layout in this file.

## Bootloader

Enter the bootloader in 3 ways:

- **Bootmagic reset**: Hold down the key at (0,0) in the matrix (usually the top left key or Escape) and plug in the keyboard
- **Physical reset button**: Briefly press the button on the back of the PCB - some may have pads you must short instead
- **Keycode in layout**: Press the key mapped to `QK_BOOT` if it is available

> [!NOTE]
> w-corne-choc supports entering the bootloader by pressing the button annotated
> by `boot` on the dongle pcb (note that there are two buttons, one for the vial
> bootloader, and one on the other side of the PCB that presumably used for
> flashing the nRF51822 chip).

## Flashing

> [!NOTE]
> Flashing the w-corne-choc is performed through the vial bootloader (vibl),
> which is not supported (at least for me) through vial-web (vial running in
> your web browser). If you want to flash your device, you'll have to download
> and run vial on your local system.

> [!NOTE]
> The QMK/vial firmware is only running on the STM32F103 chip in the dongle. The
> keyboard PCBs are outfitted only with nRF51822 chips (in my case at least)
> that transmit wireless signals to the dongle (and maybe each other?). This
> means that flashing new QMK/vial firmware happens exclusively on the dongle
> PCB.

1. Save your current layout if your layout is not stored in the source code that
   was used to build the firmware.
2. Disconnect the dongle from the computer. The keyboard halves don't come into
   play, they can remain on or off; it doesn't matter.
3. Press and hold the physical button on the dongle PCB that's annotated with
   `boot`.
4. While holding the button, connect the dongle over USB to the computer.
5. Launch (if it wasn't already) a local instance of the vial configurator
   software (download from https://get.vial.today).
6. Navigate to the `Firmware updater` tab.
7. Select the `.vfw` firmware you want to install.
8. Select the `flash` button and wait until the progress bar reports 100%
   completion.
9. The keyboard should now be flashed with your firmware and be operational. It
   is now safe to unplug the dongle when necessary.
