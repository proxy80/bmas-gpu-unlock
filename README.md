# BMAS GPU Unlock Control Plane (v1.0.0)

**Bare Metal Applied Science LLC** — Rollback-safe, Cython-compiled Linux workstation & fleet orchestrator for NVIDIA RTX 20/30/40 series Tensor performance unlocks, featuring an interactive SVG dashboard, `journalctl` early-boot auditor, PyTorch TFLOPS benchmarker, and pluggable Ollama/Groq AI Auto-Follow Co-Pilot.

---

## Why This Was Created

The underlying GPU Tensor unlock technique was discovered and shared by the open-source community (upstream `Zagitta/gpu-unlock`). While effective, applying it manually requires editing `GRUB`, managing `DKMS` modules, inspecting `initramfs` for `nvidia.ko` race conditions, and auditing `journalctl` kernel logs—where a single mistake can leave a Linux workstation unbootable.

**Bare Metal Applied Science LLC** built this control plane to refine that raw workflow into a safer, repeatable, and auditable operator experience with automated SHA-256 backups, 1-click rollback, and empirical PyTorch TFLOPS verification.

---

## Verified Benchmark (RTX 3070 — Ubuntu 24.04.3 LTS / Kernel 7.0.0-44-generic)

| Precision (4096×4096 `torch.matmul`) | Unlocked Throughput |
| :--- | :--- |
| **FP16** | **74.99 TFLOPS** |
| **BF16** | **73.72 TFLOPS** |
| **TF32** | **36.47 TFLOPS** |

All 6 post-reboot kernel verification checks (`cmdline_has_target`, `ss0_verified`, `ss1_verified`, `unlocked_banner`, `handoff_before_nvrm`, `zero_xid_faults`) validated `PASS`.

---

## 5-Stage Automated Workflow

1. **Pre-Flight Discovery:** Queries kernel version, OS, `lspci -nnk`, `nvidia-smi`, `dkms status`, `lsinitramfs` (`nvidia.ko` race check), `mokutil --sb-state`, and `/etc/default/grub`.
2. **Rollback Vault:** Creates timestamped, SHA-256 verified backups of `/etc/default/grub` and `/boot/initrd.img-$(uname -r)` in `/root/bmas-gpu-unlock-backup/` with 1-click rollback and DKMS purge.
3. **Guided Unlock & GRUB Stage:** Fetches and builds upstream `Zagitta/gpu-unlock` locally on your host and safely updates `gpu_unlock.targets=<PCI_ID>` in GRUB.
4. **Post-Reboot `journalctl` Verifier:** Audits `journalctl -k -b` and `/proc/cmdline` for `ss0=0x88888888`, `ss1=0x8`, `gpu_unlock: unlocked`, clean handoff before `NVRM`, and zero XID faults.
5. **PyTorch TFLOPS Proof-of-Gain & HTML Audit Report:** Runs 4096×4096 BF16/FP16/TF32 benchmarks and exports a print-safe landscape BMAS HTML Audit Report.

---

## Supported Linux Distributions
- **Ubuntu / Debian** (22.04 LTS, 24.04 LTS, Debian 12+)
- **Red Hat Enterprise Linux (RHEL) / Rocky Linux** (9.x)
- **Oracle Linux** (9.x)
- **Fedora Workstation** (40+)
- **SUSE / openSUSE** (Leap / Tumbleweed)

> *Requires Linux kernel headers, `dkms`, `gcc`/`make`, and NVIDIA drivers installed on the target host.*

## Quick Start (Compiled Binary Release)

Download the latest release tarball from **GitHub Releases**:

    tar -xzf bmas-gpu-unlock-control-plane-1.0.0-linux-x86_64.tar.gz
    cd bmas-gpu-unlock-control-plane-1.0.0-linux-x86_64
    pip install fastapi uvicorn cryptography pydantic
    sudo -E ./run_bmas_control_plane.sh 19070

Open `http://<host-ip>:19070` in your browser.

---

## Commercial Licensing & Pricing

* **Workstation Edition ($39 one-time):** Single-node Linux workstation license (full 5-stage SVG GUI, Rollback Vault, `journalctl` verifier, PyTorch HTML audit report, and Ollama/Groq AI Co-Pilot).
* **Pro / Multi-GPU Fleet Edition ($129 one-time):** Multi-GPU PCIe targeting (4×–8× GPU nodes), Ed25519 hardware-bound licensing, and persistent audit logs.

---

## Disclaimer & Limitation of Liability (Use at Your Own Risk)

**USE AT YOUR OWN RISK.** This software automates low-level Linux kernel, bootloader (`GRUB`), `initramfs`, and GPU hardware configuration workflows. By downloading, installing, or executing the **BMAS GPU Unlock Control Plane**, you explicitly acknowledge and agree that:

1. You assume **full and sole responsibility** for all risks associated with modifying kernel parameters, loading third-party kernel modules, and operating your GPU hardware outside of manufacturer defaults.
2. This software is provided **"AS IS"**, without warranty of any kind, express or implied, including but not limited to warranties of merchantability, fitness for a particular purpose, or non-infringement.
3. Under no circumstances shall **Bare Metal Applied Science LLC**, its founder, contributors, or affiliates be held liable for any direct, indirect, incidental, special, or consequential damages—including, without limitation, **hardware or GPU/video card damage, thermal failure, voided manufacturer warranties, system instability, boot failure, or data loss**—arising out of the use or inability to use this software.

---

## Upstream Attribution

* **BMAS GPU Unlock Control Plane** is proprietary software © 2026 **Bare Metal Applied Science LLC**.
* **Upstream Kernel Module:** This control plane does **not** bundle or redistribute kernel code or NVIDIA firmware. Stage 3 invokes your local host to fetch and build upstream [Zagitta/gpu-unlock](https://github.com/Zagitta/gpu-unlock) (licensed **GPL-2.0-only**) directly on your machine.

---

## How to Purchase & Activate a License

1. **Get Your Host Fingerprint:** Extract the release tarball, start the control plane, and visit `http://<host-ip>:19070/api/license` to copy your 32-character `fingerprint`.
2. **Complete One-Time Payment via PayPal (`@jlastrophotographer`):**
   * **Workstation Edition ($39 one-time, single node):** https://paypal.me/jlastrophotographer/39
   * **Pro / Multi-GPU Fleet Edition ($129 one-time, multi-GPU node):** https://paypal.me/jlastrophotographer/129
3. **Receive Your Key:** Include your **Host Fingerprint** and **email address** in the PayPal payment note (or send your receipt + fingerprint via LinkedIn DM).
4. **Install License:** Place the signed `gpu_unlock_license.json` you receive into `/etc/bmas/gpu_unlock_license.json` and restart the control plane.
