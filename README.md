<div align="center">

# 🖥️ btop — AMD GPU Build

**Pre-built [`btop`](https://github.com/aristocratos/btop) binaries with AMD GPU (ROCm SMI) support, automatically compiled and published via GitHub Actions.**

[![Build btop with AMD GPU Support](https://github.com/nargothrond/btop-amd-gpu-build/actions/workflows/build-amd-gpu.yaml/badge.svg)](https://github.com/nargothrond/btop-amd-gpu-build/actions/workflows/build-amd-gpu.yaml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Upstream: btop](https://img.shields.io/badge/upstream-aristocratos%2Fbtop-blue)](https://github.com/aristocratos/btop)

<br/>

![btop AMD GPU demo](https://raw.githubusercontent.com/aristocratos/btop/main/Img/normal.png)

</div>

---

## 🚀 What is this?

Upstream `btop` ships GPU monitoring support, but most distribution packages are compiled **without** it. This repo provides a **GitHub Actions pipeline** that:

1. Installs the **ROCm SMI** development library from AMD's official apt repository
2. Clones the latest `btop` source from [aristocratos/btop](https://github.com/aristocratos/btop)
3. Compiles with `GPU_SUPPORT=true` and `STATIC=false` (required for `dlopen`-based GPU detection)
4. Uploads the binary as a **GitHub Actions artifact** (and as a **Release asset** when a tag is pushed)

> **This repo does not contain any `btop` source code.** It only provides the build infrastructure.

---

## 📦 Download

### From GitHub Releases *(recommended)*

Go to the [Releases page](../../releases) and grab the latest `btop` binary for your architecture.

### From GitHub Actions Artifacts

Every successful run of the [build workflow](../../actions/workflows/build-amd-gpu.yaml) uploads a build artifact named `btop-amdgpu-linux-x86_64-<version>`. Artifacts are retained for **30 days**.

---

## ⚙️ Requirements

| Runtime requirement | Notes |
|---|---|
| **ROCm** (any recent version) | Must be installed on the target machine |
| `librocm_smi64.so` | Ships with `rocm-smi-lib`; btop uses `dlopen()` to load it |
| **Linux x86_64** | Only architecture currently built |
| **glibc ≥ 2.38** | Binary is built on Ubuntu 26.04 |

> If `librocm_smi64.so` is **not** present at runtime, btop falls back gracefully and works normally without GPU stats.

### Quick install

```bash
# Install ROCm SMI library (required for GPU panel)
sudo apt-get install rocm-smi-lib        # Ubuntu/Debian (ROCm apt repo)
# or
sudo dnf install rocm-smi-lib            # Fedora/RHEL

# Download the binary (replace <version> with the actual version)
curl -Lo btop https://github.com/nargothrond/btop-amd-gpu-build/releases/latest/download/btop
chmod +x btop
sudo mv btop /usr/local/bin/

# Run it!
btop
```

Press **`5`** inside btop to show the GPU monitoring panel.

---

## 🔄 Build Triggers

The workflow runs automatically on:

| Event | When |
|---|---|
| **Nightly schedule** | `03:00 UTC` every day — picks up upstream fixes |
| **Push to `main`** | On every commit to this repo |
| **Manual dispatch** | You can specify any btop branch/tag/commit |

### Manual dispatch

1. Go to [Actions → Build btop with AMD GPU Support](../../actions/workflows/build-amd-gpu.yaml)
2. Click **Run workflow**
3. Optionally enter a specific btop branch, tag, or commit hash
4. Click **Run workflow** ✅

---

## 🛠️ How the build works

```
GitHub Actions Runner (ubuntu-26.04)
│
├── apt install: git, g++, make, lowdown
├── Add AMD ROCm apt repository (repo.radeon.com)
├── apt install: rocm-smi-lib  ← headers + librocm_smi64.so
├── git clone aristocratos/btop (shallow, at $BTOP_REF)
│
└── make -j$(nproc) GPU_SUPPORT=true STATIC=false VERBOSE=true
    │
    ├── Sanity checks: btop --version, ldd, strings | grep rocm
    ├── Stage to dist/btop-amdgpu/
    │     ├── bin/btop
    │     └── themes/
    │
    ├── Upload artifact: btop-amdgpu-linux-x86_64-<version>
    └── (on tag push) Create GitHub Release with binary attached
```

---

## 🤝 Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) to get started.

Short version:
- **Bug reports / feature requests** → open a [GitHub Issue](../../issues/new/choose)
- **Fixes / improvements** → open a [Pull Request](../../compare)
- **Questions** → open a [Discussion](../../discussions)

---

## 🙏 Credits

- [aristocratos/btop](https://github.com/aristocratos/btop) — the amazing system monitor this project builds upon
- [AMD ROCm](https://rocm.docs.amd.com/) — the GPU compute platform

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).  
`btop` itself is licensed under the [Apache 2.0 License](https://github.com/aristocratos/btop/blob/main/LICENSE).
