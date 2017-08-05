# Nekonium Getting started

Ethereumウォレットを同一PCに保持している場合は、未知の問題が発生してEthereumに影響を及ぼす可能性があります。Ethereumウォレットやgethをセットアップしている場合は必ずバックアップを取ってください。）


## マイニング方法

go-nekoniumを入手してください。Windows 64bitについてはバイナリがあります。　
<a href="https://github.com/nekonium/go-nekonium">https://github.com/nekonium/go-nekonium</a>　
その他のOSはソースからコンパイルしてください。


1. コマンドラインからgnekoniumを起動します。
````
$gnekonium console
````
2. ネットワークに接続するまでの間しばらくおまちください。net変数で接続ピア数がわかります。
````
> net
{
  listening: true,
  peerCount: 2,
  version: "1",
  getListening: function(callback),
  getPeerCount: function(callback),
  getVersion: function(callback)
}
````

3. アカウントを作成します。アカウントを作成するためにはパスワードが必要です。
パスワードを忘れるとアカウントにアクセスできなくなります。大切に保管してください。
````
> personal.newAccount("<password>")
````

4. 作成したアカウントの一覧はaccounts変数で見ることができます。
````
> eth.accounts
````
5. miner.start関数で採掘ができます。引数はスレッド数です。
````
>miner.start(<スレッド数>)
````
6. 停止はstop関数を使います。
````
>miner.stop()
````
7. 現在のバランスを見るにはgetBalance関数を使います。単位を整形するためにfromWei関数を使います。
````
> web3.fromWei(eth.getBalance(eth.accounts[0]),"nuko")
100.968372
````
8. 終了してコマンド画面に戻るにはexitと書きます。
````
>exit
````
その他コマンドは参考リンクを調べてください。

<b>OpenBetaが終了する2万Blockを採掘するまでの間は、CPUマイニング以外を行わないでください。</b>

## 参考リンク

リンク先はEthereumの説明ですが、Nekoniumでも同じように操作ができます。gethをgnekoniumに読み替えてください。

* <a href="https://book.ethereum-jp.net/what_is_ethereum/">https://book.ethereum-jp.net/what_is_ethereum/</a>
* <a href=" https://book.ethereum-jp.net/first_use/mining_ether.html">https://book.ethereum-jp.net/first_use/mining_ether.html</a>


# ウォレットの利用

ウォレットアプリケーションMistWalletを利用できます。Windowsはバイナリがありますが、その他のOSはソースからコンパイルしてください。コンパイル手順はEthereumのMistと同じです。
オープンベータ期間中はオートアップデートが無効です。先にgnekoniumを起動しておいてください。


<a href="https://github.com/nekonium/mist/releases">https://github.com/nekonium/mist/releases</a>

* MistWallet起動の前に、まずgnekoniumを起動しておいてください。
* Zipファイルの中にあるNekonium Wallet.exeを起動します。
* Mistの送受信機能を試すことができます。使い方はEthereum版と同じです。

## 不具合
コントラクト、ウォレットコントラクトは不具合のため機能しません。また、日本語での単位表記に問題があり、小数点が,になっています。


## 参考リンク

リンク先はEthereumの説明ですが、Nekoniumでも同じように操作ができます。
* <a href="http://www.intellilink.co.jp/article/column/ethereum01.html">http://www.intellilink.co.jp/article/column/ethereum01.html</a>



