# Nekonium Open Beta試掘手順書

## 参加方法

以下の要件に該当する方が参加できます。

* 仮想通貨マイニングに関する基本的な知識があり、自力でググるなどしてマイニングまでたどり着ける技術があること。
* オープンベータ期間中のハッシュレート制限(CPUマイニング限定)や、問題発生時の採掘停止指示に従えること。
* ロールバックを含むハードフォークを許容すること。
* Ethereumウォレットを同一PCに保持していないこと（NekoniumはEthereumのフォークのため、Ethereumウォレットに何らかの影響を及ぼす可能性を否定できません。Ethereumウォレットやgethをセットアップしている場合は必ずバックアップを取ってください。）

細かいサポートはできませんので、自力でマイニングまでたどり着けない方は参加をご遠慮ください。

## マイニング方法

go-nekoniumを入手してください。Windows 64bitについてはバイナリがあります。
<a href="https://github.com/go-nekonium/go-nekonium">https://github.com/go-nekonium/go-nekonium</a>
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

<b>OpenBetaが終了するまでの間、もしくは5万Blockを採掘するまでの間は、CPUマイニング以外を行わないでください。</b>

## 参考リンク

リンク先はEthereumの説明ですが、Nekoniumでも同じように操作ができます。gethをgnekoniumに読み替えてください。

* <a href="https://book.ethereum-jp.net/what_is_ethereum/">https://book.ethereum-jp.net/what_is_ethereum/</a>
* <a href=" https://book.ethereum-jp.net/first_use/mining_ether.html">https://book.ethereum-jp.net/first_use/mining_ether.html</a>


# ウォレットの利用

ウォレットアプリケーションMistWalletを利用できます。Windowsはバイナリがありますが、その他のOSはソースからコンパイルしてください。コンパイル手順はEthereumのMistと同じです。
オープンベータ期間中はオートアップデートが無効です。先にgnekoniumを起動しておいてください。

<a href="https://github.com/nekonium/mist/releases">https://github.com/nekonium/mist/releases</a>


* MistWallet起動の前に、まずgnekoniumを起動しておいてください。
* Mistを起動します。Windowsの場合はスタートメニューから起動できると思います。
* Mistの送受信機能を試すことができます。使い方はEthereum版と同じです。



参考リンク
リンク先はEthereumの説明ですが、Nekoniumでも同じように操作ができます。
* <a href="http://www.intellilink.co.jp/article/column/ethereum01.html">http://www.intellilink.co.jp/article/column/ethereum01.html</a>



大きな問題がない限り、OpenBeta終了後もブロックチェーンのデータは保持されます。　得られたNekoniumはそのまま所有していただいて構いません。



