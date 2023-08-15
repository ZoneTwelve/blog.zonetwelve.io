---
title: Modbus Security pt.2
date: 2023-08-15 20:13:45
tags:
  - Modbus
  - Security
  - Infrastructure
---

# 引言

續先前[『Modbus Security Part 1』](/2023/08/04/Modbus-Security/)，我們已經對 PDU 的基礎結構有所認識，這次就將目標鎖定在 ADU，並深入討論 Modbus 資料儲存區的結構。

<!-- more -->
# Modbus

## ADU

ADU (Application Data Unit)，是應用程式交換資料的完整封包，ADU 也會因為不同的協議基礎，進而有些微差異，接著就要來說說有常用的架構。

除了Modbus 的 PDU 核心定義的功能以外，根據不同資料也可以包含其指定的內容。

### 常見的架構

- 常見的 ADU 類型
  - TCP
  - RTU
  - ASCII
- 常見的通訊模組
  - TCP/IP (Modbus TCP)
  - RS485 (Modbus RTU, Modbus ASCII)

### ADU Package
```markdown
+---------------------------------------------+
|  Modbus Application Protocol (MBAP) Header  |
+-------------+----------+---------+----------+
| Transaction | Protocol | Length  |   Unit   |
|  Identifier |Identifier| ......  |Identifier|
+-------------+----------+---------+----------+
|  00  |  01  | 00 | 00  | 00 | 00 |    01    |
+-------------+----------+---------+----------+
|   2 Bytes   | 2 Bytes  | 2 Bytes | 1 Bytes  | 
+---------------------------------------------+
```
由前至後
- TI: 用來辨識傳送與接收封包
- PI: Modbus 固定為 0
- Len:後續資料長度
- UI: Slave 設備的辨識碼 (Slave 1 ~ 247)

### Example Package

Protocol: Modbus TCP
讀取 Device ID 1 設備上於 40001 ~ 40002 位置的值
```
+---------------------------------------------------------------------------+
|  Modbus Application Protocol (MBAP) Header   | Modbus PDU                 |
+----------------------------------------------+----------------------------+
| Transaction ID | Protocol | Length  | UnitID | FC | DataAddress | Number  |
+---------------------------------------------------------------------------+
|  00  |   01   | 00 |  00  | 00 | 06 |   01   | 03 |  00  |  00  | 00 | 02 |
+---------------------------------------------------------------------------+
```

## 資料儲存區

### 資料組架構

我們已經知道 ADU 的範例長得如何，這次我們可以透過圖表來了解 Modbus 資料儲存區的種類。
名稱也可能會基於產業或應用而有所不同，資料組間定義了資料內容以及存取權限。

| 記憶體區塊 | 資料類型 | 主機裝置存取 |
|------------|----------|--------------|
| Coil Status| Boolean  | 讀/寫        |
| Discrete Inputs | Boolean | 讀       |
| Input Register | unsigned char | 讀  |
| Holding Register | unsigned char | 讀/寫|

### 資料對應表

PLC 資料於記憶體對應的 Modbus 位址
1. Coils Status (0:1～65535)
   以 00001 表示，儲存邏輯數據，可寫入，用於控制。
2. Discrete Inputs (1:1～65535)
   以 10001 表示，儲存邏輯數據，只可讀。
3. Input Register (3:1～65535)
   以 30001 表示，儲存數值數據 (0～65536; 2 Bytes)，只可讀。
4. Holding Register (4:1～65535)
   以 40001 表示，儲存數值數據 (0～65536; 2 Bytes)，通常可寫，用於控制。

### Modbus 資料位址設定範圍
編號機制，於資料位址加上 prefix。

Example:
 - 保存暫存器 14, 以 4,014 或 40,014 或 400,014 表示。
 - 常見範圍: 4001 ~ 4999, 40001 ~ 49999

### Modbus 資料位址起始值

根據實作方式所選擇的索引方式，會有不同記憶體地址。

| 地址 | 暫存器編碼 | 編號(索引 1) | 編號(索引 0) |
| 0    | 1          | 400001       | 4000000      |
| 1    | 2          | 400002       | 4000001      |
| 2    | 3          | 400003       | 4000002      |

# 結論

瞭解 Modbus 的資料儲存區操作，對於避免資料存取出錯或是最佳化存取效率都會有所幫助。此外，對於在網路環境中使用 Modbus 的工程師而言，了解 ADU 的結構以及其與 PDU 的關聯也極為重要，能有效地提升資料傳輸的安全性與效率，對於資訊安全研究者是截然不可或缺的一部分。

