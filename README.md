# How to Keychron QMK

So, you got a Keychron QMK compatible keyboard and want to mess with the firmware? You are in the right place. Let's do this.

> [!NOTE]
> I just noticed that this is way too small to be a repo, and should really be a gist, but I can't bother. Also, as a disclaimer, I should note that this was tested in a **Linux** environment, more specifically **Ubuntu 22.04**. 

## Table of Contents

1. [Install QMK](#1-install-qmk)
2. [Clone the Keychron QMK repo](#2-clone-the-keychron-qmk-repo)
3. [Find out in which branch your keyboard is](#3-find-out-in-which-branch-your-keyboard-is)
4. [Add the necessary submodules](#4-add-the-necessary-submodules)
5. [Mess with the firmware as you like](#5-mess-with-the-firmware-as-you-like)
6. [Compile](#6-compile)
7. [Flash](#7-flash)
8. [Gotchas](#8-gotchas)

## 1. Install QMK

First and foremost, install QMK. It's pretty simple, just follow [this][1] for your OS. Mine is as follows:

```sh
sudo apt install -y git python3-pip
python3 -m pip install --user qmk
```

> [!NOTE]
> Run `qmk doctor` and make sure everything is well and running. Execute the necessary commands for this to appear as green. This is HIGHLY recommended, as there might be some drivers you must declare and dependencies you must install so that you can compile and flash the firmware.

## 2. Clone the Keychron QMK repo

That's right, Keychron has its [own QMK repo][2] for their keyboards. You must clone it.

```sh
git clone https://github.com/Keychron/qmk_firmware
```

Please, be mindful of one thing. You **MUST** replace your old `qmk_firmware` directory with the one from the Keychron repo. For me, the directory was `$HOME/qmk_firmware`.

## 3. Find out in which branch your keyboard is

Ok, this part is on you. By searching around the branches, I found that both Q10 Max and K15 Pro are inside `wireless_playground`[^1], so I would start searching there. You will find the keyboards in the directory `qmk_firmware/keyboards/keychron`.

## 4. Add the necessary submodules

Simply go to the `qmk_firmware` and run `make git-submodule`. It will install all the submodules needed.

> [!NOTE]
> I have not tested if this needs to be done in a branch basis. I would do so just to be sure.

## 5. Mess with the firmware as you like

Do as you please with the firmware. It should be inside of `qmk_firmware/keyboards/keychron/KEYBOARD_NAME`. Mess around with anything you want. As for me, I wanted to adjust `TAPPING_TERM` and `QUICK_TAP_TERM`, so I mostly played with `config.h`. I have also created my default layout in `qmk_firmware/keyboards/keychron/q10_max/ansi_encoder/keymaps/via/keymap.c`.

I don't really know what you are looking for here to customize, so I will leave [this link][3] to the QMK docs, so that you can see what is possible to do.

## 6. Compile

After customizing, you MUST compile the firmware. Execute the command below:

```sh
qmk compile -kb keychron/KEYBOARD_NAME/ENCODER/AVAILABLE_COLORS -km via
```

Look, this part is kinda confusing. Basically, `KEYBOARD_NAME` is the keyboard name, ENCODER is either `ansi_encoder`, `iso_encoder` or `keypad_encoder`, and AVAILABLE_COLORS is either `rgb` or `white`. There might be other values, but those are the ones I found. In doubt, just search for the directory where the `keymaps` directory is, and that should be it.

## 7. Flash

Just do this:

1. Execute `qmk flash -kb keychron/KEYBOARD_NAME/ENCODER/AVAILABLE_COLORS -km via`. Notice that, after a few seconds, there will be a waiting warning for a connection with a keyboard in bootloader mode;
2. Set your keyboard to OFF in the back switch. If there is no OFF option, simply unplug the USB;
3. Press ESC or the reset button below the spacebar while toggling the switch to `Cable` or plugging the USB in;
4. **DO NOT REMOVE THE USB** while the flashing is in process;
5. Done;

Congratulations, you just customized your Keychron keyboard firmware.

## 8. Gotchas

Some things you should know when dealing with Keychron keyboards firmwares.

### Tick counter reset by keyboard sleep

As stated in the [Keychron FAQ](https://keychron.in/faq/), keyboards go into sleep mode automatically after 10 minutes of sitting idle. At every, or nearly every, keyboard sleep, the tick counter returned by `timer_read32()` resets[^2]. Keep this is mind when using it. More info about this in [#1](https://github.com/ed-henrique/how-to-keychron-qmk/issues/1).

## Oh no, something went wrong

Please, open an issue in this repo, detailing what is your problem and how to fix it (if known). This sure helps other folks.

[^1]: As [pointed out by u/PeterMortensenBlog](https://www.reddit.com/r/Keychron/comments/1i0v0bz/comment/m71o321/) you should not use the branch `bluetooth_playground` for K Pro series keyboards, since it can be considered stale, and may make debugging harder for you.

[1]: https://docs.qmk.fm/newbs_getting_started
[2]: https://github.com/Keychron/qmk_firmware
[3]: https://docs.qmk.fm/
