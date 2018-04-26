# ラズベリーパイでNekoniumノードを作る手順

### 概要
クライアントPCにWindowsを使い、raspbianをヘッドレスモードで（ラズベリーパイにディスプレイをつながずに）インストールし、gnekoniumをビルドし動作させます。


## 用意するもの
クライアントPC （この記事ではWindows10を使用します）
Raspberry Pi 2/3
ACアダプタ（5V 2.5A以上のもの）
MicroSDカード（16GB以上のもの）
LANケーブル
MicroSDｰSD変換アダプタ
SDｰUSB変換器
（その他必要に応じて:ケース、スイッチングハブ）

## OSイメージのダウンロード

リンク先から、最新のOSイメージの<B>LITE</B>版をダウンロードします。
https://www.raspberrypi.org/downloads/raspbian/

![Download_os_img](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/download_os_img.jpg)


## OSイメージの書き込み
ダウンロードしたものは、ディスクイメージと呼ばれるものなので、そのままコピーしても動作させることはできません。
win32diskimagerというソフト等を使ってイメージをディスクに展開します。
<b>免責事項 - SDカードをフォーマットあるいはイメージを書き込む場合、そのカードに保存されているすべての情報が削除されることに注意してください。nekonium開発チームは 、フォーマットあるいはイメージの書き込みに伴い発生する可能性のあるデータ損失について責任を負いません。</b>
<details><summary>win32diskimagerのダウンロード</summary><div>

https://ja.osdn.net/projects/sfnet_win32diskimager/releases/

![download_diskimager](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/download_diskimager.jpg)
</div></details>

<details><summary>イメージの書き込み</summary><div>

書き込むSDカード以外のUSBストレージデバイスを可能な限り外します。

ダウンロードしたイメージファイルを指定し、書き込み先のSDカードのあるドライブを間違えないように指定し、書き込みます。

![diskimager](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/diskimager.jpg)
</div></details>
書き込みが終わったら、書き込んだドライブのルートディレクトリに、SSHという名前のファイル（拡張子なし）を作ります。

<details><summary>SSHというファイルを作る方法</summary><div>

書き込んだドライブをExplorerで表示します。
拡張子が表示されていない場合は、拡張子を表示するようにしておきます。
![view_ext](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/view_ext.jpg)

Explorerの右ペインの空いてる部分で（ほかのファイルを選択しないように）右クリックして新規作成、テキストドキュメントと選択していきます。
![create_txt](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/create_txt.jpg)

"新しいテキストドキュメント.txt"と出てくると思いますので、.txtも含めて全部消去して"ssh"と書きます。

拡張子を変更すると、ファイルが使えなくなる可能性があります。変更しますか？
と警告が出ますが、そのままはいを選択し変更します。
![save_ssh_file](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/save_ssh_file.jpg)

</div></details>


SSHというファイルを作り終わったら、USBメモリ型のアイコンをクリックして、USB変換器を選択し、安全に取り外します。
![safe_eject](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/safe_eject.jpg)
![safe_eject_hardware](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/safe_eject_hardware.jpg)

## ラズベリーパイの起動とローカルIPアドレスの取得

OSイメージを書き込んだばかりのラズベリーパイを起動するときは、DHCPによってIPアドレスが決定されます。
そのため、新規インストールした（DHCP接続の）ラズベリーパイを探せるソフトwifiguardをダウンロードして展開します。

https://www.softperfect.com/download/

![download_wifiguard](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/download_wifiguard.jpg)

これを使ってラズベリーパイを起動する前にネットワーク構成を読み込み、<b>すべてに</b>マークしておきます。
<details><summary>マーキングの方法</summary><div>

1.ソフトを起動する
2.ソフトのウィンドウの左下がアイドル状態になるまで待つ
3.IPアドレスの左側のオレンジ色のIPアドレスを選択してプロパティを開く
4.この機器に問題はありませんにチェックを入れる

![wifiguard_check](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/wifiguard_check.jpg)
</div></details>

マーキングが終わったら、1.で書き込んだMicroSDカードをラズペリーパイに差し込み、LANケーブルをつなげ、最後にACアダプタをつないでラズペリーパイを起動させます。

## SSHでの接続

<details><summary>ターミナルソフトのダウンロード</summary><div>

SSH接続するためのクライアントソフトが必要になるので持っていない場合ダウンロードします。
teraterm: https://ja.osdn.net/projects/ttssh2/

</div></details>

<details><summary>IPアドレスの確認</summary><div>

ラズベリーパイのLEDを見て、点滅が落ち着いてきたら、先ほどのwifiguardをもう一度動かします。

そうすると、新しく接続されたラズベリーパイがオレンジのマークで表示されているはずなので、そのIPアドレスを控えます。

![wifiguard_seach_ip](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/wifiguard_search_ip.jpg)
</div></details>



"IPアドレスの確認”で控えたIPアドレスにターミナルソフトで接続します。

TCP/IPを選択し、Host（先ほど控えたIPアドレス）とポート番号（デフォルトで22になっていると思います）を入力します。
サービスはSSHを、SSHバージョンはSSH2を選択します。

![terminal_connect](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/terminal_connect.jpg)
OKを押して進みます。

この時点で控えたIPアドレスに接続できそうにないときは、1－3をやり直してみてください。特にOSイメージを書き込んだ後にSSHというファイルを作っているかを確認してください。

初めて接続するとき、security_warningというごちゃごちゃっとした文字列が表示されると思いますが、してcontinueを押して次に進みます。
![teraterm_security_warning](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/teraterm_security_warn.jpg)
User nameとPassphraseを入力します。
この時点ではUsernameにpi, Passphraseにraspberryを入力します。

![terminal_auth](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/terminal_auth.jpg)

OKを押して進みます。

この画面のようになれば成功です。
![terminal_connected](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/terminal_connected.jpg)

### パスワードの変更

続いてパスワードを変更します。（IoTデバイスに対する攻撃が急増していますので、必ず変更してください）
パスワードには推測されやすい単語を利用しないようにして10文字以上のランダムな文字・数字・記号を使いましょう。大文字、小文字は区別されます。
```bash
$passwd
```
上記を実行します。
現在のパスワード、raspberryを入力してEnterキーを押します。ただし入力した文字は表示されません。
次に、新しいパスワードを入力しEnterキーを押します。これも入力した文字は表示されません。
もう一度、新しいパスワードで入力したパスワードを入力しEnterキーを押します。表示されていないので、間違えていないか確認するためです。
ここで入力したパスワードは忘れないようにしてください。
次回からのログイン時に必要になります。

![change_password](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/change_password.jpg)

### タイムゾーンの変更
デフォルト設定だとタイムゾーンがUTCなので、JSTに変更したい場合は次のコマンドを実行します。
```bash
$sudo dpkg-reconfigure tzdata
```
地域の選択画面が出てきますので、カーソル移動キーを使ってAsia→tを押してTokyoの近くに移動してから再びカーソル移動キーを使ってTokyoを選択しOKを押すと完了です。
![tz_asia](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/tz_asia.jpg)
![tz_tokyo](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/tz_tokyo.jpg)


### IPアドレスの固定
現在の設定だと電源を入れるたびに違うIPアドレスになる可能性があるので、IPアドレスを固定します。

まず、現在のネットワークの設定を見るために、次のコマンドを実行します。
```bash
$ip route
```

![ip_route](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/ip_route.jpg)
赤色の下線の部分がルーターアドレス
黄色の下線の部分がインターフェイス名
水色の下線の部分が現在のIPアドレス
緑色の下線の部分がネットワークアドレスと、/（スラッシュ）の後ろがサブネットマスクbit数
<details><summary>サブネットマスクbit数が24以外の場合</summary><div>

ネットワークアドレスを2進数での表現を書くか、早見表を作ってしまう。
</div></details>
<br>
現在のIPアドレスの<b>4つ目</b>の値を、今回は105に変更します。1-3番目の数字は、ipコマンドで取得したご自分の環境の値を利用してください。
（本来はサブネットマスクのbit数が24の場合、2－254の値のうち、DHCPの自動割り当て範囲外かつ利用されていない値に変更しますが、DHCPの自動割り当て範囲はルーター等のDHCPサーバの設定を直接確認しなければならないため、ここでは説明しきれないのであまり使われていないであろう値に決め打ちしています）

ここで得た情報を使って、次のコマンドを順に実行します。
(わかりやすいように上図の値を例に使っています。太字の部分をご自分の環境で取得した値に置き換えて実行してください)
```bash
$cd /etc
$sudo cp dhcpcd.conf dhcpcd.conf.orig
$sudo cat << EOF >> dhcpcd.conf
>interface eth0
>static ip_address=192.168.1.105/24
>static routers=192.168.1.1
>static domain_name_servers=1.1.1.1
>EOF
$
```

次のコマンドを実行して、ファイルの末尾にechoコマンドを使って追記した文字列が記載されているか確認します。

```bash
$tail -n4 dhcpcd.conf
```
![tail_dhcpcd](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/tail_dhcpcd.jpg)

ビルドした実行ファイルを置くためのディレクトリを作っておきます。
```bash
$mkdir bin
```
ここまで出来たら再起動します。次のコマンドを実行します。

```bash
$sudo shutdown -r now
```

ラズベリーパイが再起動しますので、接続しているteraterm等のターミナルソフトも接続がいったん切れます。
設定したIPアドレスに、ユーザー名piと新しく設定したパスワードを入力して再接続します。

### gnekoniumのダウンロードとビルド

コンパイルしてネイティブバイナリを作る場合
もしネイティブなバイナリを必要とするなら、ラズペリーパイ上でgnekoniumをコンパイルすることが出来ます。
gnekoniumはGoを使って書かれていますので、Goをコンパイルできる環境を作る必要があります。

<b>下準備</b>
```bash
$sudo apt-get -y update
$sudo apt-get -y upgrade
$sudo apt-get -y dist-upgrade
$sudo apt-get -y install dphys-swapfile build-essential libgmp3-dev golang git python curl
```
<b>go-nekoniumのソースコードのclone</b>
```bash
$cd
$mkdir src
$cd src
$git clone https://github.com/nekonium/go-nekonium.git
```

<b>go-nekoniumのソースコードのビルド</b>
```bash
$cd go-nekonium
$make -j2 gnekonium
```

![compile_success](https://github.com/nekonium/nekonium.github.io/blob/master/raspberry_pi/image/compile_success.jpg)
./build/binにビルドしたバイナリが入っていますので使いやすい場所にコピーしてください。
ここではホームディレクトリ直下に作ったbinディレクトリに置いてみます。
```bash
$cp build/bin/gnekonium ~/bin
```


### gnekoniumの起動
nekoniumのメインネットに接続してブロックチェーンをダウンロードします。
```bash
$cd
$gnekonium console
```

#### プライベートネットの起動方法
以下を参照してください。
https://github.com/nekonium/nekonium.github.io/blob/master/documents/browser-solidity/solidity.JP.md#%E3%83%86%E3%82%B9%E3%83%88%E7%92%B0%E5%A2%83%E3%81%A7%E8%A9%A6%E3%81%97%E3%81%A6%E3%81%BF%E3%82%8B






