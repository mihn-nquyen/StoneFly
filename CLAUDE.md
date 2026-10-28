# StoneFly repo: notes for Claude Code

## What this is
- Fork of EnviroDIY/USGS_NGWOS (archived upstream), owned by mihn-nguyen. Public repo.
- Firmware examples for the EnviroDIY **Stonefly** data logger (ATSAMD51N19A, ARM Cortex-M4F, 120 MHz),
  used in the ORCA river flood monitoring station (VegaPuls C21 radar over SDI-12, Geolux HydroCam over RS232,
  SIM7080G LTE Bee for cellular upload to AWS).
- I'm an EE student new to coding. Explain changes in plain language.

## Layout
- Each `NGWOS_*` folder and `PowerAndBlink` is its **own PlatformIO project**. I open one folder at a time in VS Code.
- Inside each: `platformio_example.ini` (tracked template), `platformio.ini` (my working copy, **ignored by git**),
  and a same-named subfolder with the `.ino` sketch (`src_dir`).
- Projects in use: `PowerAndBlink`, `NGWOS_VegaAndHydroCam` (radar + camera), `NGWOS_AWS_MQTT` (cellular upload).
- Board files live outside the repo in `%USERPROFILE%\.platformio\boards\envirodiy_stonefly_m4.json`
  and `%USERPROFILE%\.platformio\variants\stonefly_m4\` (from EnviroDIY/EnviroDIYSAMDBoards).

## Rules
- **Ask before changing anything.** Only change what I asked for; tell me exactly what changed.
- Never edit files in `.pio/` (download cache) or EnviroDIY library code.
- Don't touch other example folders unless I ask.
- Don't commit or push; I do that in GitHub Desktop.
- After any `platformio.ini` change: clean build, confirm SUCCESS, and report RAM/flash usage before and after.

## Known fixes and gotchas
- All git libraries are pinned to exact commits (no branch names like `#develop`).
- EnviroDIY rewrote history on TinyGSM and Arduino-SDI-12 `ExtInts`; old commits can't be git-cloned.
  Use GitHub archive ZIPs instead: `https://github.com/EnviroDIY/<repo>/archive/<sha>.zip`.
- NGWOS_VegaAndHydroCam needs TinyGSM pinned to `95d4a57e57635d23f7dbdb9cf9659418f2021092` (last commit with
  `TinyGsmEnums.h`, which ModularSensors still includes), listed first in `lib_deps`, plus
  `build_unflags = -std=gnu++11` and `build_flags = -std=gnu++17`. Reference: `NGWOS_VegaAndHydroCam/platformio.ini`.
- First build sometimes says "Nothing to build"; rerun it.
- To save a working config to GitHub, its `platformio.ini` must be copied into `platformio_example.ini`.
