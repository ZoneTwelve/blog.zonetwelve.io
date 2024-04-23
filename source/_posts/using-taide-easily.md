---
title: AI 焦慮？怕被 AI 淘汰？手把手教你在筆電上使用 TAIDE！(Part 1. LM-Studio)
date: 2024-04-23 22:17:39
tags:
 - LLM
 - TAIDE
 - 人工智慧
 - 機器學習
 - 工作效率提升
---

# 介紹

## 關於 TAIDE

根據 TAIDE 官方 Huggingface 介紹 「[TAIDE計畫](https://taide.tw/index)致力於開發符合台灣語言和文化特性的生成式人工智慧對話引擎模型，同時建構可信任的人工智慧環境。結合產學研能量，推動可信任生成式人工智慧的發展，提升台灣在國際競爭中的地位，促進產業發展，避免對外國技術的依賴。」

簡單扼要地說，TAIDE 是一個台灣在地化引擎，雖然小弟我習慣使用外語學習，但是中文學習起來還是更輕鬆一些。

所以我想藉由這次 TAIDE 釋出撰寫一篇文章，希望可以幫助大家不要被這波的 AI 潮流帶走。

## 前情提要
<!-- more -->
這次教學會需要對文件以及網路有一丁點的了解，難度不高，但需要大家跟緊。


我們這次教學會用到以下工具
- 官方模型 TAIDE-LX-7B-Chat (https://huggingface.co/taide/TAIDE-LX-7B-Chat)
  - [TAIDE-LX-7B-Chat-GGUF](https://huggingface.co/ZoneTwelve/TAIDE-LX-7B-Chat-GGUF)
  - 使用 TAIDE 之前必須了解一下 [TAIDE 的使用者規範](https://huggingface.co/ZoneTwelve/TAIDE-LX-7B-Chat-GGUF/blob/main/LICENSE.pdf)
- [LM Studio](https://lmstudio.ai/)
- [Anything LLM](https://useanything.com/)

# LM Studio

使用 LM Studio,您可以...
🤖 - 在您的筆記型電腦上完全離線運行大型語言模型
👾 - 通過應用內的聊天界面或相容 OpenAI 的本地伺服器使用模型
📂 - 從 HuggingFace 🤗 存儲庫下載任何相容的模型檔案
🔭 - 在應用程式的首頁中探索新的和值得注意的大型語言模型
LM Studio 支援 Hugging Face 上的任何 ggml Llama、MPT 和 StarCoder 模型(Llama 2、Orca、Vicuna、Nous Hermes、WizardCoder、MPT 等)

最低要求:M1/M2/M3 Mac,或支援 AVX2 的 Windows 電腦處理器。Linux 版本處於測試階段。

你可以點擊[此連結下載 LM Studio](https://lmstudio.ai/)
![Download](1_DownloadLMStudio_on_Mac.png)

## 安裝

![Installation](2_Install_LMStudio.png)
將 LM Studio 安裝到您的電腦中，然後開啟它，並選擇您要使用的模型。

## 使用

![Release Note](3_ReleaseNote.png)
點 Close 並繼續。

![Search the Model](4_Search_the_Model.png)
輸入您想要使用的模型名稱，我這裡會以 TAIDE-LX-7B-Chat 作為範例。

![Model list](5_ModelList.png)

第 1 點(粉紅色) 這邊是可以選擇 ZoneTwelve/TAIDE-LX-7B-Chat-GGUF -> (我選 Q2_K 因為最省資源) 作為要用的模型
第 2 點(綠色) 是顯示你電腦可以使用的模型
    - 如果顯示藍色則是有點勉強
    - 如果是紅色則是跑不動 <img src="5-1_Unable_to_run.png" width="480px"/>

接下來就是開始使用進入遊樂場 (Playground) ![6.Playground](6_Playground.png)


![Play with TAIDE](6-1_Playground_play_with_TAIDE.png)
這頁面分別是
- (粉紅色) 模型選單
- (綠色) 模型設定(包含 System Prompt) 系統提示
- (藍色) 跟 TAIDE 聊天



