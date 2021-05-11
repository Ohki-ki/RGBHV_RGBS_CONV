# Note for Rev1.0B MAY/11/2021
このノートはとりいそぎ書いており、そのうち消します。
Rev1.0Bのソース、ガーバーをupしました。
対応ロジックは以前より減って、CMOS3.3V駆動可能な74HC86/74AC86/74AHC86のいずれかになります。
レギュレータは3.3Vで50mAくらい出る物を使ってください。とりあえずピン配列の違う２つを用意しときました。どっちか適当に使ってください。HT7533-1あたりが安くてオススメです。
構成部品が結構変わっています。写真はそのうち入れ替えます。

# RGBHV_RGBS_CONV
Display/Monitor video signal converter PCB (RGBHV &lt;--> RGBS ./ etc. )

  
  
***
# 概要
RGBHVまたはRGBSを使用するディスプレイ信号線の、コネクタ変換、信号変換、信号分岐を行うボードです。  
プロジェクトにはKiCad PCB 5.0用のプロジェクトファイルとガーバーが含まれます。

Rev0.5A
![Rev5a converter pcb_photo with GBS-8200](https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/rev5a_photo.jpg)  
Rev1.0B
![Rev10b converter pcb_photo 1](https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/REV10B1.jpg)  
![Rev10b converter pcb_photo 2](https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/REV10B2.jpg)  


## PCB Artwork
Rev0.5b  
![Rev5b converter pcb_art 1](https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/10B_SIDEA.png)  
![Rev5b converter pcb_art 2](https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/10B_SIDEB.png)  
  
  
***
# Release note
## Rev 1.0B release
2021/MAY/11  
3.3Vレギュレータによる電源安定化
3.3V駆動の5VトレラントCMOSロジックでGBS出力の無調整化(SN74AHC86推奨)
GBS8200のCSYNC入力ラインに100Ωのプルダウン抵抗を付けるパターンをなんとなく追加(GBS無改造で同様の効果)
DA15のPC8801m2SR仕様のオーディオ信号を3.5mmステレオジャックに引き出した

## Rev 0.5B release
2020/JUN/10  
5BのKicadソースコードをアップロード
CC BY-SA 4.0とMITライセンスのデュアルライセンス化

## Rev 0.5B pre-release
2020/MAR/25  
RGBS OUTのJ15をVRで電圧降下させるパターンを追加。
検証済み。2KRの多回転VRを使用し、GBS8200/8220のR34が1KRの場合はVRの値を1KRに、R34が2.2KRの場合は1.4KRに設定して、現物合わせで微調整してください。
訂正：1KRで300R、2.2KRで450R

## Rev 0.5A release
2019/AUG/28  
あまり使わないコネクタ、SYNCセパレータを削除。
エッジ端のDA15コネクタは動作未確認。

## Rev 0.4A release candidate
2019/MAR/04  
コネクタ位置を調整。
動作確認中。

## Rev 0.4
2019/MAR/04  
初公開。X1用のDIN6ピンコネクタは動作未確認です。他は動作確認済み。  
2019/MAR/31  
X1用のDIN6ピンコネクタ動作確認  
  
  
  
***
# 作り方
## 手順
1. 部品表に従い、各部品をはんだ付けしてください。  
1. 向きがある部品は写真を参照してください。  

## 部品表(BOM)
## 部品表(BOM)
| Ref | Name | Qty./Note |
|:---|:---|:---|
|J2 |DE15 VGA connector | N/A |
|J3 |DA15 connector | N/A |
|J1 |DIN 8pin connector (e.g.HOSIDEN TCS4480-0140577) | N/A |
|J4 |DIN 6pin connector (e.g.HOSIDEN TCS4460-0140577) | N/A |
|J5,J7,J9,J11,J15 |JST B8B-XH-A connector | 5pcs |
|J16 |JST S8B-XH-A connector | N/A |
|J17 |JST S2B-XH-A connector | N/A |
|J6 |16pin 2.54mm pitch IDC BOX-HEADER | N/A |
|J8 |10pin 2.54mm pitch IDC BOX-HEADER | N/A |
|R1-R3,R5,R6,R9 | Resistor 150Ω 1/4W | 6pcs |
|R8 | Resistor 10KΩ 1/4W | N/A |
|C1,C3,C5 | Capacitor 0.1uF CC or MLCC | 3pcs |
|C4,C6 | Capacitor 10uF CC or MLCC | 2pcs |
|U1 | 14pin DIP IC SOCKET 300mil | N/A |
|U3 | HT7533-1 3.3V LDO | N/A |
| N/A | CMOS XOR gate logic ic (74HC86/74AC86/74AHC86) | install to U1 |


  
  
***
# デザインと使い方解説
## 構成

ボードはアナログRGBS信号コネクタ、アナログRGBHV信号コネクタ、デジタルRGBHV信号コネクタ、デジタルアナログ変換回路、同期信号合成回路の5パートで構成されます。  
![Descroption](https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/part_dsc.jpg)  

RGBの信号はデジタルRGBHV部とその他で、同期信号はアナログRGBS信号部とその他で分離しており、各コネクタは同一バス上に配置されています。  
各部２つづつある8ピンXHコネクタを使って、任意の信号位置で複数ボードをカスケード接続可能です。  

### アナログRGBS信号コネクタ
8ピンのXHコネクタが２つ。出力に使います。  
2ピンの電源コネクタといずれかの8ピンコネクタをGBSと接続します。（接続ケーブルはopposite sideです。安く売ってるsame sideだと映りません）

### アナログRGBHV信号コネクタ
8ピンのXHコネクタが2つ、IDC DA15コネクタ用16ピンヘッダ、IDC 10ピンヘッダ、PCBマウントDA15コネクタ、VGA DE15コネクタの、合計6つのコネクタがあり、入出力両方に使います。
DA15コネクタから音声信号を引き出してあります。
アナログRGBSとは同期信号が分離しており、HVからSへは同期信号合成回路を通過します。 
J6コネクタには、リボンケーブルを使用し、DA15のIDCコネクタを付けることでDA15コネクタを増設できます。  

### デジタルRGBHV信号コネクタ
ここで言うデジタルRGBは、主に日本で1980年代頃のパソコンに搭載されていた仕様で、8色表示のものに対応しています。RGBIの16色には対応していません。  
8ピンのXHコネクタが２つ、8ピンDINが1つ、6ピンDINが1つ、合計4つのコネクタがあり、入力に使います。  
8ピンDINはPC9801、PC8801シリーズ等で使われていたピン配置にしてあり、給電は無視します。  
6ピンDINはSHARP X1シリーズ用(turbo model40を除く)のピン配置にしてあります。 X1 turbo model40は8ピンDINコネクタを使用すれば表示可能です。
これら2つのDINコネクタは、各PC標準のケーブルではなく、オス-オス仕様のDINコネクタストレートケーブルを使用します。  

### デジタルアナログ変換回路
150Ωの抵抗でデジタルRGB信号を、アナログRGB信号のレベルに変換します。  

### 同期信号合成回路
XORゲートICを使用してHSYNC信号とVSYNC信号からCSYNC信号を作ります。
5Vトレラントな3.3Vで動作する74シリーズCMOS XORロジック(74HC86/74AC86/74AHC86)を使用します。推奨はSN74AHC86Nです。
電源はGBSから（5V-12V）入力します。コンデンサ等は耐圧16V以上がおすすめです。

## 動作確認済機種
### Video signal src.
- IBM-PC compatible (VGA)  
- PC-8801mk2 (Digital RGB)  
- PC-8801mk2MR/FH (Digital RGB/Analog RGB DA15)  
- PC-9801DA/U2 (Analog RGB DA15)  
- PC-9801VX41 (Digital RGB/Analog RGB DA15)  
- X1 Turbo model40 (Digital RGB)  
- X1 Turbo model30 (Digital RGB)  
- Arcade-Game PCBs (クイズココロジー2／上海2／子育てクイズ マイエンジェル）
- 上記の他、PC8001シリーズ(Digital RGB)で動作報告をいただいております。ありがとうございます。
### Video signal dst.
- GBS-8200 (RGBS)
- Some VGA monitors (DE15/VGA)

  
  
***
# Lastest Gerber
## 1.0B 
https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/RGBHV_RGBS_CONV10B2.zip

## Arcive
https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/RGBHV_RGBS_CONV05B.zip
https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/RGBHV_RGBS_CONV05A.zip
https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/RGBHV_RGBS_CONV_04A.zip  
https://github.com/antarcticlion/RGBHV_RGBS_CONV/blob/master/RGBHV_RGBS_CONV_04.zip  

  
  
***
# Changes Log  
| Revision | Date | Description |
|:---|:---|:---|
|1.0B release |2021/05/11 | 3.3Vレギュレータによる電源安定化、3.3V駆動の5VトレラントCMOSロジックでGBS出力の無調整化(SN74AHC86N推奨) |
|0.5B release |2020/06/10 | RGBS出力に多回転VRを追加し、出力レベルを下げられるコネクタを用意。(GBS8200/8220のCSYNC信号不安定対策) |
|0.5A release |2019/08/28 | あまり使わないコネクタと、動悸分離回路を削除、RGBHV DA15のエッジ端コネクタを追加 |
|0.4A release candidate |2019/03/04 | コネクタ位置調整、DCジャック追加 |
|0.4 |2019/03/04 | first release. (experimental) |
|0.1～0.3 | N/A | private |



***
# ライセンス

※Rev.5Bより、CC BY-SA 4.0とMITライセンスのデュアルライセンスとします。

クリエイティブコモンズ　表示 - 継承 4.0 国際 (CC BY-SA 4.0)

https://creativecommons.org/licenses/by-sa/4.0/  
https://creativecommons.org/licenses/by-sa/4.0/legalcode

---

Copyright 2019-2020 @antarcticlion

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

