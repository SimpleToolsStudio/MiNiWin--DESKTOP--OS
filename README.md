# MiniWin Desktop OS

A miniature graphical desktop operating system written in x86 assembly.

![License](https://img.shields.io/badge/License-MIT-blue.svg)
![Assembly](https://img.shields.io/badge/Assembly-NASM-orange.svg)

---

## Features

- 320×200 256-color graphics mode
- PS/2 mouse driver
- Crosshair cursor + double buffering
- Status bar showing coordinates
- ~45 FPS

---

## File Structure

```
MiNiWin--DESKTOP--OS/
├── bin/
│   ├── boot.bin          # Build artifact
│   ├── kernel.bin        # Build artifact
│   └── os.img            # Bootable image
├── src/
│   ├── boot.asm          # Boot sector
│   ├── kernel.asm        # Kernel entry (only `%include "stage2.inc"`)
│   └── stage2.inc        # Desktop system implementation
├── tools/
│   ├── build.sh          # Build script (Linux/WSL)
│   └── STARTOS.BAT       # One-click launch (Windows)
├── LICENSE
└── README.md
```

---

## Build and Run

### Windows

Double-click `tools/STARTOS.BAT`

> Prerequisite: `bin/os.img` must already exist. On first use, run `tools/build.sh` under Linux/WSL first, or use the `bin/os.img` bundled with the package.

### Linux/WSL

```bash
cd tools
chmod +x build.sh
./build.sh
```

The script will automatically compile, package `bin/os.img`, and ask whether to launch QEMU immediately.

### Manual Build

```bash
nasm -f bin src/boot.asm -o bin/boot.bin
nasm -f bin -I src/ src/kernel.asm -o bin/kernel.bin

dd if=/dev/zero of=bin/os.img bs=512 count=2880
dd if=bin/boot.bin of=bin/os.img bs=512 count=1 conv=notrunc
dd if=bin/kernel.bin of=bin/os.img bs=512 seek=1 conv=notrunc

qemu-system-x86_64 -drive format=raw,file=bin/os.img
```

> Note: `src/kernel.asm` contains `%include "stage2.inc"`, and NASM will expand `stage2.inc` into it during assembly. Since `stage2.inc` and `kernel.asm` are both in the `src/` directory, you need to add `-I src/` to specify the include search path when compiling.

---

## Dependencies

- NASM
- QEMU (for running)
- dd / certutil (for packaging)

---

## License

MIT License

Copyright (c) 2026 20260509 (SimpleTools studio)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.