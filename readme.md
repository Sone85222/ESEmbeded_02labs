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
改變 asm02.s 中
``` 	
push	{r1, r2, r3, r4}
pop	    {r4, r5, r6, r7}
```
registers 的順序,以 qemu 運行,並以 gdb 觀察,
請以 Markdown 語法完成一份 **簡易報告**,
紀錄於 readme.md ,並 push 至 gtihub 繳交作業.
[Markdown 語法參考](https://markdown.tw)
Markdown 簡易範例
```
最高階標題
===
```
最高階標題
===

```
第二階標題
---
```
第二階標題
---

```
# 最大字體標題
## 次大字體標題,以此類推
```
# 最大字體標題
## 次大字體標題,以此類推

```
*斜體*
**粗體**
```
*斜體*
**粗體**

```
* 無序清單
1. 有序清單
2. 有序清單
```
* 無序清單
1. 有序清單
2. 有序清單

```
`程式碼`
```
`程式碼`
```
[插入連結 ex:Google](https://www.google.com.tw)
```
[插入連結 ex:Google](https://www.google.com.tw)
