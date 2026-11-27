# Reddit Launch Post (`r/LocalLLaMA` / `r/homelab`)

**Title:**
Built a rollback-safe Linux GUI & verifier for Zagitta/gpu-unlock (RTX 20/30/40 Tensor boost) — 73.7 TFLOPS BF16 on an RTX 3070

**Body:**
If you've looked at the upstream Zagitta/gpu-unlock mod for unlocking Tensor throughput on Linux RTX 20/30/40 rigs, you know the pain: manually editing GRUB, managing DKMS, checking lsinitramfs for nvidia.ko race conditions, and grepping journalctl after reboot—one typo and you're fighting a black screen.

I built a Cython-compiled web control plane (BMAS GPU Unlock Control Plane v1.0.0) to wrap that workflow in a proper safety net:

1. Pre-Flight Discovery: Checks kernel, lspci -nnk, nvidia-smi, dkms, Secure Boot, GRUB, and lsinitramfs.
2. SHA-256 Rollback Vault: Snapshots /etc/default/grub and /boot/initrd.img-$(uname -r) before touching anything, with 1-click rollback + DKMS purge.
3. Local Upstream Build: Fetches and builds Zagitta/gpu-unlock directly on your host and updates gpu_unlock.targets=<PCI_ID> in GRUB. (Zero GPL-2.0 code or NVIDIA firmware is bundled in the binary.)
4. Post-Reboot Verifier: Parses journalctl -k -b and /proc/cmdline for ss0=0x88888888, ss1=0x8, clean handoff before NVRM, and zero XID faults.
5. PyTorch 4096x4096 Benchmarker: Runs BF16/FP16/TF32 matmul and exports a print-safe HTML audit report.
+ Optional AI Co-Pilot sidebar that hooks into your local Ollama (or Groq/OpenAI-compatible) endpoint to inspect raw stage telemetry.

Live test on my Ubuntu 24.04.3 LTS / Kernel 7.0.0-44-generic RTX 3070 box:
- BF16: 73.72 TFLOPS
- FP16: 73.42 TFLOPS
- TF32: 36.47 TFLOPS
- Kernel checks: 6/6 PASS, 0 XID faults

Full transparency: the upstream CLI mod is free on GitHub. This is a commercial orchestration/safety utility ($39 one-time for a single workstation, $129 for multi-GPU 4x-8x nodes, no subscriptions). Use at your own risk—no liability for hardware or future vendor driver/firmware changes.

Repo & Linux x86_64 release: https://github.com/proxy80/bmas-gpu-unlock

Happy to answer any technical questions on the initramfs/DKMS checks or PyTorch numbers.
