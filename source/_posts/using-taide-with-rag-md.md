---
title: (下) 擁有超能力？！大型語言模型輔助學習，從無到有學會使用 RAG (Part 2. Anything LLM 基礎)
date: 2024-04-24 21:58:21
tags:
---


# 介紹

## 前情提要

上一篇各位就學會如何使用在自己的電腦上 TAIDE，這次透過 Anything LLM 可以簡單的把你所有資料當作動力，AI 啟動。

這次的教學要根據上次的內容搭配，如果還沒有跟到上次的教學，可以參考 [AI 焦慮？怕被 AI 淘汰？手把手教你在筆電上使用 TAIDE！(Part 1. LM-Studio)](/2024/04/23/using-taide-easily)。

<!-- ![TAIDE 啟動](1_AI_Launch.jpg) -->
![TAIDE 啟動](/2024/04/24/using-taide-with-rag-md/1_AI_Launch.jpg)

# 相關工具

我們這次教學會用到以下工具(稍後會進一步說明):

<!-- more -->

- 官方模型 TAIDE-LX-7B-Chat ([https://huggingface.co/taide/TAIDE-LX-7B-Chat](https://huggingface.co/taide/TAIDE-LX-7B-Chat))
    - [TAIDE-LX-7B-Chat-GGUF](https://huggingface.co/ZoneTwelve/TAIDE-LX-7B-Chat-GGUF)
    - 使用 TAIDE 之前必須了解一下 [TAIDE 的使用者規範](https://huggingface.co/ZoneTwelve/TAIDE-LX-7B-Chat-GGUF/blob/main/LICENSE.pdf)
- [LM Studio](https://lmstudio.ai/)
- [Anything LLM](https://useanything.com/)

# Anything LLM

## 初始化

### 歡迎介面
當你順利下載到 Anything LLM 啟動的畫面如下:
![Anything LLM Launch](2_AnythingLLM_Launch.png)

### 後端選擇

基於本次教學採用 LM Studio，故選擇 LM Studio 繼續。
![LLM Preference - Backend](3_LLM_Preference.png)

### 後端設定

主要有三個選項:
- 伺服器地址: `http://127.0.0.1:1234/v1` (也就是上一篇 LM Studio Playground 設定的伺服器)
![LLM Preference - Config Server](4_LLM_Preference_server.png)

### Embedding 設定

Embedding 是為了建立向量資料庫 (Vector Database) 的編碼工具，能夠以盡提高搜尋效率以及精準度的方法。
這邊我們就選用 `AnythingLLM Embedding`，如果你有其他想用的 Embedding model，也可以參考 Ollama 如何建置 Embedding 的 Server。

![Embedding Configration](5_Embedding_Preference.png)

### 向量資料庫 (Vector Database)

我們選用 `LanceDB`，因為這是可以零麻煩的在你本機設定一個 Vector Database。
![Vector database](6_Vector_database.png)

### 確認設定

這邊我們可以看到分別選擇:
- LMStudio 作為大型語言模型的伺服器
- AnythingLLM Embedder 作為詞向量的引擎
- LanceDB 則是向量資料庫，提供比傳統資料庫更高精度的搜尋結果。

![Comfirm](7_Comfirm_settings.png)

### 官方調查問卷

你可以根據自己的意願填寫官方調查問卷，不願意填寫的各位可以按 `Skip Survey`。
![Survey](8_survey.png)

## 個人工作區

### 建立個人工作空間 (Your Workspace)

這邊則是建立你個人期望工作空間的名稱，我這邊將以 `資訊工程學系` 示範，你可以根據自己的工作、科系、專題建立不同的工作區。

![Create workspace](9_create_workspace.png)

### 工作區介面

這邊將是你可以直接跟語言模型聊天的介面
![Workspace overview](10_workspace_overview.png)


你可以在右側點擊工作區名稱，並開啟新的聊天階段 (Chat Session)

![Chat Session](11_create_chat_session.png)

### 聊天功能區介紹

- 綠色區域: 聊天管理區域
  - 藍色括弧：在本工作區建立新的聊天
  - + New Workspace: 建立新的工作區
- 粉色區域：上傳文件 (也就是想要 **RAG** 的核心區域)
![ChatUI Feature Overview](12_chat_session.png)

### 聊天示範

在什麼文件都沒有給他的情況下，他跟你聊天會這樣
-> **在什麼情況下會需要一個機器人像我這樣的來完成這項任務呢？**

![Chat Demo](13_chat_demo.png)
> 我沒有刻意修改或引導他說話，結果說出如此深奧的話語，太有趣了！ (LOL)

## RAG

記得剛剛的粉紅色區域嗎？
這裡就是魔法 RAG 展現的地方，
![Do you remember?](14_Remember_pink_area.png)

### 上傳檔案或網頁

直接將檔案拖入**左下邊**的工作區塊(*綠色勾勾的 taide.html*)，就可以上傳檔案，接者勾選**左上方** taide.html (橘白色圓圈) 就會出現 "Move to Workspace" 移動到工作區的按鈕，點擊按鈕就可以準備將想要加入的資料傳到 Vector Database。
![Upload file](15_upload_file.png)

### 將資料存檔

如果你的畫面長這樣，就代表你即將把你上傳的資料轉成詞嵌入並加入到向量資料庫中。  
你只需要再按下 **Save and Embed** 就成功了！
![Create Database](16_prepare_embedding_and_db.png)

### 示範 Demo

接著我們以 TAIDE 官方資料測試他知道什麼！並且意外的精準！
![Demo](17_ask_taide.png)
> User: TAIDE 有什麼最新動態？

#### 相關連結資料

如果你有注意到上一張圖有個 taide.tw/index 的藍色連結，那就是你在這次聊天用到的資料，讓 LLM 告訴你正確的結果。
這就是 RAG，正確的使用就可以節省很多找資料、看資料甚至是總結資料的時間。