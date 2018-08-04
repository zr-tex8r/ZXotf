ZXotf パッケージバンドル
========================

LaTeX： XeLaTeX で Adobe-Japan1 CID 番号によるグリフ出力を行う

Adobe-Japan1 のグリフ集合をもつ CID-keyed OpenType フォントについて、
XeLaTeX で CID 番号によるグリフ出力を可能にする。齋藤修三郎氏作の
[japanese-otf パッケージ]の `\CID` 命令を XeLaTeX で使うためものなので
ZXotf という名前にした。

[japanese-otf パッケージ]: https://ctan.org/pkg/japanese-otf

### 対応環境

  * フォーマット： LaTeX
  * エンジン： XeTeX
  * 前提パッケージ：
      - BXbase（0.4 版以降）
      - japanese-otf： `macros` オプション使用時

### インストール

TDS 1.1 に従ったシステムでは、各ファイルを次の場所に移動する。

  - `*.sty`          → $TEXMF/tex/xelatex/zxotf/
  - `zxotc2u.tfm`    → $TEXMF/fonts/tfm/public/zxotf/

※ XeTeX なのに TFM ファイルが要るのは、論理フォントとは全く別の目的で
使われているからである。


zxotf パッケージ ― 本体
------------------------

### 読込

プレアンブルにおいて、`\usepackage` を用いて読み込む。
                                      
    \usepackage[<オプション>,...]{zxotf}

オプションは次のものが用意されている。

  * `macros`： japanese-otf の ajmacros パッケージを読み込む。
  * `nomacros`（既定）： ajmacros パッケージを読み込まない。

### 機能

#### Adobe-Japan1 CID 入力

  * `\CID{<符号値>}`： 指定された AJ1 CID のグリフを出力する。
  * `\UTF{<符号値>}`： 指定された Unicode 値の文字を出力する。  
    ※これは該当の文字を直接書くのと同じで、フォント切替等は行わない。

bxbase の `\AJ` は zxotf の `\CID` と連携する。zxotf 自身が bxbase を内部
で読み込むので次の命令も使えることになる。

  * `\AJ{<符号値>,...}`：［bxbase の命令］ `\CID` と同じだが、複数入力
    指定可能。

#### Adobe-Japan1 グリフ集合用のフォント設定

  * `\setcidminchofont[<属性リスト>]{<フォントファミリ名>}`  
    `\setcidgothicfont[<属性リスト>]{<フォントファミリ名>}`： `\CID` に
    よるグリフを出力するのに用いるフォント（ファミリ）を指定する。
    `\setcidminchofont` は明朝体、`\setcidgothicfont` はゴシック体を想定
    し、現在のファミリが `\sffamily` か `\ttfamily` の場合はゴシック、
    `\rmfamily` の場合は明朝が使われる。
    `<属性リスト>` と `<フォントファミリ名>` の指定方法は fontspec の
    ものと同じである。

    指定されていない場合は、現在のフォントがそのまま使われる。(zxjatype
    読込時は、まず和文フォントに切り替える。) 既定値は無指定である。

#### japanese-otf の ajmacros パッケージ読込機能

パッケージオプションに `macros` を指定すると、japanese-otf バンドルの
ajmacros パッケージを読み込む（japanese-otf が正しくインストールされて
いれば読み込めるはずである）。これで、ajmacros で提供される `\ajMaru`、
`\ajLig`、`\○` 等の AJ1 の多様な文字（グリフ）を明快な名前で入力する
命令が使えるようになる。

ajmacros.sty ファイルの文字コードは UTF-8 または ISO-2022-JP（所謂 JIS
エンコーディング）である必要がある。(u)pLaTeX 用のパッケージファイルは
ISO-2022-JP にしているシステムが多いので ISO-2022-JP を読めるようにして
いるのだが、この場合、`\aj半角` 等の一部の命令が正常に動作しないようで
ある。この問題を避けるには、ajmacros.sty を UTF-8 に変換したファイルを
zxotf.sty と同じディレクトリに置けばよい。この場合でも、pTeX/upTeX からは
従来の ISO-2022-JP のものが使われるはずである。

#### Unicode 出力によるエミュレート

AJ1 用に指定されたフォント（無指定で現在のものがそのまま使われる場合も
含む）が CID-keyed でない場合は、**非漢字に限って** Unicode 入力による
エミュレートが行われる。すなわち、指定の AJ1 のグリフに対応する Unicode
文字がある場合はその文字が出力され、ない場合（および漢字の場合）はゲタ
記号〈〓〉が出力される。漢字を省いたのは、異体字の問題があって対応が単純
でないという他に、そもそもこの機能が専ら ajmacros を利用するためのものと
考えられるからである。（このため、`\ajHashigoTaka`、` \ajTsuchiYoshi`、
`\ajTatsuSaki` は特別に対応している。）


更新履歴
--------

  * Version 0.2b ‹2018/08/04›
      - fontspec 2.6f 版以降に対応。

  * Version 0.2a ‹2009/11/18›

  * Version 0.2  ‹2009/11/16›
      - 最初の公開版。

--------------------
Takayuki YATO (aka. "ZR")  
https://github.com/zr-tex8r
