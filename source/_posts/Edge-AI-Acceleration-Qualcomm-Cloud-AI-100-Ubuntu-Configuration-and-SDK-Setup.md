---
title: >-
  Edge AI Acceleration - Qualcomm Cloud AI 100 Ubuntu Configuration and SDK
  Setup
date: 2026-03-27 19:15:31
tags:
 - Qualcomm Cloud AI 100 Setup
 - PCIe AI accelerator
 - qpm3 install aic100-ultra-driver
 - QAIC platform SDK installation
---

I am configuring a server with a Qualcomm Cloud AI 100 (AIC100) accelerator, and I need the correct drivers and platform SDK to run machine learning models. You can find these devices online. For example, [this eBay listing](https://www.ebay.com/itm/335926973654) is provided purely as a reference so you know what hardware to look for, though you should always verify the seller's legitimacy before making a purchase. Similar to standard Ubuntu configuration guides, this walkthrough covers the exact steps to install the Qualcomm Package Manager (QPM3), resolve Linux kernel compatibility issues, build the AIC100 driver, and run a test model.

## Update System and Install Prerequisites

<!-- more -->
Before installing the Qualcomm packages, update your package lists and install the necessary build tools and kernel headers:

```
sudo apt update sudo apt install xdg-utils dkms build-essential linux-headers-$(uname -r)
```


Next, install the Qualcomm Package Manager (QPM3) using the downloaded Debian package:

```
sudo dpkg -i ./QualcommPackageManager3.3.0.131.3.Linux-x86.deb
```

## Install the AIC100 Driver

Use QPM3 to install the base AIC100 ultra driver package via the command line interface:

```
sudo qpm3 install ./aic100-ultra-driver.qpm --cli-only --no-sandbox
```

## Fix Kernel Compatibility

The AIC100 module relies on specific kernel versions to build and load successfully. If checking the loaded modules (`lsmod | grep aic100`) returns nothing, you must install a supported kernel.

## Install the Required Kernel

Install the `5.15.0-173-generic` kernel and its corresponding headers:

```
sudo apt install --install-recommends linux-image-5.15.0-173-generic linux-headers-5.15.0-173-generic
```

## Update GRUB and Reboot

Set the newly installed kernel as the default boot option in GRUB:

```
sudo grub-set-default "Advanced options for Ubuntu>Ubuntu, with Linux 5.15.0-173-generic" sudo update-grub
```

Remove any conflicting newer kernels, update GRUB again, and reboot the system for the changes to take effect:


```
sudo apt autoremove --purge sudo update-grub sudo reboot
```

## Install the QAIC Platform SDK

After logging back in, confirm you are running the correct kernel by typing `uname -r`. Once verified, navigate to your extracted SDK directory and run the installation script to compile the kernel modules:

```
cd ~/qaic-platform-sdk-1.20.1.2/x86_64/deb
sudo ./install.sh
```

## Verify Hardware and Run Inference

Check that the system correctly recognizes the PCIe device and has loaded the `qaic` and `aic100` drivers:

```
lsmod | grep aic100 lsmod | grep qaic lspci | grep -i qualcomm aic100u-info
```

Finally, validate the hardware setup by activating the Qualcomm efficient transformers virtual environment and running a GPT-2 text generation script:

```
source /opt/qeff-env/bin/activate && \
cd /workspace/efficient-transformers/examples && \
python text_generation/basic_inference.py --model-name gpt2 --prompt "Hello, how are you?"
```

## References & Further Reading
* [Qualcomm Cloud AI SDK Official Installation Guide](https://quic.github.io/cloud-ai-sdk-pages/latest/Getting-Started/Installation/)
* [Qualcomm Efficient Transformers GitHub Repository](https://github.com/quic/efficient-transformers)
* [Ubuntu Community Help Wiki: GRUB2 Setup & Kernel Management](https://help.ubuntu.com/community/Grub2/Setup)
* [Qualcomm Cloud AI 100 Linux Kernel Documentation](https://docs.kernel.org/6.12/accel/qaic/aic100.html)
* [Qualcomm Package Manager (QPM3)](https://qpm.qualcomm.com/#/main/tools/details/QPM3)
* [Dynamic Kernel Module Support (DKMS) on Ubuntu](https://oneuptime.com/blog/post/2026-03-02-how-to-use-dkms-for-dynamic-kernel-module-management-on-ubuntu/view)
* [HuggingFace - GPT-2 Model](https://huggingface.co/openai-community/gpt2)
