# BMAS GPU Unlock Control Plane (`v1.0.0`)

**By Bare Metal Applied Science LLC**

## Preface — Why This Utility Exists
Consumer NVIDIA RTX 20/30/40 GPUs on Linux have far more dense FP16/BF16 Tensor compute headroom than stock workstation configurations expose, yet manually patching kernel parameters, GRUB boot targets, and DKMS modules is error-prone and risks an unbootable system.

The **BMAS GPU Unlock Control Plane** turns that process into a safe, guided, 5-stage web appliance:
- **Zero guesswork:** Automatically inspects PCIe topology, VBIOS/firmware, VRAM usage, and active GPU PIDs.
- **Safety first:** Creates SHA-256 verified GRUB/initramfs backups in a Rollback Vault before touching boot configuration.
- **AI Auto-Follow Co-Pilot:** Connects to local **Ollama** (or Groq/OpenAI-compatible) on first launch to narrate Before vs. After gains, flag risks, and verify every stage.
- **Real proof:** Benchmarks live PyTorch FP16/BF16/TF32 TFLOPS and exports an Executive HTML Audit Report.

## Supported Linux Distributions
- **Ubuntu / Debian** (22.04 LTS, 24.04 LTS, Debian 12+)
- **Red Hat Enterprise Linux (RHEL) / Rocky Linux** (9.x)
- **Oracle Linux** (9.x)
- **Fedora Workstation** (40+)
- **SUSE / openSUSE** (Leap / Tumbleweed)

> *Requires Linux kernel headers, `dkms`, `gcc`/`make`, and NVIDIA drivers installed on the target host.*

## How It Works (Try Free in Stages 1–2, Activate at Stage 3)
1. **First-Launch AI Co-Pilot Setup (Free):** Configure local Ollama or Groq/OpenAI (or Manual Mode).
2. **Stage 1 — Pre-Flight & Before/After Briefing (Free):** Detects GPU, OS, kernel, VBIOS, VRAM, and top 3 active GPU PIDs, and projects expected Tensor compute gain.
3. **Stage 2 — Rollback Vault Backup (Free):** Snapshots GRUB and boot config with SHA-256 verification.
4. **Stage 3 — Apply GRUB & Build Upstream DKMS (License Required):** Clicking **3a. Apply GRUB** or **3b. Build Upstream** on an unlicensed host opens the activation modal with your 32-character Host Fingerprint, PayPal links, and instant JSON license installer.
5. **Stage 4 — Verify Boot (`journalctl`):** Read-only post-reboot verification confirming early-boot handoff and zero XID faults.
6. **Stage 5 — PyTorch TFLOPS Proof & HTML Report:** Benchmarks live FP16/BF16/TF32 throughput and exports a printable BMAS HTML Report.

## Licensing (One-Time Payment, 100% Offline / Air-Gapped)
Licenses are cryptographically bound (Ed25519) to your host's 32-character machine fingerprint—no subscriptions, no cloud phone-home.

| Edition | Price | Scope |
| :--- | :--- | :--- |
| **Workstation Edition** | **$39** (One-Time) | Single Linux workstation / node |
| **Pro Multi-GPU Server** | **$129** (One-Time) | Single server chassis containing **4–8 GPUs** |

### Activation Steps
1. Run **Stage 1 (Pre-Flight)** and click **Copy** next to your 32-character Host Fingerprint.
2. Complete payment via PayPal (include your **Host Fingerprint** and **email** in the payment note):
   - Workstation ($39): https://paypal.me/jlastrophotographer/39
   - Pro Multi-GPU Server ($129): https://paypal.me/jlastrophotographer/129
3. Paste your signed `gpu_unlock_license.json` into the Stage 3 activation modal (saved to `/etc/bmas/gpu_unlock_license.json`).

## Quick Start
Download `bmas-gpu-unlock-control-plane-1.0.0-linux-x86_64.tar.gz` from **GitHub Releases**, extract, and run:

    tar -xzf bmas-gpu-unlock-control-plane-1.0.0-linux-x86_64.tar.gz
    cd bmas-gpu-unlock-control-plane-1.0.0
    sudo ./start_control_plane.sh

Then open **`http://<your-host-ip>:19070`** in your browser.

---
*© 2026 Bare Metal Applied Science LLC. Proprietary Control Plane. Upstream `Zagitta/gpu-unlock` (GPL-2.0-only) is fetched and built locally on the target host.*
