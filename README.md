# CIRILYライブラリ
このライブラリは、Felicaを日本語プログラミング言語「なでしこ」で動作させる為の諸々です。　　
未完成、β版です。とりあえず掲載しておきましたって感じです。　　
　　
# 使い方
とりあえず仮で移植しただけなので、日本語に不自然な面や、最適化が済んでいません。　　
将来的にはなでしこの「部品」として動作させたいと思っています。　　
Vistaダイアログ命令を使っているので、Windows Vista以上で動作すると考えてください。　　

## PaSoRi周りの命令
### PaSoRi開く
Felica.dll (Felicalib) をロード、初期化します。　　
これの返り値に、PaSoRiハンドルが含まれています。　　

### PaSoRi初期化
名の通り、PaSoRiを初期化します。　　
初期化に成功すると、0が返ってきます。　　
書式は下記の通りです。　　
~~~
PSR_HNDL(で|を|の)PaSoRi初期化
~~~
PSR_HNDLには、PaSoRi開く命令で得られたPaSoRiハンドルの値が入ります。　　
　　
### PaSoRi閉じる
名の通り、PaSoRiとの通信を終了します。　　
これにより、Felica.dllが解放されます。　　
書式は下記の通りです。　　
~~~
PSR_HNDL(で|を|の)PaSoRi閉じる
~~~
PSR_HNDLには、PaSoRi開く命令で得られたPaSoRiハンドルの値が入ります。　　
　　
## Felica周りの命令
### Felica取得
FeliCaカードが置かれているか確認する命令です。(ポーリングです)　　
これの返り値には、Felicaハンドルが含まれています。　　
書式は下記の通りです。　　
~~~
TYPEのPSR_HNDL(で|を|の)Felica取得
~~~
このTYPEには、　　
- POLLING_ANY → なんでも読み込みます　　
- POLLING_EDY → EDYを読み込みます　　
- POLLING_SUICA → 交通系ICを読み込みます　　
のいずれかが入ります。　　
PSR_HNDLには、PaSoRi開く命令で得られたPaSoRiハンドルの値が入ります。　　

### Felica解放
Felicaハンドルを解放します。　　
別のカードを読み出す際や，通信に失敗した場合はこれを呼び出してから再度Felica取得命令を使用する必要があります。　　
~~~
FLC_HNDL(で|を|の)Felica解放
~~~
FLC_HNDLには、Felica取得で得られたFelicaハンドルを使用します。　　

### Felica製造ID取得
FelicaのIDm (製造ID) を取得します。　　
これの返り値には、IDmが含まれています。　　
書式は下記の通りです。　　
~~~
FLC_HNDL(で|を|の)Felica製造ID取得
~~~
FLC_HNDLには、Felica取得で得られたFelicaハンドルを使用します。　　

### Felica製造パラメータ取得
FelicaのPMm (製造パラメータ) を取得します。　　
これの返り値には、PMmが含まれています。　　
書式は下記の通りです。　　
~~~
FLC_HNDL(で|を|の)Felica製造パラメータ取得
~~~
FLC_HNDLには、Felica取得で得られたFelicaハンドルを使用します。　　

### Felica非暗号化塊読み込み
Felicaの暗号化されていないブロックを取得します。　　
Suicaの履歴とかを読み込む際に使えます。　　
これの返り値には、16バイトの16進数の数字が返ってきます。　　
書式は下記の通りです。　　
~~~
NUMのTYPEをFLC_HNDL(で|を|の)Felica製造パラメータ取得
~~~
NUMには、読み込みたいブロック番号が入ります。(0~255)　　
TYPEにはサービスコードが入ります。(交通系ICだけはSERVICE_SUICA_HISTORYを代入する事が出来ます)　　
FLC_HNDLには、Felica取得で得られたFelicaハンドルを使用します。　　

## エラー類諸々
このライブラリ (というより関数集) では、エラーはVistaダイアログとして表示してくれる機能があります。　　
面倒な人は、とりあえず今のところはコメントアウトして使ってください。　　

### 「PaSoRiの初期化に失敗しました」
このエラーはPaSoRi初期化命令で発生します。　　
これは、PaSoRiが接続されていない際に発生します。　　
USB接続型PaSoRiの場合、正常に接続されているか、ご確認ください。　　

### 「Felicaの読み込みに失敗しました」
このエラーはFelica取得命令で発生します。　　
これは、カードが認識できないと発生します。　　
残念ながら、今のバージョンでは「読み取り待ち」状態にはできません。　　
なので、一定時間で繰り返す命令を作ってください。　　

### 「Felicaのブロック読み込みに失敗しました」
このエラーはFelica非暗号化塊読み込みで発生します。　　
これは、カードが読み込めないと発生します。　　

# ライセンス
乗っけてるFelicalib　(Takuya Murakami氏) 自体は3条項BSDライセンスです。　　
CIRILYライブラリも、同じく3条項BSDライセンスを採用します。　　
