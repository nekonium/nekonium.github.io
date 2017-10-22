# Nekoniumのコントラクトの作り方

NekoniumはEthereum1.6.6からできているので、Ethereumのコントラクトをそのまま利用できます。
開発ツールや開発手法も若干の修正で利用することができます。


## コントラクトの開発環境

コントラクトを作成するにはブラウザで動作するbrowser-solidityが便利です。
Ethereum向けのものも利用できますが、Nekonium向けに調整したものがあります。

<img width="50%" src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/1.png"/>


### browser-solidityを起動する

以下のURLを開くとbrowser-solidityが起動します。

<a href="http://nekonium.org/browser-solidity/01.85884478a57de99508250b0e1b625e9afd1e7eaf/">http://nekonium.org/browser-solidity/01.85884478a57de99508250b0e1b625e9afd1e7eaf/</a>

右側のRunパネルの、Environmentが"JavaScript VM"であることを確認してください。browser-solidityはJavascriptでコントラクトのエミュレートができます。
コードを書いて挙動をチェックするところまでは、ネットワークに接続せずにエミュレータで開発ができます。


<img width="50%" src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/2.png"/>



Accountには利用可能なユーザアカウントアドレスと、その残高が表示されます。JavaScript VMはエミュレータなので使い放題です。

**Gas Price**はGasとNukoのレートです。1 Nukoが何Gasに相当するかを表します。

**Gas Limit**は、コントラクトがバグなどにより延々と動作し続けてGas(Nuko)を（無駄に）消費し続けることがないように設けられた、動作上限を設定するための項目です。

**Value**は、個々のトランザクションで、コントラクトに対して送金する金額を指定します。(今回は0で問題ありません)


### とりあえず何かを実装してみる

起動時点ではエディタにballot.solというサンプルがあります。よくわからないので×ボタンで閉じます。

<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/3.png"/>


代わりに<a href="https://book.ethereum-jp.net/first_use/contract.html">Ethereum入門</a>にある簡単なコントラクトを使いましょう。

左上の＋ボタンで新しいファイルを追加して、次のコードを入力します。

<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/4.png"/>

このコードはstoredDataに整数値を格納したり読み出したりできるコードです。

```
contract SingleNumRegister {
    uint storedData;
    function set(uint x) {
        storedData = x;
    }
    function get() constant returns (uint retVal) {
    return storedData;
    }
}
```

### ファイルの保存

編集中のファイルはブラウザ上のCookieに保存されます。

**間違って消してしまわないように、コピペするなどして安全な場所に保存してください。**

詳しくは、Browser-Soldityのマニュアルを確認して下さい。



### VMで実行してみる

ソースコードができたらコンパイルです。Createボタンを押します。
ちょっとだけ警告ができますが大体よさそうです。

<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/5.png"/>


よく見るとget関数とset関数がありますね。set関数に値を入力してボタンをクリック後、getボタンを押すとget関数の返す値が変わるのが確認できました。

<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/6.png"/>

また、画面中央下にはコンソールがあります。Detailsをクリックすると使用したGasなどがわかります。

<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/7.png"/>

## テスト環境で試してみる

コントラクトができたら、Nekoniumのプライベートネットを構築してテストしましょう。


### プライベートネットの準備

Windowsでの作業例です。ほかのOSではそれなりに読み替えてください。
作業用のフォルダを作り、gnekoniumをコピーして、テキストファイルを２つ作成します。

<b>genesis_private.json</b>
```
{
  "config": {
        "chainId": 2,
        "homesteadBlock": 0,
        "eip150Block": 0,
        "eip155Block": 100,
        "eip158Block": 100
  },
  "coinbase"   : "0x0000000000000000000000000000000000000000",
  "difficulty" : "0x200",
  "extraData"  : "",
  "gasLimit"   : "0x2fefd8",
  "nonce"      : "0x0000000000000042",
  "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp"  : "0x00",
  "alloc":{
  }
}
```

<b>start_private.bat</b>
```
gnekonium --networkid "10" --rpc --rpcaddr "localhost"  --rpccorsdomain "*" --nodiscover --datadir "./privatenet" console
```

ここまでで、作業ディレクトリには３個のファイルがあるはずです。(privatenetディレクトリは作らなくてもかまいません。)

<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/8.png"/>


### ジェネシスブロックの作成

プライベートネットのジェネシスブロックを作成します。
ファイルのあるディレクトリで、コマンドラインから次のように実行します。

```
$gnekonium --datadir ./privatenet init ./genesis_private.json
```

ログが流れてプライベートネットのデータがprivatenetフォルダに生成されます。

<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/11.png"/>


### プライベートネットの起動

ファイルのあるディレクトリで、コマンドラインからバッチファイルでプライベートネットを起動します。

<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/9.png"/>


gnekoniumが起動したら、テスト用のユーザーアカウントの作成して、アカウントのアンロックしておきます。

```
> personal.newAccount("passwd")
I"N0FxO3 d5f5[00479-02d7c|21c38:c2b7f:85930]b 9Nfe8w2 ew7a0lbl5ect0 6acpep6ecaerfe2d"
  >                  url=keystore://D:\\新しいフォ??… status=Locked
> personal.unlockAccount(eth.accounts[0])
Unlock account 0x3d5f50490dc2c8cbf890b9f82e70b5c06ce6cef2
Passphrase: ("passwd"と入力)
true
>
```

ここでは、0x3d5f50490dc2c8cbf890b9f82e70b5c06ce6cef2というアカウントを新規で作成し、アンロックしています。


### gnekoniumに接続する

browser-solidityのEnvironmentから、Web3 Providerを選択してください。

問い合わせダイアログでOKを押すと接続先のノードを聞かれるので、そこに起動したgnekoniumのアドレスを入力します。

接続に成功すると、Accountの残高とアドレスが更新されます。

<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/10.png"/>


接続に失敗すると、Accountの残高が更新されずに右側のパネルの下にエラーが表示されます。
よくある原因は以下の通りです。頑張って問題を解消してください。
特にアンロックは300秒で自動的に解除されるので、その場合は再度gnekoniumからアンロックを実施して下さい。

<a href="http://qiita.com/hshimo/items/4ec0889f06d1ce263137">Qiita:Ethereum: アカウントをアンロックする方法</a>

- gnekoniumが起動していない。またはgnekoniumのポートが間違ってる
- プライベートネットにアカウントがない。またはアカウントのアンロックを忘れている.または時間経過でアンロックが無効になっている。
- browser-solidityをhttps://で開いている


### コントラクタをプライベートネットワークに送信する


送信前に、gnekoniumのマイニングを開始しておきます。マイニングを実行しておかないとコントラクトのトランザクションがブロックチェーンに取り込まれません。
gnekoniumのコンソールから、miner.start(1)を実行します。テストですから本気でマイニングをする必要はありません。スレッド数は１で十分です。

```
>miner.start(1)
null
```

browser-solidityのAccountの残高が溜まるまで少し待ちます。

Createボタンを押すと、プライベートネットへトランザクションが送信されてるので、マイニングされるまでしばらく待ちます。
<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/12.png"/>

マイニングされればブロックチェーンへのコントラクトの登録は完了です。

コントラクタのアドレスは、Copy addressで得ることができます。
<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/13.png"/>



## メインネットに送信する

テスト環境で十分な検証ができたら、接続先をメインネットに変えてトランザクションを送信します。<b>ここからの作業はメインネットのNekoniumに影響を及ぼします。慎重に実施してください。</b>

手順はプライベートネットとほぼ同じです。


### メインネットをRPC付きで起動

gnekoniumをオプションをつけて次のように起動します。
```
gnekonium --rpc --rpcaddr "localhost"  --rpccorsdomain "*" console
```


### メインネットにコントラクトを送信する

Environmentを一度JavascriptVMに戻して、もう一度Web3Providerを選択します。

接続が完了したら、Accountsから登録を行うユーザアカウントを選択してください。
<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/browser-solidity/img/14.png"/>

選択したアカウントは、テストと同じ要領でアンロックしておきます。

```
> personal.unlockAccount(eth.accounts[?アカウント番号?]) 
Unlock account 0x............
Passphrase: 
true
>
```

Createボタンを押すと、メインネットにトランザクションが送信されて登録が行われます。
メインネットで新しいブロックが生成されるまでしばらくお待ちください。

### コントラクトアドレスの取得
登録が完了すると、Copy addressからコントラクタのアドレスがコピーできるようになります。<b>Copy addressで忘れずに控えておいてください。</b>

メインネットの場合はハッシュレートが外部から供給されているので自分でマイニングを行う必要はありません。
また、<b>一度登録したコントラクトはNekoniumが消滅するまで消えることはありません。十分に注意してください。</b>



