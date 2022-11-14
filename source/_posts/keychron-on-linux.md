---
title: Keychron 在 Linux 的 Function key (F鍵) 無法使用
date: 2022-11-14 15:29:42
tags:
  - Linux
  - Keychron
  - Function key
  - fix
---

# Linux 下的 Keychron
市面上的鍵盤多到沒辦法只用一個購物清單就全部買下，所以我呢，折衷選擇了同學想賣的二手 Keychron，免運又有折扣。

但是在開始使用的情況下發現了一個奇妙的問題，Linux 下的 [Keychron K2](https://www.keychron.com/products/keychron-k2-wireless-mechanical-keyboard) 卻沒有預設將 function keys (F1-F12) 當做原先的 F 鍵。

害得我 Refresh 都要摸摸觸控版才能完成一個按鍵的動作。

<!-- more -->

Keychron 支援兩種系統模式，**Windows** 跟 **MacOS**，但兩者都沒辦法成功操作 function keys。

所以要如何修他呢？
- 把模式切到 Windows
- 使用 `Fn + X + L` (維持四秒) 就可以將 function key 切換到 "Function mode" (如果在預設的 Windows 模式下通常不需要這麼做)
- `echo 0 | sudo tee /sys/module/hid_apple/parameters/fnmode`

如果完成上述動作，Function key 應該要跟我一樣可以用了, 然後按住 `Fn` 鍵也可以使用其他的功能鍵。

要保留這個設定，就需要在 `hid_apple` 加上一行:

`echo "options hid_apple fnmode=0" | sudo tee -a /etc/modprobe.d/hid_apple.conf`

這個操作可能需要重建 **initramfs**，如果你跟我一樣用的是 Ubutnu/Arch 就可以使用下面的命令重建。
- **Ubuntu:** `sudo update-initramfs -u `
- **Arch:** `mkinitcpio -P`

這樣就完成所有設定啦，請享用您的鍵盤。