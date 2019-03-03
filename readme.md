Lecture 02 Labs
===
This is the labs files, please clone this repo.

##
1. Under the path of **asm01**, use `make` to build

## asm02
1. Download the qemu pack [here](http://140.116.39.233/ese/02/gnu-mcu-eclipse-qemu.zip)
2. Extract into the **asm02** folder
3. Under the path of **asm02**, use `make` to build, and then `make qemu` to emulate
4. Use another terminal window to open gdb by command `arm-none-eabi-gdb`
5. Connect to qemu: `target remote 127.0.0.1:1234`


## HW02
改變 asm02.s 中 registers 的順序,以 qemu 運行,並以 gdb 觀察,
請以 Markdown 語法完成一份簡易報告,
紀錄於 readme.md ,並 push 至 github 繳交作業.
``` 	
push    {r1, r2, r3, r4}
pop     {r4, r5, r6, r7}
```
### 報告格式：
#### 1. 實驗題目
#### 2. 實驗步驟
#### 3. 結果與討論

* [Markdown 語法參考](https://markdown.tw)

## Markdown 簡易範例
```
最高階標題
===
```
最高階標題
===

---
```
第二階標題
---
```
第二階標題
---

---
```
# 最大字體標題
## 次大字體標題,以此類推
```
# 最大字體標題
## 次大字體標題,以此類推

---
```
*斜體*
**粗體**
```
*斜體*
**粗體**

---
```
* 無序清單
1. 有序清單
2. 有序清單
```
* 無序清單
1. 有序清單
2. 有序清單

---
```
`程式碼`
```
`程式碼`

---
```
    \```
    程式碼
    \```
```
```
    程式碼
```
---
```
[插入連結 ex: Google](https://www.google.com.tw)
```
[插入連結 ex: Google](https://www.google.com.tw)

---


HW02 範例
===
## 1. 實驗題目
將 ams02.s 中 最後 `bx    lr` 改為直接跳至 label `sleep`，觀察結果及理解原因。
## 2. 實驗步驟
1. 根據原始程式碼，`bx	lr`  lr 存的位置是 `bl    label01` 的下一行為 label `sleep` 之位置。
```c=63
	bl	lable01

sleep:
	b sleep



lable01:
	nop
	nop
	bx	lr
```
2. 根據 [ARM Information Center](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.dui0204ic/Cihfddaf.html) ，bx 屬於 op2，後方必須接 registers，如 lr 。
```
op2{cond} Rm

Rm
    是一个寄存器，包含要跳转到的目标地址。
```
如果要跳轉至指定 label 須使用屬於 op1 之指令如 `b, bl, blx`，後方接 label
```
op1{cond}{.W} label
```
根據 Arm Information Center ，bx 之定義
```
BX
    跳轉並切換指令集。
```
`b, bl, blx` 三者中只有 `blx` 能切換指令集，因此修改程式如下
```c=63
	bl	lable01

sleep:
	b sleep



lable01:
	nop
	nop
        @bx	lr
        blx    sleep
```

3. 以 qemu 運行並以 gdb 觀察
* 執行 `blx sleep` 前
```
┌──Register group: general─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│r0             0x20000000       536870912      r1             0x20000100       536871168      r2             0x9      9                      r3             0x64     100                      │
│r4             0x11000000       285212672      r5             0x66     102                    r6             0x67     103                    r7             0x11000001       285212673        │
│r8             0x0      0                      r9             0x0      0                      r10            0x0      0                      r11            0x0      0                        │
│r12            0x0      0                      sp             0x20000100       0x20000100     lr             0x3b     59                     pc             0x40     0x40                     │
│cpsr           0x173    371                    MSP            0x20000100       536871168      PSP            0x0      0                      PRIMASK        0x0      0                        │
│BASEPRI        0x0      0                      FAULTMASK      0x1      1                      CONTROL        0x0      0                                                                       │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
   ┌───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
   │0x2a    ldr    r1, [r0, #0]                                                                                                                                                                │
   │0x2c    ldr    r2, [r0, #4]                                                                                                                                                                │
   │0x2e    movs.w r0, #536870912  ; 0x20000000                                                                                                                                                │
   │0x32    str    r1, [r0, #0]                                                                                                                                                                │
   │0x34    str    r2, [r0, #4]                                                                                                                                                                │
   │0x36    bl     0x3c                                                                                                                                                                        │
   │0x3a    b.n    0x3a                                                                                                                                                                        │
   │0x3c    nop                                                                                                                                                                                │
   │0x3e    nop                                                                                                                                                                                │
  >│0x40    blx    0x3c                                                                                                                                                                        │
   │0x44    movs   r0, r0                                                                                                                                                                      │
   │0x46    movs   r0, r0                                                                                                                                                                      │
   │0x48    movs   r0, r0                                                                                                                                                                      │
   │0x4a    movs   r0, r0                                                                                                                                                                      │
   │0x4c    movs   r0, r0                                                                                                                                                                      │
   └────────────────────────────────────────────────────────────────────────
```
* 執行 `blx sleep`  後
```
┌──Register group: general─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│r0             0x20000000       536870912      r1             0x20000100       536871168      r2             0x9      9                      r3             0x64     100                      │
│r4             0x11000000       285212672      r5             0x66     102                    r6             0x67     103                    r7             0x11000001       285212673        │
│r8             0x0      0                      r9             0x0      0                      r10            0x0      0                      r11            0x0      0                        │
│r12            0x0      0                      sp             0x20000100       0x20000100     lr             0x45     69                     pc             0x3c     0x3c                     │
│cpsr           0x153    339                    MSP            0x20000100       536871168      PSP            0x0      0                      PRIMASK        0x0      0                        │
│BASEPRI        0x0      0                      FAULTMASK      0x1      1                      CONTROL        0x0      0                                                                       │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
   ┌───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
   │0x2a    ldr    r1, [r0, #0]                                                                                                                                                                │
   │0x2c    ldr    r2, [r0, #4]                                                                                                                                                                │
   │0x2e    movs.w r0, #536870912  ; 0x20000000                                                                                                                                                │
   │0x32    str    r1, [r0, #0]                                                                                                                                                                │
   │0x34    str    r2, [r0, #4]                                                                                                                                                                │
   │0x36    bl     0x3c                                                                                                                                                                        │
   │0x3a    b.n    0x3a                                                                                                                                                                        │
  >│0x3c    nop                                                                                                                                                                                │
   │0x3e    nop                                                                                                                                                                                │
   │0x40    blx    0x3c                                                                                                                                                                        │
   │0x44    movs   r0, r0                                                                                                                                                                      │
   │0x46    movs   r0, r0                                                                                                                                                                      │
   │0x48    movs   r0, r0                                                                                                                                                                      │
   │0x4a    movs   r0, r0                                                                                                                                                                      │
   │0x4c    movs   r0, r0                                                                                                                                                                      │
   └────────────────────────────────────────────────────────────────────────
```
* 再執行一次 `si`
```
┌──Register group: general─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│r0             0x20000000       536870912      r1             0x20000100       536871168      r2             0x9      9                      r3             0x64     100                      │
│r4             0x11000000       285212672      r5             0x66     102                    r6             0x67     103                    r7             0x11000001       285212673        │
│r8             0x0      0                      r9             0x0      0                      r10            0x0      0                      r11            0x0      0                        │
│r12            0x0      0                      sp             0x20000100       0x20000100     lr             0x47     71                     pc             0x3c     0x3c                     │
│cpsr           0x153    339                    MSP            0x20000100       536871168      PSP            0x0      0                      PRIMASK        0x0      0                        │
│BASEPRI        0x0      0                      FAULTMASK      0x1      1                      CONTROL        0x0      0                                                                       │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
│                                                                                                                                                                                              │
   ┌───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                   [ No Assembly Available ]                                                                                               │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   │                                                                                                                                                                                           │
   └───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
None No process In:                                                                                                                                                                L??   PC: ??
0x00000022 in ?? ()
0x00000024 in ?? ()
0x00000026 in ?? ()
0x00000028 in ?? ()
0x0000002a in ?? ()
0x0000002c in ?? ()
0x0000002e in ?? ()
0x00000032 in ?? ()
0x00000034 in ?? ()
0x00000036 in ?? ()
0x0000003e in ?? ()
0x00000040 in ?? ()
0x00000042 in ?? ()
0x0000003c in ?? ()
Remote connection closed
(gdb)
```
qemu 的模擬掛了 why??
而且`blx sleep` 在 qemu 中卻跳至 `0x3c` 而不是 label `sleep` 所在的 `0x3a` why??

## 3. 結果與討論
根據 [ARM and Thumb instruction set overview](http://www.keil.com/support/man/docs/ARMASM/armasm_dom1359731139853.htm)

* All ARM instructions are 32 bits long. Instructions are stored word-aligned, so the least significant two bits of instruction addresses are always zero in ARM state.
* Thumb instructions are either 16 or 32 bits long. Instructions are stored half-word aligned. Some instructions use the least significant bit of the address to determine whether the code being branched to is Thumb code or ARM code.

`blx sleep` 的 `sleep` 的 least significant bit 為 0 ，所以將轉換為 ARM instruction 來執行後續的指令，而 ARM instruction 為一個字節對齊，所以 compiler 自動將 `0x3a` 補齊至 `0x3c` 。

[Cortex-M4-ARM Developer](https://developer.arm.com/products/processors/cortex-m/cortex-m4) 表示 Cortex-M4 的 ISA Support Thumb/Thumb-2 指令，不支援 ARM 指令，因此若切換至 ARM 指令，並且執行的話會產生 Exception (UsageFault) 造成 qemu 關閉。
