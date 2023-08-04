---
title: Modbus Security pt.1
date: 2023-08-04 20:33:19
tags:
 - Modbus
 - Security
 - Infrastructure
---

# 摘要
小弟我在 N 年前曾經是做電機工控相關的研究與開發，近期也有朋友來詢問我相關的問題，因此我要寫一些關於 Modbus 協議的內容。
<!-- more -->
# 大綱

## Modbus 介紹

Modbus 是一種串行通訊協議，由 Modicon 於 1979 年發表，能夠用於可編程邏輯控制器 (PLC)。
採用主從式架構通訊，也就是 Master 對 Slave 做通訊。
Modbus 的特性
 - Master 會主動廣播給 (Slave)
 - Slave 不能主動發起通訊請求
 - Slave 必須有 唯一地址
 - 通常會透過 人機界面 (HMI) 或者 資料監控與擷取系統 (SCADA) 互動
 - 附屬裝置會包含感測器、程式化邏輯控制器 (PLC) 或 程式自動化控制器 (PAC)
 - 不包含任何安全性設計

## Modbus 運作方式

![communication graph](/2023/08/04/Modbus-Security/communication.jpg)
<small>備註: RTU (Remote Terminal Unit)</small>

## Modbus 通訊結構

![packket structure](/2023/08/04/Modbus-Security/packet.jpg)

## Data Unit
協議資料單元 PDU (Protocol Data Unit)
每個 PDU 包含 Function code 跟 Data
應用資料單元 ADU (Application Data Unit)
每個 ADU 都有一個協議資料單元 (PDU)
TCP/IP ADU:
```
  +------------+---------+--------+---------+--------------+
  | Transaction| Protocol| Length | Unit ID | Modbus PDU   |
  +------------+---------+--------+---------+--------------+
```

## PDU (1/2)
早期 Modbus 以序列為基礎的單一應用層協議，只有協議資料單元 (PDU)。
### PDU
 - Function code +252位元的函數資料
 - 附屬裝置(Ex.PLC)會檢驗 Function generator、Data address、Data range等資料後，接著會執行預期的動作。
 - 每個附屬裝置都要檢查 Function code, Input 數量, Begin address, Range 以及實際執行讀取。

 ```
  +---------------+------------------------+
  | Function code | Function specific data |
  +---------------+------------------------+
 ```

 ## PDU (2/2)

Protocol Data Unit 是 Modbus 的核心，他可以定義協議要用的資料型態跟存取方法。
Example: 讀取 40001 ~ 40002 的值

 ```
  +-----------------------+------------------------+
  | Function code         | Function specific data |
  | Read holding registers| Data address | Number  |
  +-----------------------+--------------+---------+
  | 03                    |  00  |  00   | 00 | 02 |
  +-----------------------+--------------+---------+
 ```


# 結語

 總結來說，Modbus作為一種串行通訊協議，在工業控制系統中扮演著重要角色。然而，它的缺乏安全性設計使得系統容易受到攻擊。
第一章節就在討論他的背景、封包架構、通訊模式，接者就會介紹 ADU，Function code 等等內容。
