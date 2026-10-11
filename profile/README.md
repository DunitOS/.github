<p align="center">
  <img src="https://raw.githubusercontent.com/DunitOS/dunit-os/main/assets/images/image.png" width="820" alt="Dunit OS"/>
</p>

<h1 align="center">Dunit OS</h1>

<p align="center">
  A from-scratch x86_64 operating system — a Rust <code>no_std</code> microkernel,
  a userspace desktop, its own stable ABI, and a ported musl.
</p>

<p align="center">
  <code>kernel = mechanism</code> · <code>userspace = policy</code> · <code>config drives the desktop</code>
</p>

## 📌 Repositories

<table>
<tr>
<td width="50%" valign="top">

### [dunit-os](https://github.com/DunitOS/dunit-os)

The OS monorepo — the **Green Tea** kernel (Rust `no_std`), the C/NASM HAL,
Limine BIOS/UEFI boot, the userspace ABI + `libdunit`, the Stack B
compositor/DWM and apps, DunitFS, and the `tools/qemu_test.py` test harness.

`Rust` · `no_std` · `x86_64` · `Limine`

</td>
<td width="50%" valign="top">

### [dunit-musl](https://github.com/coreformdev/dunit-musl)

A fork of [musl](https://musl.libc.org/) being **ported onto Dunit's native
syscall ABI** (not Linux compat), so static C programs build against the
system's own libc.

`C` · `libc` · `static` · `port`

</td>
</tr>
</table>

---

Processes are independent ring-3 ELF binaries; the compositor/DWM and every app
live in userspace; **configuration — not Rust code — drives the desktop**. POSIX
and musl are treated as a *source-portability layer*, not a Linux-compatibility
goal: the kernel speaks its own native syscall ABI, and everything above it is
replaceable policy.

### Highlights

- Boots on **BIOS and UEFI** (Limine), with a Terminal Mode and a GUI/DWM Mode.
- Preemptive multithreaded ring-3 processes with **crash isolation** — a faulting
  app is killed, the system lives on.
- **Config-driven desktop**: no app names, paths, colors, or layout baked into
  code — the compositor reads it all from TOML.
- Native ABI + a ported musl; runs **static C programs** against its own libc.
- Installs to a **real disk** with a persistent filesystem (DunitFS).

### Build & run

Everything builds and tests through one entry point:

```sh
# clean GUI desktop
make run-dwm
# or the normal ISO (Limine menu: Terminal Mode / Graphical Interface)
make iso && qemu-system-x86_64 -enable-kvm -cdrom build/microkernel.iso -m 512M \
  -serial stdio -display gtk -vga std -global VGA.vgamem_mb=32 -boot menu=on
```

> A hobby OS, built to be understood, changed, and verified end-to-end.
