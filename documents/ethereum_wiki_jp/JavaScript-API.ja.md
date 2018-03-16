From https://github.com/ethereum/wiki/wiki/JavaScript-API

# Web3 JavaScript app API for 0.2x.x

**注意: このドキュメントは、web3.js version 0.2x.x向けです。もしweb3.js 1.0 を使うなら、こちらのドキュメントを見てください。(http://web3js.readthedocs.io/en/1.0/index.html).**

Ethereumで動作するアプリケーションを作るには、[web3.js library]の`web3`オブジェクトプロバイダを使うことができます。
(https://github.com/ethereum/web3.js). ローカルノードとの通信を隠蔽します。 [RPC calls](https://github.com/ethereum/wiki/wiki/JSON-RPC). web3.jsはEthereumノードとともに動作し、RPCレイヤーを公開します。

`web3` は`eth`オブジェクトを持ちます。 - `web3.eth` オブジェクト(Ethereumブロックチェイン操作に使用) 、 `shh` オブジェクト - `web3.shh` (Whisper操作に使用). 今後、web3の他のオブジェクトについても紹介していきます。
使用例が、[examples can be found here](https://github.com/ethereum/web3.js/tree/master/example)にあります。


Web3を使ったより実用的なサンプルはこちらです。 [useful app patterns](https://github.com/ethereum/wiki/wiki/Useful-Dapp-Patterns).

## Getting Started

* [Adding web3](#adding-web3)
* [Using Callbacks](#using-callbacks)
* [Batch requests](#batch-requests)
* [A note on big numbers in web3.js](#a-note-on-big-numbers-in-web3js)
* [-> API Reference](#web3js-api-reference)

### Adding web3

web3.jsを自分のプロジェクトにセットアップする方法です。以下の方法を使います。:

- npm: `npm install web3`
- bower: `bower install web3`
- meteor: `meteor add ethereum:web3`
- vanilla: link the `dist./web3.min.js`

つぎに、web3インスタンスを作成してproviderの設定をします。
Mistでは既にセットアップされているproviderを上書きしないようにしてください。web3をアクティブにする前に確認します。

```js
if (typeof web3 !== 'undefined') {
  web3 = new Web3(web3.currentProvider);
} else {
  // set the provider you want from Web3.providers
  web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
}
```

以上で、web3オブジェクトの[API](web3js-api-reference)を使う準備は完了です。

### Using callbacks

As this API is designed to work with a local RPC node, all its functions use synchronous HTTP requests by default.

If you want to make an asynchronous request, you can pass an optional callback as the last parameter to most functions.
All callbacks are using an [error first callback](http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/) style:

```js
web3.eth.getBlock(48, function(error, result){
    if(!error)
        console.log(JSON.stringify(result));
    else
        console.error(error);
})
```

### Batch requests

Batch requests allow queuing up requests and processing them at once.

**Note** Batch requests are not faster! In fact making many requests at once will in some cases be faster, as requests are processed asynchronously. Batch requests are mainly useful to ensure the serial processing of requests.

```js
var batch = web3.createBatch();
batch.add(web3.eth.getBalance.request('0x0000000000000000000000000000000000000000', 'latest', callback));
batch.add(web3.eth.Contract(abi).at(address).balance.request(address, callback2));
batch.execute();
```

### A note on big numbers in web3.js

You will always get a BigNumber object for number values as JavaScript is not able to handle big numbers correctly.
Look at the following examples:

```js
"101010100324325345346456456456456456456"
// "101010100324325345346456456456456456456"
101010100324325345346456456456456456456
// 1.0101010032432535e+38
```

web3.js depends on the [BigNumber Library](https://github.com/MikeMcl/bignumber.js/) and adds it automatically.

```js
var balance = new BigNumber('131242344353464564564574574567456');
// or var balance = web3.eth.getBalance(someAddress);

balance.plus(21).toString(10); // toString(10) converts it to a number string
// "131242344353464564564574574567477"
```

The next example wouldn't work as we have more than 20 floating points, therefore it is recommended to always keep your balance in *wei* and only transform it to other units when presenting to the user:
```js
var balance = new BigNumber('13124.234435346456466666457455567456');

balance.plus(21).toString(10); // toString(10) converts it to a number string, but can only show max 20 floating points 
// "13145.23443534645646666646" // you number would be cut after the 20 floating point
```

## Web3.js API Reference

* [web3](#web3)
  * [version](#web3versionapi)
     * [api](#web3versionapi)
     * [node/getNode](#web3versionnode)
     * [network/getNetwork](#web3versionnetwork)
     * [ethereum/getEthereum](#web3versionethereum)
     * [whisper/getWhisper](#web3versionwhisper)
  * [isConnected()](#web3isconnected)
  * [setProvider(provider)](#web3setprovider)
  * [currentProvider](#web3currentprovider)
  * [reset()](#web3reset)
  * [sha3(string, options)](#web3sha3)
  * [toHex(stringOrNumber)](#web3tohex)
  * [toAscii(hexString)](#web3toascii)
  * [fromAscii(textString, [padding])](#web3fromascii)
  * [toDecimal(hexString)](#web3todecimal)
  * [fromDecimal(number)](#web3fromdecimal)
  * [fromWei(numberStringOrBigNumber, unit)](#web3fromwei)
  * [toWei(numberStringOrBigNumber, unit)](#web3towei)
  * [toBigNumber(numberOrHexString)](#web3tobignumber)
  * [isAddress(hexString)](#web3isaddress)
  * [net](#web3net)
    * [listening/getListening](#web3netlistening)
    * [peerCount/getPeerCount](#web3ethpeercount)
  * [eth](#web3eth)
    * [defaultAccount](#web3ethdefaultaccount)
    * [defaultBlock](#web3ethdefaultblock)
    * [syncing/getSyncing](#web3ethsyncing)
    * [isSyncing](#web3ethissyncing)
    * [coinbase/getCoinbase](#web3ethcoinbase)
    * [hashrate/getHashrate](#web3ethhashrate)
    * [gasPrice/getGasPrice](#web3ethgasprice)
    * [accounts/getAccounts](#web3ethaccounts)
    * [mining/getMining](#web3ethmining)
    * [blockNumber/getBlockNumber](#web3ethblocknumber)
    * [register(hexString)](#web3ethregister) (Not implemented yet)
    * [unRegister(hexString)](#web3ethunregister) (Not implemented yet)
    * [getBalance(address)](#web3ethgetbalance)
    * [getStorageAt(address, position)](#web3ethgetstorageat)
    * [getCode(address)](#web3ethgetcode)
    * [getBlock(hash/number)](#web3ethgetblock)
    * [getBlockTransactionCount(hash/number)](#web3ethgetblocktransactioncount)
    * [getUncle(hash/number)](#web3ethgetuncle)
    * [getBlockUncleCount(hash/number)](#web3ethgetblockunclecount)
    * [getTransaction(hash)](#web3ethgettransaction)
    * [getTransactionFromBlock(hashOrNumber, indexNumber)](#web3ethgettransactionfromblock)
    * [getTransactionReceipt(hash)](#web3ethgettransactionreceipt)
    * [getTransactionCount(address)](#web3ethgettransactioncount)
    * [sendTransaction(object)](#web3ethsendtransaction)
    * [sendRawTransaction(object)](#web3ethsendrawtransaction)
    * [sign(object)](#web3ethsign)
    * [call(object)](#web3ethcall)
    * [estimateGas(object)](#web3ethestimategas)
    * [filter(array (, options) )](#web3ethfilter)
        - [watch(callback)](#web3ethfilter)
        - [stopWatching(callback)](#web3ethfilter)
        - [get()](#web3ethfilter)
    * [Contract(abiArray)](#web3ethcontract)
    * [contract.myMethod()](#contract-methods)
    * [contract.myEvent()](#contract-events)
    * [contract.allEvents()](#contract-allevents)
    * [getCompilers()](#web3ethgetcompilers)
    * [compile.lll(string)](#web3ethcompilelll)
    * [compile.solidity(string)](#web3ethcompilesolidity)
    * [compile.serpent(string)](#web3ethcompileserpent)
    * [namereg](#web3ethnamereg)
    * [sendIBANTransaction](#web3ethsendibantransaction)
    * [iban](#web3ethiban)
      * [fromAddress](#web3ethibanfromaddress)
      * [fromBban](#web3ethibanfrombban)
      * [createIndirect](#web3ethibancreateindirect)
      * [isValid](#web3ethibanisvalid)
      * [isDirect](#web3ethibanisdirect)
      * [isIndirect](#web3ethibanisindirect)
      * [checksum](#web3ethibanchecksum)
      * [institution](#web3ethibaninstitution)
      * [client](#web3ethibanclient)
      * [address](#web3ethibanaddress)
      * [toString](#web3ethibantostring)
  * [db](#web3db)
    * [putString(name, key, value)](#web3dbputstring)
    * [getString(-       var rXArray = stringToNumberArray(form.rX.value);
name, key)](#web3dbgetstring)
    * [putHex(name, key, value)](#web3dbputhex)
    * [getHex(name, key)](#web3dbgethex)
  * [shh](#web3shh)
    * [post(postObject)](#web3shhpost)
    * [newIdentity()](#web3shhnewidentity)
    * [hasIdentity(hexString)](#web3shhhaveidentity)
    * [newGroup(_id, _who)](#web3shhnewgroup)
    * [addToGroup(_id, _who)](#web3shhaddtogroup)
    * [filter(object/string)](#web3shhfilter)
      * [watch(callback)](#web3shhfilter)
      * [stopWatching(callback)](#web3shhfilter)
      * [get(callback)](#web3shhfilter)

### 使い方

#### web3
`web3` オブジェクトプロバイダの全メソッド

##### 使用例

```js
var Web3 = require('web3');
// create an instance of web3 using the HTTP provider.
// NOTE in mist web3 is already available, so check first if it's available before instantiating
var web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
```

###### HTTP Basic Authenticationの使用例
```js
var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545", 0, BasicAuthUsername, BasicAuthPassword));
//Note: HttpProvider takes 4 arguments (host, timeout, user, password)
```

***

#### web3.version.api

```js
web3.version.api
```

##### 戻り値

`String` - EthereumのjsAPIのバージョン

##### 使用例

```js
var version = web3.version.api;
console.log(version); // "0.2.0"
```

***

#### web3.version.node

    web3.version.node
    // 非同期呼び出し
    web3.version.getNode(callback(error, result){ ... })


##### 戻り値

`String` - クライアント/ノードのバージョン

##### 使用例

```js
var version = web3.version.node;
console.log(version); // "Mist/v0.9.3/darwin/go1.4.1"
```

***

#### web3.version.network

    web3.version.network
    // or async
    web3.version.getNetwork(callback(error, result){ ... })


##### 戻り値

`String` - ネットワークプロトコルのバージョン

##### 使用例

```js
var version = web3.version.network;
console.log(version); // 54
```

***

#### web3.version.ethereum

    web3.version.ethereum
    // or async
    web3.version.getEthereum(callback(error, result){ ... })


##### 戻り値

`String` - Ethereumプロトコルのバージョン

##### 使用例

```js
var version = web3.version.ethereum;
console.log(version); // 60
```

***

#### web3.version.whisper

    web3.version.whisper
    // or async
    web3.version.getWhisper(callback(error, result){ ... })


##### 戻り値

`String` - whisper protocolのバージョン

##### 使用例

```js
var version = web3.version.whisper;
console.log(version); // 20
```

***
#### web3.isConnected

    web3.isConnected()

他のノードへ接続しているかチェックするために使います。

##### パラメータ
なし

##### 戻り値

`Boolean`

##### 使用例

```js
if(!web3.isConnected()) {
  
   // 例えばユーザーがノードを起動したかを尋ねるダイアログを表示する。

} else {
 
   // web3を使った様々な処理を行う。
  
}
```

***

#### web3.setProvider

    web3.setProvider(provider)

プロバイダをセットします。

##### パラメータ
なし

##### 戻り値

`undefined`

##### 使用例

```js
web3.setProvider(new web3.providers.HttpProvider('http://localhost:8545')); // 8080 for cpp/AZ, 8545 for go/mist
```

***

#### web3.currentProvider

    web3.currentProvider

現在のプロバイダがあればセットされます。これはMistなどで、すでにプロバイダがセットされているかを調べることができます。
です。

##### 戻り値

`Object` - The provider set or `null`;

##### 使用例

```js
// Check if mist etc. already set a provider
if(!web3.currentProvider)
    web3.setProvider(new web3.providers.HttpProvider("http://localhost:8545"));

```

***

#### web3.reset

    web3.reset(keepIsSyncing)

Web3の状態をリセットするときに使います。マネージャーを除くものをリセットします。ポーリング、フィルタは削除されます。

##### パラメータ

1. `Boolean` - `true`の場合はフィルタが削除されます。しかし、 [web3.eth.isSyncing()](#web3ethissyncing) にプールされている分は維持されます。

##### 戻り値

`undefined`

##### 使用例

```js
web3.reset();
```

***

#### web3.sha3

    web3.sha3(string [, options])

##### パラメータ

1. `String` - Keccak-256 SHA3アルゴリズムでハッシュ化する文字列です。
1. `Object` - (optional) 16進数文字列としてハッシュを得る場合は、`encoding` に `hex`を指定します。 先頭の `0x` は自動で無視します。

##### 戻り値

`String` - Keccak-256 SHA3ハッシュ値

##### 使用例

```js
var hash = web3.sha3("Some string to be hashed");
console.log(hash); // "0xed973b234cf2238052c9ac87072c71bcf33abc1bbd721018e0cca448ef79b379"

var hashOfHash = web3.sha3(hash, {encoding: 'hex'});
console.log(hashOfHash); // "0x85dd39c91a64167ba20732b228251e67caed1462d4bcf036af88dc6856d0fdcc"
```

***

#### web3.toHex

    web3.toHex(mixed);
 
あらゆる値をHEX文字列に変換します。

##### パラメータ

1. `String|Number|Object|Array|BigNumber` - HEX文字列に変換する値。これがオブジェクトや配列だった場合、先に`JSON.stringify`で文字列化します。BigNumberであるなら、数値として扱います。

##### 戻り値

`String` - `mixed`のHEX文字列。

##### 使用例

```js
var str = web3.toHex({test: 'test'});
console.log(str); // '0x7b2274657374223a2274657374227d'
```

***

#### web3.toAscii

    web3.toAscii(hexString);

HEX文字列をASCII文字列に戻します。

##### パラメータ

1. `String` - ASCII文字列に変換するHEX文字列。

##### 戻り値

`String` - `hexString`から復元したASCII文字列。

##### 使用例

```js
var str = web3.toAscii("0x657468657265756d000000000000000000000000000000000000000000000000");
console.log(str); // "ethereum"
```

***

#### web3.fromAscii

    web3.fromAscii(string [, padding]);

任意のASCII文字列をHEX文字列にします。

##### パラメータ

1. `String` - HEX文字列に変換するASCII文字列
2. `Number` - (optional) 出力するHEX文字列のバイト数。

##### 戻り値

`String` - 変換したHEX文字列。

##### 使用例

```js
var str = web3.fromAscii('ethereum');
console.log(str); // "0x657468657265756d"

var str2 = web3.fromAscii('ethereum', 32);
console.log(str2); // "0x657468657265756d000000000000000000000000000000000000000000000000"
```

***

#### web3.toDecimal

    web3.toDecimal(hexString);

HEX文字列を十進数表記の数値に変換します。

##### パラメータ

1. `String` - 変換した十進数の値


##### 戻り値

`Number` - `hexString`の数値表現

##### 使用例

```js
var number = web3.toDecimal('0x15');
console.log(number); // 21
```

***

#### web3.fromDecimal

    web3.fromDecimal(number);

数値をHEX文字列に変換します。

##### パラメータ

1. `Number|String` - 10新数文字列を変換したHEX文字列

##### 戻り値

`String` - `number`のHEX文字列。

##### 使用例

```js
var value = web3.fromDecimal('21');
console.log(value); // "0x15"
```

***

#### web3.fromWei

    web3.fromWei(number, unit)

数値(wei単位)を以下のethereum単位の数値に変換します。

- `Gwei`
- `Kwei`
- `Mwei`/`babbage`/`ether`/`femtoether`
- `ether`
- `finney`/`gether`/grand/`gwei`
- `kether`/`kwei`/`lovelace`/`mether`/`micro`
- `microether`/`milli`/`milliether`
- `mwei`/`nano`/`nanoether`
- `noether`
- `picoether`/`shannon`
- `szabo`
- `tether`
- `wei`

※Nekoniumの単位nukoはetherと同じ単位です。補助単位はありません。

##### パラメータ

1. `Number|String|BigNumber` - 数値、またはBigNumberの値
2. `String` - 上記リストにあるEthereum単位。


##### 戻り値

`String|BigNumber` - 指定したEthereum単位の数値文字列、またはBigNumberの値。値の種類は`number`引数の種類で変わります。

##### 使用例

```js
var value = web3.fromWei('21000000000000', 'finney');
console.log(value); // "0.021"
```

***

#### web3.toWei

    web3.toWei(number, unit)

Ethereum単位の数値を、wei単位の数値に変換します。

- `kwei`/`ada`
- `mwei`/`babbage`
- `gwei`/`shannon`
- `szabo`
- `finney`
- `ether`
- `kether`/`grand`/`einstein`
- `mether`
- `gether`
- `tether`

※Nekoniumの単位nukoはetherと同じ単位です。補助単位はありません。

##### パラメータ

1. `Number|String|BigNumber` - 数値、またはBigNumberの値
2. `String` - 上記リストにあるEthereum単位。

##### 戻り値

`String|BigNumber` - wei単位の数値文字列、またはBigNumberの値。値の種類は`number`引数の種類で変わります。

##### 使用例

```js
var value = web3.toWei('1', 'ether');
console.log(value); // "1000000000000000000"
```

***

#### web3.toBigNumber

    web3.toBigNumber(numberOrHexString);

数値をBigNumberに変換します。

参照 [note on BigNumber](#a-note-on-big-numbers-in-web3js).

##### パラメータ

1. `Number|String` - 数値、または数値文字列、数値から作成したHEX文字列

##### 戻り値

`BigNumber` - 変換した数値を格納したBigNumber

##### 使用例

```js
var value = web3.toBigNumber('200000000000000000000001');
console.log(value); // instanceOf BigNumber
console.log(value.toNumber()); // 2.0000000000000002e+23
console.log(value.toString(10)); // '200000000000000000000001'
```

***

#### web3.isAddress

    web3.isAddress(HexString);

与えた文字列がEthereumアドレスのフォーマットであるか確認します。

##### パラメータ

1. `String` - HEX文字列

##### 戻り値

`Boolean` - `false` Ethereumアドレスのフォーマットではない。 `true` 全てが小文字、または大文字の正しいフォーマットのアドレス。大文字、小文字が混在したアドレスは、`web3.isChecksumAddress()`関数でチェックしてください。

##### 使用例

```js
var isAddress = web3.isAddress("0x8888f1f195afa192cfee860698584c030f4c9db1");
console.log(isAddress); // true
```

***

### web3.net

#### web3.net.listening

```js
web3.net.listening
// 非同期.
web3.net.getListening(callback(error, result){ ... })
```

このプロパティは読み出し専用です。ノードがネットワーク接続をアクティブに受信待ちしているかどうかを示します。

##### 戻り値

`Boolean` - `true` クライアントがネットワーク接続を受信待ちしている。`false` それ以外。

##### 使用例

```js
var listening = web3.net.listening;
console.log(listening); // true of false
```

***

#### web3.net.peerCount
```js
web3.net.peerCount
// or async
web3.net.getPeerCount(callback(error, result){ ... })
```

読み出し専用プロパティです。接続してルピアノードの数を返します。

##### 戻り値

`Number` - 現在接続しているピアの数です。

##### 使用例

```js
var peerCount = web3.net.peerCount;
console.log(peerCount); // 4
```

***

### web3.eth

Ethereumブロックチェーンに関するメソッドを含みます。

##### 使用例

```js
var eth = web3.eth;
```

***

#### web3.eth.defaultAccount

```js
web3.eth.defaultAccount
```

このデフォルトアドレスが、次のメソッドで使われます。（メソッドのオプション引数`from`でプロパティで上書きすることができます。）

- [web3.eth.sendTransaction()](#web3ethsendtransaction)
- [web3.eth.call()](#web3ethcall)

##### Values

`String`, 20 Bytes - 所有する秘密鍵を持つアドレスのひとつです。

*Default is* `undefined`.

##### 戻り値

`String`, 20 Bytes - 現在のデフォルトアドレスです。

##### 使用例

```js
var defaultAccount = web3.eth.defaultAccount;
console.log(defaultAccount); // ''

// set the default account
web3.eth.defaultAccount = '0x8888f1f195afa192cfee860698584c030f4c9db1';
```

***

#### web3.eth.defaultBlock

```js
web3.eth.defaultBlock
```

デフォルトブロックは以下のメソッドで使います。(メソッドのオプション引数`defaultBlock`でプロパティで上書きすることができます。）

- [web3.eth.getBalance()](#web3ethgetbalance)
- [web3.eth.getCode()](#web3ethgetcode)
- [web3.eth.getTransactionCount()](#web3ethgettransactioncount)
- [web3.eth.getStorageAt()](#web3ethgetstorageat)
- [web3.eth.call()](#web3ethcall)
- [contract.myMethod.call()](#contract-methods)
- [contract.myMethod.estimateGas()](#contract-methods)

##### Values

デフォルトブロック引数には、以下のいずれかが使用できます:

- `Number` - ブロック番号の数値
- `String` - `"earliest"`, ジェネシスブロック
- `String` - `"latest"`, 最終ブロック(ブロックチェーンの現在の先頭)
- `String` - `"pending"`, 採掘中のブロック(保留中のトランザクションを含む)

*Default is* `latest`

##### 戻り値

`Number|String` - 状態を照会するときに使うブロック番号。

##### 使用例

```js
var defaultBlock = web3.eth.defaultBlock;
console.log(defaultBlock); // 'latest'

// デフォルトブロックの番号を設定する。
web3.eth.defaultBlock = 231;
```

***

#### web3.eth.syncing

```js
web3.eth.syncing
//async
web3.eth.getSyncing(callback(error, result){ ... })
```

読み取り専用のプロパティです。ノードが同期していれば、syncオブジェクトを返します。それ以外は`false`を返します。

##### 戻り値

`Object|Boolean` - ノードが現在同期していれば以下の要素を持つオブジェクトを返します。それ以外は`false`です。:

- `startingBlock`: `Number` - 同期を開始したブロック番号。
- `currentBlock`: `Number` - すでに同期が完了しているブロック番号。
- `highestBlock`: `Number` - 最終的に同期できるブロック番号の予測値。

##### 使用例

```js
var sync = web3.eth.syncing;
console.log(sync);
/*
{
   startingBlock: 300,
   currentBlock: 312,
   highestBlock: 512
}
*/
```

***

#### web3.eth.isSyncing

```js
web3.eth.isSyncing(callback);
```

この関数は同期の開始、更新、停止を`callback`に通知する関数です。

##### 戻り値

`Object` - 以下の関数を持つisSincingオブジェクト:

* `syncing.addCallback()`: 他のコールバックを追加します。ノードが開始、または停止した時も呼び出されます。
* `syncing.stopWatching()`: このコールバックを停止します。

##### Callback の引数
※二番目の引数に、２種類のオブジェクとが渡されます。

- `Boolean` - コールバックが開始したときは`true`。停止するときは`false`です。
- `Object` - 同期中はsyncingオブジェクトです。:
  - `startingBlock`: `Number` - 同期を開始したブロック番号。
  - `currentBlock`: `Number` - すでに同期が完了しているブロック番号。
  - `highestBlock`: `Number` - 最終的に同期できるブロック番号の予測値。


##### 使用例

```js
web3.eth.isSyncing(function(error, sync){
    if(!error) {
        // stop all app activity
        if(sync === true) {
           // we use `true`, so it stops all filters, but not the web3.eth.syncing polling
           web3.reset(true);
        
        // show sync info
        } else if(sync) {
           console.log(sync.currentBlock);
        
        // re-gain app operation
        } else {
            // run your app init function...
        }
    }
});
```

***

#### web3.eth.coinbase

```js
web3.eth.coinbase
// or async
web3.eth.getCoinbase(callback(error, result){ ... })
```

読み出し専用のプロパティです。マイニング報酬を受け取るcoinbaseのアドレスを返します。

##### 戻り値

`String` - 現在のクライアントのcoinbaseアドレスです。

##### 使用例

```js
var coinbase = web3.eth.coinbase;
console.log(coinbase); // "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
```

***

#### web3.eth.mining

```js
web3.eth.mining
// or async
web3.eth.getMining(callback(error, result){ ... })
```

読み出し専用のプロパティです。ノードがマイニング中かどうかを返します。


##### 戻り値

`Boolean` - マイニング中なら`true`です。マイニングが停止していれば`false`です。

##### 使用例

```js
var mining = web3.eth.mining;
console.log(mining); // true or false
```

***

#### web3.eth.hashrate

```js
web3.eth.hashrate
// or async
web3.eth.getHashrate(callback(error, result){ ... })
```

読み取り専用のプロパティです。ノードのハッシュレート(hash/sec)の数値を返します。


##### 戻り値

`Number` - ハッシュレート(hash/sec)の数値

##### 使用例

```js
var hashrate = web3.eth.hashrate;
console.log(hashrate); // 493736
```

***

#### web3.eth.gasPrice

```js
web3.eth.gasPrice
// or async
web3.eth.getGasPrice(callback(error, result){ ... })
```

読み取り専用のプロパティです。現在のGas priceを返します。
Gas priceは、最終ブロックのカス価格の平均値から計算します。

##### 戻り値

`BigNumber` - 現在のgas priceをweiで格納したBigNumberです。

[note on BigNumber](#a-note-on-big-numbers-in-web3js)を参照してください。

##### 使用例

```js
var gasPrice = web3.eth.gasPrice;
console.log(gasPrice.toString(10)); // "10000000000000"
```

***

#### web3.eth.accounts

```js
web3.eth.accounts
// or async
web3.eth.getAccounts(callback(error, result){ ... })
```

読み取り専用のプロパティです。ノードの制御下にあるアカウントのリストを返します。

##### 戻り値

`Array` - ノードの制御下にあるアカウントのリスト。

##### 使用例

```js
var accounts = web3.eth.accounts;
console.log(accounts); // ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"] 
```

***

#### web3.eth.blockNumber

```js
web3.eth.blockNumber
// or async
web3.eth.getBlockNumber(callback(error, result){ ... })
```

読み取り専用のプロパティです。現在のブロック番号を返します。

##### 戻り値

`Number` - 最新のブロック番号です。

##### 使用例

```js
var number = web3.eth.blockNumber;
console.log(number); // 2744
```

***

#### web3.eth.register

```js
web3.eth.register(addressHexString [, callback])
```

(未実装です)
アドレスを`web3.eth.accounts`へ登録します。この機能は、PrivateKeyを所有しないアカウント（例えばコントラクトウォレット）をaccountsに関連付けるために使います。

##### パラメータ

1. `String` - 登録するアドレス。
2. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。


##### 戻り値

?


##### 使用例

```js
web3.eth.register("0x407d73d8a49eeb85d32cf465507dd71d507100ca")
```

***

#### web3.eth.unRegister

```js
web3.eth.unRegister(addressHexString [, callback])
```

(未実装)
指定したアドレスをaccountsリストから削除します。

##### パラメータ

1. `String` - 削除するアドレス。
2. `Function` - (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。


##### 戻り値

?


##### 使用例

```js
web3.eth.unregister("0x407d73d8a49eeb85d32cf465507dd71d507100ca")
```

***

#### web3.eth.getBalance

```js
web3.eth.getBalance(addressHexString [, defaultBlock] [, callback])
```

指定したブロックについて、そのアドレスの残高を得ます。

##### パラメータ

1. `String` - バランスを調べるNekoniumアドレス
2. `Number|String` - (optional) 対象のブロック番号。省略するとデフォルトブロックを使います。 [web3.eth.defaultBlock](#web3ethdefaultblock).
3. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`String` - wei単位で、そのブロックにおけるアドレスのバランスを返します。

 [note on BigNumber](#a-note-on-big-numbers-in-web3js)を参照してください。

##### 使用例

```js
var balance = web3.eth.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(balance); // instanceof BigNumber
console.log(balance.toString(10)); // '1000000000000'
console.log(balance.toNumber()); // 1000000000000
```

***

#### web3.eth.getStorageAt

```js
web3.eth.getStorageAt(addressHexString, position [, defaultBlock] [, callback])
```

指定アドレスのストレージを読み出します。（ストレージは、そのアドレスにあるコントラクトが保存した情報です。[参考](https://ethereum.stackexchange.com/questions/5865/what-does-web3-eth-getstorageat-return)）

##### パラメータ

1. `String` - ストレージを読み出すアドレス文字列。
2. `Number` - 読みだすストレージの位置(position)
3. `Number|String` - (optional) 調査するブロックの番号。省略するとデフォルトブロック番号を使います。[web3.eth.defaultBlock](#web3ethdefaultblock).
4. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`String` - 読みだしたストレージの値。

##### 使用例

```js
var state = web3.eth.getStorageAt("0x407d73d8a49eeb85d32cf465507dd71d507100c1", 0);
console.log(state); // "0x03"
```

***

#### web3.eth.getCode

```js
web3.eth.getCode(addressHexString [, defaultBlock] [, callback])
```

指定アドレスのコード(EVMバイトコード)を読み出します。

##### パラメータ

1. `String` - コードを読み出すアドレス文字列。
2. `Number|String` - (optional) 調査するブロックの番号。省略するとデフォルトブロック番号を使います。[web3.eth.defaultBlock](#web3ethdefaultblock).
3. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。
##### 戻り値

`String` - The data at given address `addressHexString`.

##### 使用例

```js
var code = web3.eth.getCode("0xd5677cf67b5aa051bb40496e68ad359eb97cfbf8");
console.log(code); // "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
```

***

#### web3.eth.getBlock

```js
web3.eth.getBlock(blockHashOrBlockNumber [, returnTransactionObjects] [, callback])
```

ブロック番号、またはブロックハッシュに一致するブロックを返します。

##### パラメータ

1. `String|Number` - ブロック番号、またはブロックのハッシュ。または、次の文字列 `"earliest"`, `"latest"` or `"pending"` これらは[default block parameter](#web3ethdefaultblock)を参照してください。
2. `Boolean` - (optional, 省略時 `false`) `true`ならトランザクションオブジェクトを戻り値に含みます。`false`ならトランザクションのハッシュのみを返します。
3. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。
##### 戻り値

`Object` - ブロックオブジェクト:

- `number`: `Number` - ブロック番号。`null`の場合は保留ブロック。 
- `hash`: `String`, 32 Bytes - ブロックのハッシュ。`null`の場合は保留ブロック。 
- `parentHash`: `String`, 32 Bytes - 親ブロックのハッシュ。 
- `nonce`: `String`, 8 Bytes - PoWで生成したハッシュ。`null`の場合は保留ブロック。 
- `sha3Uncles`: `String`, 32 Bytes - このブロックにあるuncles dataのSHA3ハッシュ 
- `logsBloom`: `String`, 256 Bytes - ブロックのログのためのブルームフィルタ。 `null`の場合は保留ブロック。 
- `transactionsRoot`: `String`, 32 Bytes - ブロックのトランザクション木？(transaction trie)のルート。 
- `stateRoot`: `String`, 32 Bytes - ブロックの最終状態木？(state trie)のルート。 
- `miner`: `String`, 20 Bytes - ブロックの採掘報酬を得たアドレス。 
- `difficulty`: `BigNumber` - このブロックの難易度。 
- `totalDifficulty`: `BigNumber` - このブロックまでの難易度の合計 
- `extraData`: `String` - このブロックの"extra data"フィールドの値 
- `size`: `Number` - ブロックのbytes単位でのサイズ。 
- `gasLimit`: `Number` - このブロックで使用できる最大gas量。 
- `gasUsed`: `Number` - このブロックで使用したgasの合計。 
- `timestamp`: `Number` - ブロックを照合したunix時刻。 
- `transactions`: `Array` - トランザクションオブジェクトの配列。または、トランザクションハッシュの配列。 
- `uncles`: `Array` - uncleブロックのハッシュのリスト。 

##### 使用例

```js
var info = web3.eth.getBlock(3150);
console.log(info);
/*
{
  "number": 3,
  "hash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "parentHash": "0x2302e1c0b972d00932deb5dab9eb2982f570597d9d42504c05d9c2147eaf9c88",
  "nonce": "0xfb6e1a62d119228b",
  "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "transactionsRoot": "0x3a1b03875115b79539e5bd33fb00d8f7b7cd61929d5a3c574f507b8acf415bee",
  "stateRoot": "0xf1133199d44695dfa8fd1bcfe424d82854b5cebef75bddd7e40ea94cda515bcb",
  "miner": "0x8888f1f195afa192cfee860698584c030f4c9db1",
  "difficulty": BigNumber,
  "totalDifficulty": BigNumber,
  "size": 616,
  "extraData": "0x",
  "gasLimit": 3141592,
  "gasUsed": 21662,
  "timestamp": 1429287689,
  "transactions": [
    "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b"
  ],
  "uncles": []
}
*/
```

***

#### web3.eth.getBlockTransactionCount

```js
web3.eth.getBlockTransactionCount(hashStringOrBlockNumber [, callback])
```
ブロックに含まれるトランザクションの数を返します。

##### パラメータ

1. `String|Number` - ブロック番号、またはブロックのハッシュ。または定義済の文字列値 `"earliest"`, `"latest"` or `"pending"`  [default block parameter](#web3ethdefaultblock)を参照してください。
2. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。
##### 戻り値

`Number` - 指定したブロックに含まれるトランザクションの数。

##### 使用例

```js
var number = web3.eth.getBlockTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(number); // 1
```

***

#### web3.eth.getUncle

```js
web3.eth.getUncle(blockHashStringOrNumber, uncleNumber [, returnTransactionObjects] [, callback])
```
ブロックの持つuncleブロックを得ます。どのuncleを得るかはindexで指定します。

##### パラメータ

1. `String|Number` - Tブロック番号、またはブロックのハッシュ。または定義済の文字列値 `"earliest"`, `"latest"` or `"pending"`  [default block parameter](#web3ethdefaultblock)を参照してください。
2. `Number` - uncleのインデクス番号を指定します。
3. `Boolean` - (optional, 省略時 `false`) `true`ならトランザクションオブジェクトを戻り値に含みます。`false`ならトランザクションのハッシュのみを返します。
4. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`Object` - uncleブロックです。[web3.eth.getBlock()](#web3ethgetblock)を参照してください。

**Note**: uncleは個々のトランザクションを含みません。

##### 使用例

```js
var uncle = web3.eth.getUncle(500, 0);
console.log(uncle); // see web3.eth.getBlock

```

***

##### web3.eth.getTransaction

```js
web3.eth.getTransaction(transactionHash [, callback])
```
トランザクションハッシュ値に一致するトランザクションを得ます。

##### パラメータ

1. `String` - トランザクションのハッシュ値
2. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`Object` - `transactionHash`のハッシュを持つトランザクション:

- `hash`: `String`, 32 Bytes - トランザクションのハッシュ値。
- `nonce`: `Number` - 事前に送信者が作成した数値。
- `blockHash`: `String`, 32 Bytes - このトランザクションのあるブロックのハッシュ値。保留中ならば`null`。
- `blockNumber`: `Number` - このトランザクションのあるブロックの番号。保留中ならば`null`
- `transactionIndex`: `Number` - ブロックの中でのトランザクションのインデックス番号。保留中ならば`null`
- `from`: `String`, 20 Bytes - 送信元のアドレス
- `to`: `String`, 20 Bytes - 受信先のアドレス。コントラクトの作成トランザクションの場合は`null`
- `value`: `BigNumber` - 送信したWei単位での量。
- `gasPrice`: `BigNumber` - 送信者が提供したWei単位でのgas price.
- `gas`: `Number` - 送信者が提供したgas。
- `input`: `String` - トランザクションと共に送信したデータ。


##### 使用例

```js
var transaction = web3.eth.getTransaction('0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b');
console.log(transaction);
/*
{
  "hash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
  "nonce": 2,
  "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "blockNumber": 3,
  "transactionIndex": 0,
  "from": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
  "to": "0x6295ee1b4f6dd65047762f924ecd367c17eabf8f",
  "value": BigNumber,
  "gas": 314159,
  "gasPrice": BigNumber,
  "input": "0x57cb2fc4"
}
*/

```

***

#### web3.eth.getTransactionFromBlock

```js
getTransactionFromBlock(hashStringOrNumber, indexNumber [, callback])
```
指定したブロック内にあるインデクス番号で指定したトランザクションを一つ返します。

##### パラメータ

1. `String` - ブロック番号、またはブロックのハッシュを指定します。または、Or the string `"earliest"`, `"latest"` or `"pending"` が指定できます。[default block parameter](#web3ethdefaultblock)を参照してください。
2. `Number` - トランザクションのインデクス番号。
3. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`Object` - トランザクションオブジェクトを返します。[web3.eth.getTransaction](#web3ethgettransaction)を参照してください。:


##### 使用例

```js
var transaction = web3.eth.getTransactionFromBlock('0x4534534534', 2);
console.log(transaction); // see web3.eth.getTransaction

```

***

#### web3.eth.getTransactionReceipt

```js
web3.eth.getTransactionReceipt(hashString [, callback])
```
トランザクションハッシュから、トランザクションの受領オブジェクトを取得します。

**Note** 受領オブジェクトは保留トランザクションには無効です。


##### パラメータ

1. `String` - The transaction hash.
2. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`Object` - トランザクションの受領オブジェクト。まだ存在しなければ`null`:

- `blockHash`: `String`, 32 Bytes - トランザクションを含むブロックのハッシュ。
- `blockNumber`: `Number` - トランザクションを含むブロックの番号
- `transactionHash`: `String`, 32 Bytes - トランザクションのハッシュ。
- `transactionIndex`: `Number` - ブロックの中でのトランザクションのインデックス番号。保留中ならば`null`
- `from`: `String`, 20 Bytes - 送信元のアドレス
- `to`: `String`, 20 Bytes - 受信先のアドレス。コントラクトの作成トランザクションの場合は`null`
- `cumulativeGasUsed `: `Number ` - ブロックが実行されたことで消費したgasの総量。
- `gasUsed `: `Number ` -  このトランザクションのみが使用したgasの量
- `contractAddress `: `String` - 20 Bytes - コントラクトを作成した場合は、そのアドレス。それ以外は`null`
- `logs `:  `Array` - トランザクションが生成したログオブジェクトの配列。
- `status `:  `Number` - 0:トランザクションは失敗した。, 1:トランアクションは成功した。

##### 使用例
```js
var receipt = web3.eth.getTransactionReceipt('0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b');
console.log(receipt);
{
  "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
  "transactionIndex": 0,
  "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "blockNumber": 3,
  "contractAddress": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         // logs as returned by getFilterLogs, etc.
     }, ...],
  "status": "0x1"
}
```

***

#### web3.eth.getTransactionCount

```js
web3.eth.getTransactionCount(addressHexString [, defaultBlock] [, callback])
```

指定したアドレスから送信したトランザクションの数を返します。

##### パラメータ

1. `String` - 送信したトランザクション数を得るアドレス。
2. `Number|String` - (optional) 調べるブロック番号、またはハッシュを指定します。省略するとデフォルトブロックを使います。[web3.eth.defaultBlock](#web3ethdefaultblock)を参照してください。
3. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`Number` - ブロックに含まれる、指定したアドレスから送信されたトランザクションの数。

##### 使用例

```js
var number = web3.eth.getTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(number); // 1
```

***

#### web3.eth.sendTransaction

```js
web3.eth.sendTransaction(transactionObject [, callback])
```

トランザクションをネットワークに送信します。

##### パラメータ

1. `Object` - 送信するトランザクションオブジェクト:
- `from`: `String` - 送信先のアドレス。省略時は、[web3.eth.defaultAccount](#web3ethdefaultaccount)プロパティの値を使います。
- `to`: `String` - (optional) 送信先のアドレス。コントラクト作成の場合は省略します。
- `value`: `Number|String|BigNumber` - (optional) Wei単位の送信額。コントラクトの作成トランザクションの場合は、付与金額。
- `gas`: `Number|String|BigNumber` - (optional, default:自動設定) トランザクションで使用するgasの量。(未使用分は返却される。)
- `gasPrice`: `Number|String|BigNumber` - (optional, default: 自動設定) Wei単位のgas価格。デフォルトではネットワークのガス価格。
- `data`: `String` - (optional) [byte string](https://github.com/ethereum/wiki/wiki/Solidity,-Docs-and-ABI) メッセージか、コントラクトの作成の場合は初期化コード。
- `nonce`: `Number`  - (optional) 整数のnonce値。同一な値の保留中のトランザクションを上書きすることができる。
2. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`String` - 32BytesのトランザクションハッシュのJEX文字列

もしトランザクションがコントラクト作成のものであれば、採掘後に[web3.eth.getTransactionReceipt()](#web3gettransactionreceipt) を使ってコントラクトのアドレスを得られる。

##### 使用例

```js

// compiled solidity source code using https://chriseth.github.io/cpp-ethereum/
var code = "603d80600c6000396000f3007c01000000000000000000000000000000000000000000000000000000006000350463c6888fa18114602d57005b6007600435028060005260206000f3";

web3.eth.sendTransaction({data: code}, function(err, transactionHash) {
  if (!err)
    console.log(transactionHash); // "0x7f9fade1c0d57a7af66ab4ead7c2eb7b11a91385"
});
```

***

#### web3.eth.sendRawTransaction

```js
web3.eth.sendRawTransaction(signedTransactionData [, callback])
```
署名済のトランザクションを送信します。署名については以下を参照してください。
https://github.com/SilentCicero/ethereumjs-accounts

##### パラメータ

1. `String` - Signed transaction data in HEX format
2. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`String` - 32Bytesのトランザクションハッシュ

もしトランザクションがコントラクト作成のものであれば、採掘後に[web3.eth.getTransactionReceipt()](#web3gettransactionreceipt) を使ってコントラクトのアドレスを得られる。

##### 使用例

```js
var Tx = require('ethereumjs-tx');
var privateKey = new Buffer('e331b6d69882b4cb4ea581d88e0b604039a3de5967688d3dcffdd2270c0fd109', 'hex')

var rawTx = {
  nonce: '0x00',
  gasPrice: '0x09184e72a000', 
  gasLimit: '0x2710',
  to: '0x0000000000000000000000000000000000000000', 
  value: '0x00', 
  data: '0x7f7465737432000000000000000000000000000000000000000000000000000000600057'
}

var tx = new Tx(rawTx);
tx.sign(privateKey);

var serializedTx = tx.serialize();

//console.log(serializedTx.toString('hex'));
//f889808609184e72a00082271094000000000000000000000000000000000000000080a47f74657374320000000000000000000000000000000000000000000000000000006000571ca08a8bbf888cfa37bbf0bb965423625641fc956967b81d12e23709cead01446075a01ce999b56a8a88504be365442ea61239198e23d1fce7d00fcfc5cd3b44b7215f

web3.eth.sendRawTransaction('0x' + serializedTx.toString('hex'), function(err, hash) {
  if (!err)
    console.log(hash); // "0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385"
});
```

***


#### web3.eth.sign

```js
web3.eth.sign(address, dataToSign, [, callback])
```

指定したアカウントでデータに署名します。アカウントはアンロックする必要があります。

##### パラメータ

1. `String` - 署名するアカウント。
2. `String` - 署名するデータ。
3. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`String` - 署名済みのデータ。

HEXプレフィックスの後の文字は、ECDSA値に対応します。
After the hex prefix, characters correspond to ECDSA values like this:
```
r = signature[0:64]
s = signature[64:128]
v = signature[128:130]
```

Note that if you are using `ecrecover`, `v` will be either `"00"` or `"01"`. As a result, in order to use this value, you will have to parse it to an integer and then add `27`. This will result in either a `27` or a `28`.

##### 使用例

```js
var result = web3.eth.sign("0x135a7de83802408321b74c322f8558db1679ac20",
    "0x9dd2c369a187b4e6b9c402f030e50743e619301ea62aa4c0737d4ef7e10a3d49"); // second argument is web3.sha3("xyz")
console.log(result); // "0x30755ed65396facf86c53e6217c52b4daebe72aa4941d89635409de4c9c7f9466d4e9aaec7977f05e923889b33c0d0dd27d7226b6e6f56ce737465c5cfd04be400"
```

***

#### web3.eth.call

    web3.eth.call(callObject [, defaultBlock] [, callback])

Executes a message call transaction, which is directly executed in the VM of the node, but never mined into the blockchain.

##### パラメータ

1. `Object` - A transaction object see [web3.eth.sendTransaction](#web3ethsendtransaction), with the difference that for calls the `from` property is optional as well.
2. `Number|String` - (optional) If you pass this parameter it will not use the default block set with [web3.eth.defaultBlock](#web3ethdefaultblock).
3. `Function` -  (optional) HTTPリクエストのコールバックを受け取る関数。指定すると、HTTPリクエストが非同期になります。詳細は[このドキュメント](#using-callbacks)を参照してください。

##### 戻り値

`String` - The returned data of the call, e.g. a codes functions return value.

##### 使用例

```js
var result = web3.eth.call({
    to: "0xc4abd0339eb8d57087278718986382264244252f", 
    data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
});
console.log(result); // "0x0000000000000000000000000000000000000000000000000000000000000015"
```

***

#### web3.eth.estimateGas

    web3.eth.estimateGas(callObject [, callback])

Executes a message call or transaction, which is directly executed in the VM of the node, but never mined into the blockchain and returns the amount of the gas used.

##### パラメータ

See [web3.eth.sendTransaction](#web3ethsendtransaction), except that all properties are optional.

##### 戻り値

`Number` - the used gas for the simulated call/transaction.

##### 使用例

```js
var result = web3.eth.estimateGas({
    to: "0xc4abd0339eb8d57087278718986382264244252f", 
    data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
});
console.log(result); // "0x0000000000000000000000000000000000000000000000000000000000000015"
```

***

#### web3.eth.filter

```js
// can be 'latest' or 'pending'
var filter = web3.eth.filter(filterString);
// OR object are log filter options
var filter = web3.eth.filter(options);

// watch for changes
filter.watch(function(error, result){
  if (!error)
    console.log(result);
});

// Additionally you can start watching right away, by passing a callback:
web3.eth.filter(options, function(error, result){
  if (!error)
    console.log(result);
});
```

##### パラメータ

1. `String|Object` - The string `"latest"` or `"pending"` to watch for changes in the latest block or pending transactions respectively. Or a filter options object as follows:
  * `fromBlock`: `Number|String` - The number of the earliest block (`latest` may be given to mean the most recent and `pending` currently mining, block). By default `latest`.
  * `toBlock`: `Number|String` - The number of the latest block (`latest` may be given to mean the most recent and `pending` currently mining, block). By default `latest`.
  * `address`: `String` - An address or a list of addresses to only get logs from particular account(s).
  * `topics`: `Array of Strings` - An array of values which must each appear in the log entries. The order is important, if you want to leave topics out use `null`, e.g. `[null, '0x00...']`. You can also pass another array for each topic with options for that topic e.g. `[null, ['option1', 'option2']]`

##### 戻り値

`Object` - A filter object with the following methods:

  * `filter.get(callback)`: Returns all of the log entries that fit the filter.
  * `filter.watch(callback)`: Watches for state changes that fit the filter and calls the callback. See [this note](#using-callbacks) for details.
  * `filter.stopWatching()`: Stops the watch and uninstalls the filter in the node. Should always be called once it is done.

##### Watch callback return value

- `String` - When using the `"latest"` parameter, it returns the block hash of the last incoming block.
- `String` - When using the `"pending"` parameter, it returns a transaction hash of the most recent pending transaction.
- `Object` - When using manual filter options, it returns a log object as follows:
    - `logIndex`: `Number` - integer of the log index position in the block. `null` when its pending log.
    - `transactionIndex`: `Number` - integer of the transactions index position log was created from. `null` when its pending log.
    - `transactionHash`: `String`, 32 Bytes - hash of the transactions this log was created from. `null` when its pending log.
    - `blockHash`: `String`, 32 Bytes - hash of the block where this log was in. `null` when its pending. `null` when its pending log.
    - `blockNumber`: `Number` - the block number where this log was in. `null` when its pending. `null` when its pending log.
    - `address`: `String`, 32 Bytes - address from which this log originated.
    - `data`: `String` - contains one or more 32 Bytes non-indexed arguments of the log.
    - `topics`: `Array of Strings` - Array of 0 to 4 32 Bytes `DATA` of indexed log arguments. (In *solidity*: The first topic is the *hash* of the signature of the event (e.g. `Deposit(address,bytes32,uint256)`), except if you declared the event with the `anonymous` specifier.)

**Note** For event filter return values see [Contract Events](#contract-events)

##### 使用例

```js
var filter = web3.eth.filter('pending');

filter.watch(function (error, log) {
  console.log(log); //  {"address":"0x0000000000000000000000000000000000000000", "data":"0x0000000000000000000000000000000000000000000000000000000000000000", ...}
});

// get all past logs again.
var myResults = filter.get(function(error, logs){ ... });

...

// stops and uninstalls the filter
filter.stopWatching();

```

***

#### web3.eth.contract

    web3.eth.contract(abiArray)

Creates a contract object for a solidity contract, which can be used to initiate contracts on an address.
You can read more about events [here](https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI#example-javascript-usage).

##### パラメータ

1. `Array` - ABI array with descriptions of functions and events of the contract.

##### 戻り値

`Object` - A contract object, which can be initiated as follows:

```js
var MyContract = web3.eth.contract(abiArray);

// instantiate by address
var contractInstance = MyContract.at(address);

// deploy new contract
var contractInstance = MyContract.new([constructorParam1] [, constructorParam2], {data: '0x12345...', from: myAccount, gas: 1000000});

// Get the data to deploy the contract manually
var contractData = MyContract.new.getData([constructorParam1] [, constructorParam2], {data: '0x12345...'});
// contractData = '0x12345643213456000000000023434234'
```

And then you can either initiate an existing contract on an address,
or deploy the contract using the compiled byte code:

```js
// Instantiate from an existing address:
var myContractInstance = MyContract.at(myContractAddress);


// Or deploy a new contract:

// Deploy the contract asynchronous from Solidity file:
...
const fs = require("fs");
const solc = require('solc')

let source = fs.readFileSync('nameContract.sol', 'utf8');
let compiledContract = solc.compile(source, 1);
let abi = compiledContract.contracts['nameContract'].interface;
let bytecode = compiledContract.contracts['nameContract'].bytecode;
let gasEstimate = web3.eth.estimateGas({data: bytecode});
let MyContract = web3.eth.contract(JSON.parse(abi));

var myContractReturned = MyContract.new(param1, param2, {
   from:mySenderAddress,
   data:bytecode,
   gas:gasEstimate}, function(err, myContract){
    if(!err) {
       // NOTE: The callback will fire twice!
       // Once the contract has the transactionHash property set and once its deployed on an address.

       // e.g. check tx hash on the first call (transaction send)
       if(!myContract.address) {
           console.log(myContract.transactionHash) // The hash of the transaction, which deploys the contract
       
       // check address on the second call (contract deployed)
       } else {
           console.log(myContract.address) // the contract address
       }

       // Note that the returned "myContractReturned" === "myContract",
       // so the returned "myContractReturned" object will also get the address set.
    }
  });

// Deploy contract syncronous: The address will be added as soon as the contract is mined.
// Additionally you can watch the transaction by using the "transactionHash" property
var myContractInstance = MyContract.new(param1, param2, {data: myContractCode, gas: 300000, from: mySenderAddress});
myContractInstance.transactionHash // The hash of the transaction, which created the contract
myContractInstance.address // undefined at start, but will be auto-filled later
```

##### 使用例

```js
// contract abi
var abi = [{
     name: 'myConstantMethod',
     type: 'function',
     constant: true,
     inputs: [{ name: 'a', type: 'string' }],
     outputs: [{name: 'd', type: 'string' }]
}, {
     name: 'myStateChangingMethod',
     type: 'function',
     constant: false,
     inputs: [{ name: 'a', type: 'string' }, { name: 'b', type: 'int' }],
     outputs: []
}, {
     name: 'myEvent',
     type: 'event',
     inputs: [{name: 'a', type: 'int', indexed: true},{name: 'b', type: 'bool', indexed: false}]
}];

// creation of contract object
var MyContract = web3.eth.contract(abi);

// initiate contract for an address
var myContractInstance = MyContract.at('0xc4abd0339eb8d57087278718986382264244252f');

// call constant function
var result = myContractInstance.myConstantMethod('myParam');
console.log(result) // '0x25434534534'

// send a transaction to a function
myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200, gas: 2000});

// short hand style
web3.eth.contract(abi).at(address).myAwesomeMethod(...);

// create filter
var filter = myContractInstance.myEvent({a: 5}, function (error, result) {
  if (!error)
    console.log(result);
    /*
    {
        address: '0x8718986382264244252fc4abd0339eb8d5708727',
        topics: "0x12345678901234567890123456789012", "0x0000000000000000000000000000000000000000000000000000000000000005",
        data: "0x0000000000000000000000000000000000000000000000000000000000000001",
        ...
    }
    */
});
```

***

#### Contract Methods

```js
// Automatically determines the use of call or sendTransaction based on the method type
myContractInstance.myMethod(param1 [, param2, ...] [, transactionObject] [, defaultBlock] [, callback]);

// Explicitly calling this method
myContractInstance.myMethod.call(param1 [, param2, ...] [, transactionObject] [, defaultBlock] [, callback]);

// Explicitly sending a transaction to this method
myContractInstance.myMethod.sendTransaction(param1 [, param2, ...] [, transactionObject] [, callback]);

// Get the call data, so you can call the contract through some other means
// var myCallData = myContractInstance.myMethod.request(param1 [, param2, ...]);
var myCallData = myContractInstance.myMethod.getData(param1 [, param2, ...]);
// myCallData = '0x45ff3ff6000000000004545345345345..'
```

The contract object exposes the contract's methods, which can be called using parameters and a transaction object.

##### パラメータ

- `String|Number|BigNumber` - (optional) Zero or more parameters of the function. If passing in a string, it must be formatted as a hex number, e.g. "0xdeadbeef" If you have already created BigNumber object, then you can just pass it too.
- `Object` - (optional) The (previous) last parameter can be a transaction object, see [web3.eth.sendTransaction](#web3ethsendtransaction) parameter 1 for more. **Note**: `data` and `to` properties will not be taken into account.
- `Number|String` - (optional) If you pass this parameter it will not use the default block set with [web3.eth.defaultBlock](#web3ethdefaultblock).
- `Function` - (optional) If you pass a callback as the last parameter the HTTP request is made asynchronous. See [this note](#using-callbacks) for details.

##### 戻り値

`String` - If its a call the result data, if its a send transaction a created contract address, or the transaction hash, see [web3.eth.sendTransaction](#web3ethsendtransaction) for details.


##### 使用例

```js
// creation of contract object
var MyContract = web3.eth.contract(abi);

// initiate contract for an address
var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

var result = myContractInstance.myConstantMethod('myParam');
console.log(result) // '0x25434534534'

myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200, gas: 2000}, function(err, result){ ... });
```

***


#### Contract Events

```js
var event = myContractInstance.MyEvent({valueA: 23} [, additionalFilterObject])

// watch for changes
event.watch(function(error, result){
  if (!error)
    console.log(result);
});

// Or pass a callback to start watching immediately
var event = myContractInstance.MyEvent([{valueA: 23}] [, additionalFilterObject] , function(error, result){
  if (!error)
    console.log(result);
});

```

You can use events like [filters](#web3ethfilter) and they have the same methods, but you pass different objects to create the event filter.

##### パラメータ

1. `Object` - Indexed return values you want to filter the logs by, e.g. `{'valueA': 1, 'valueB': [myFirstAddress, mySecondAddress]}`. By default all filter values are set to `null`. It means, that they will match any event of given type sent from this contract.
2. `Object` - Additional filter options, see [filters](#web3ethfilter) parameter 1 for more. By default filterObject has field 'address' set to address of the contract. Also first topic is the signature of event.
3. `Function` - (optional) If you pass a callback as the last parameter it will immediately start watching and you don't need to call `myEvent.watch(function(){})`. See [this note](#using-callbacks) for details.

##### Callback return


`Object` - An event object as follows:

- `address`: `String`, 32 Bytes - address from which this log originated.
- `args`: `Object` - The arguments coming from the event.
- `blockHash`: `String`, 32 Bytes - hash of the block where this log was in. `null` when its pending.
- `blockNumber`: `Number` - the block number where this log was in. `null` when its pending.
- `logIndex`: `Number` - integer of the log index position in the block.
- `event`: `String` - The event name.
- `removed`: `bool` -  indicate if the transaction this event was created from was removed from the blockchain (due to orphaned block) or never get to it (due to rejected transaction).
- `transactionIndex`: `Number` - integer of the transactions index position log was created from.
- `transactionHash`: `String`, 32 Bytes - hash of the transactions this log was created from.

##### 使用例

```js
var MyContract = web3.eth.contract(abi);
var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

// watch for an event with {some: 'args'}
var myEvent = myContractInstance.MyEvent({some: 'args'}, {fromBlock: 0, toBlock: 'latest'});
myEvent.watch(function(error, result){
   ...
});

// would get all past logs again.
var myResults = myEvent.get(function(error, logs){ ... });

...

// would stop and uninstall the filter
myEvent.stopWatching();
```

***

#### Contract allEvents

```js
var events = myContractInstance.allEvents([additionalFilterObject]);

// watch for changes
events.watch(function(error, event){
  if (!error)
    console.log(event);
});

// Or pass a callback to start watching immediately
var events = myContractInstance.allEvents([additionalFilterObject,] function(error, log){
  if (!error)
    console.log(log);
});

```

Will call the callback for all events which are created by this contract.

##### パラメータ

1. `Object` - Additional filter options, see [filters](#web3ethfilter) parameter 1 for more. By default filterObject has field 'address' set to address of the contract. This method sets the topic to the signature of event, and does not support additional topics.
2. `Function` - (optional) If you pass a callback as the last parameter it will immediately start watching and you don't need to call `myEvent.watch(function(){})`. See [this note](#using-callbacks) for details.

##### Callback return


`Object` - See [Contract Events](#contract-events) for more.

##### 使用例

```js
var MyContract = web3.eth.contract(abi);
var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

// watch for an event with {some: 'args'}
var events = myContractInstance.allEvents({fromBlock: 0, toBlock: 'latest'});
events.watch(function(error, result){
   ...
});

// would get all past logs again.
events.get(function(error, logs){ ... });

...

// would stop and uninstall the filter
events.stopWatching();
```

****

#### web3.eth.getCompilers
## Compiling features being deprecated https://github.com/ethereum/EIPs/issues/209
    web3.eth.getCompilers([callback])

Gets a list of available compilers.

##### パラメータ

1. `Function` - (optional) If you pass a callback the HTTP request is made asynchronous. See [this note](#using-callbacks) for details.

##### 戻り値

`Array` - An array of strings of available compilers.

##### 使用例

```js
var number = web3.eth.getCompilers();
console.log(number); // ["lll", "solidity", "serpent"]
```

***

#### web3.eth.compile.solidity

    web3.eth.compile.solidity(sourceString [, callback])

Compiles solidity source code.

##### パラメータ

1. `String` - The solidity source code.
2. `Function` - (optional) If you pass a callback the HTTP request is made asynchronous. See [this note](#using-callbacks) for details.

##### 戻り値

`Object` - Contract and compiler info.


##### 使用例

```js
var source = "" + 
    "contract test {\n" +
    "   function multiply(uint a) returns(uint d) {\n" +
    "       return a * 7;\n" +
    "   }\n" +
    "}\n";
var compiled = web3.eth.compile.solidity(source);
console.log(compiled); 
// {
  "test": {
    "code": "0x605280600c6000396000f3006000357c010000000000000000000000000000000000000000000000000000000090048063c6888fa114602e57005b60376004356041565b8060005260206000f35b6000600782029050604d565b91905056",
    "info": {
      "source": "contract test {\n\tfunction multiply(uint a) returns(uint d) {\n\t\treturn a * 7;\n\t}\n}\n",
      "language": "Solidity",
      "languageVersion": "0",
      "compilerVersion": "0.8.2",
      "abiDefinition": [
        {
          "constant": false,
          "inputs": [
            {
              "name": "a",
              "type": "uint256"
            }
          ],
          "name": "multiply",
          "outputs": [
            {
              "name": "d",
              "type": "uint256"
            }
          ],
          "type": "function"
        }
      ],
      "userDoc": {
        "methods": {}
      },
      "developerDoc": {
        "methods": {}
      }
    }
  }
}
```

***

#### web3.eth.compile.lll

    web3. eth.compile.lll(sourceString [, callback])

Compiles LLL source code.

##### パラメータ

1. `String` - The LLL source code.
2. `Function` - (optional) If you pass a callback the HTTP request is made asynchronous. See [this note](#using-callbacks) for details.

##### 戻り値

`String` - The compiled LLL code as HEX string.


##### 使用例

```js
var source = "...";

var code = web3.eth.compile.lll(source);
console.log(code); // "0x603880600c6000396000f3006001600060e060020a600035048063c6888fa114601857005b6021600435602b565b8060005260206000f35b600081600702905091905056"
```

***

#### web3.eth.compile.serpent

    web3.eth.compile.serpent(sourceString [, callback])

Compiles serpent source code.

##### パラメータ

1. `String` - The serpent source code.
2. `Function` - (optional) If you pass a callback the HTTP request is made asynchronous. See [this note](#using-callbacks) for details.

##### 戻り値

`String` - The compiled serpent code as HEX string.


```js
var source = "...";

var code = web3.eth.compile.serpent(source);
console.log(code); // "0x603880600c6000396000f3006001600060e060020a600035048063c6888fa114601857005b6021600435602b565b8060005260206000f35b600081600702905091905056"
```

***

#### web3.eth.namereg

    web3.eth.namereg

Returns GlobalRegistrar object.

##### 使い方

see [namereg](https://github.com/ethereum/web3.js/blob/master/example/namereg.html) example.

***

### web3.db

#### web3.db.putString

    web3.db.putString(db, key, value)

This method should be called, when we want to store a string in the local leveldb database.

##### パラメータ

1. `String` - The database to store to.
2. `String` - The name of the store.
3. `String` - The string value to store.

##### 戻り値

`Boolean` - `true` if successfull, otherwise `false`.

##### 使用例

 param is db name, second is the key, and third is the string value.
```js
web3.db.putString('testDB', 'key', 'myString') // true
```

***

#### web3.db.getString

    web3.db.getString(db, key)

This method should be called, when we want to get string from the local leveldb database.

##### パラメータ

1. `String` - The database string name to retrieve from.
2. `String` - The name of the store.

##### 戻り値

`String` - The stored value.

##### 使用例
 param is db name and second is the key of string value.
```js
var value = web3.db.getString('testDB', 'key');
console.log(value); // "myString"
```

***

#### web3.db.putHex

    web3.db.putHex(db, key, value)

This method should be called, when we want to store binary data in HEX form in the local leveldb database.

##### パラメータ

1. `String` - The database to store to.
2. `String` - The name of the store.
3. `String` - The HEX string to store.

##### 戻り値

`Boolean` - `true` if successfull, otherwise `false`.

##### 使用例
```js
web3.db.putHex('testDB', 'key', '0x4f554b443'); // true

```

***

#### web3.db.getHex

    web3.db.getHex(db, key)

This method should be called, when we want to get a binary data in HEX form from the local leveldb database.

##### パラメータ

1. `String` - The database to store to.
2. `String` - The name of the store.

##### 戻り値

`String` - The stored HEX value.


##### 使用例
 param is db name and second is the key of value.
```js
var value = web3.db.getHex('testDB', 'key');
console.log(value); // "0x4f554b443"
```

***

### web3.shh

[Whisper  Overview](https://github.com/ethereum/wiki/wiki/Whisper-Overview)

##### 使用例

```js
var shh = web3.shh;
```

***

#### web3.shh.post

   web3.shh.post(object [, callback])

This method should be called, when we want to post whisper message to the network.

##### パラメータ

1. `Object` - The post object:
  - `from`: `String`, 60 Bytes HEX - (optional) The identity of the sender.
  - `to`: `String`, 60 Bytes  HEX - (optional) The identity of the receiver. When present whisper will encrypt the message so that only the receiver can decrypt it.
  - `topics`: `Array of Strings` - Array of topics `Strings`, for the receiver to identify messages.
  - `payload`: `String|Number|Object` - The payload of the message. Will be autoconverted to a HEX string before.
  - `priority`: `Number` - The integer of the priority in a range from ... (?).
  - `ttl`: `Number` - integer of the time to live in seconds.
2. `Function` - (optional) If you pass a callback the HTTP request is made asynchronous. See [this note](#using-callbacks) for details.

##### 戻り値

`Boolean` - returns `true` if the message was sent, otherwise `false`.


##### 使用例

```js
var identity = web3.shh.newIdentity();
var topic = 'example';
var payload = 'hello whisper world!';

var message = {
  from: identity,
  topics: [topic],
  payload: payload,
  ttl: 100,
  workToProve: 100 // or priority TODO
};

web3.shh.post(message);
```

***

#### web3.shh.newIdentity

    web3.shh.newIdentity([callback])

Should be called to create new identity.

##### パラメータ

1. `Function` - (optional) If you pass a callback the HTTP request is made asynchronous. See [this note](#using-callbacks) for details.


##### 戻り値

`String` - A new identity HEX string.


##### 使用例

```js
var identity = web3.shh.newIdentity();
console.log(identity); // "0xc931d93e97ab07fe42d923478ba2465f283f440fd6cabea4dd7a2c807108f651b7135d1d6ca9007d5b68aa497e4619ac10aa3b27726e1863c1fd9b570d99bbaf"
```

***

#### web3.shh.hasIdentity

    web3.shh.hasIdentity(identity, [callback])

Should be called, if we want to check if user has given identity.

##### パラメータ

1. `String` - The identity to check.
2. `Function` - (optional) If you pass a callback the HTTP request is made asynchronous. See [this note](#using-callbacks) for details.

##### 戻り値

`Boolean` - returns `true` if the identity exists, otherwise `false`.


##### 使用例

```js
var identity = web3.shh.newIdentity();
var result = web3.shh.hasIdentity(identity);
console.log(result); // true

var result2 = web3.shh.hasIdentity(identity + "0");
console.log(result2); // false
```

***

#### web3.shh.newGroup

##### 使用例
```js
// TODO: not implemented yet
```

***

#### web3.shh.addToGroup

##### 使用例
```js
// TODO: not implemented yet
```

***

#### web3.shh.filter

```js
var filter = web3.shh.filter(options)

// watch for changes
filter.watch(function(error, result){
  if (!error)
    console.log(result);
});
```

Watch for incoming whisper messages.

##### パラメータ

1. `Object` - The filter options:
  * `topics`: `Array of Strings` - Filters messages by this topic(s). You can use the following combinations:
    - `['topic1', 'topic2'] == 'topic1' && 'topic2'`
    - `['topic1', ['topic2', 'topic3']] == 'topic1' && ('topic2' || 'topic3')`
    - `[null, 'topic1', 'topic2'] == ANYTHING && 'topic1' && 'topic2'` -> `null` works as a wildcard
  * `to`: Filter by identity of receiver of the message. If provided and the node has this identity, it will decrypt incoming encrypted messages.
2. `Function` - (optional) If you pass a callback the HTTP request is made asynchronous. See [this note](#using-callbacks) for details.

##### Callback return

`Object` - The incoming message:

  - `from`: `String`, 60 Bytes - The sender of the message, if a sender was specified.
  - `to`: `String`, 60 Bytes - The receiver of the message, if a receiver was specified.
  - `expiry`: `Number` - Integer of the time in seconds when this message should expire (?).
  - `ttl`: `Number` -  Integer of the time the message should float in the system in seconds (?).
  - `sent`: `Number` -  Integer of the unix timestamp when the message was sent.
  - `topics`: `Array of String` - Array of `String` topics the message contained.
  - `payload`: `String` - The payload of the message.
  - `workProved`: `Number` - Integer of the work this message required before it was send (?).

***

#### web3.eth.sendIBANTransaction

```js
var txHash = web3.eth.sendIBANTransaction('0x00c5496aee77c1ba1f0854206a26dda82a81d6d8', 'XE81ETHXREGGAVOFYORK', 0x100);
```

Sends IBAN transaction from user account to destination IBAN address.

##### パラメータ

- `string` - address from which we want to send transaction
- `string` - IBAN address to which we want to send transaction
- `value` - value that we want to send in IBAN transaction

***

#### web3.eth.iban

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
```

***

#### web3.eth.iban.fromAddress

```js
var i = web3.eth.iban.fromAddress('0x00c5496aee77c1ba1f0854206a26dda82a81d6d8');
console.log(i.toString()); // 'XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS
```

***

#### web3.eth.iban.fromBban

```js
var i = web3.eth.iban.fromBban('ETHXREGGAVOFYORK');
console.log(i.toString()); // "XE81ETHXREGGAVOFYORK"
```

***

#### web3.eth.iban.createIndirect

```js
var i = web3.eth.iban.createIndirect({
  institution: "XREG",
  identifier: "GAVOFYORK"
});
console.log(i.toString()); // "XE81ETHXREGGAVOFYORK"
```

***

#### web3.eth.iban.isValid

```js
var valid = web3.eth.iban.isValid("XE81ETHXREGGAVOFYORK");
console.log(valid); // true

var valid2 = web3.eth.iban.isValid("XE82ETHXREGGAVOFYORK");
console.log(valid2); // false, cause checksum is incorrect

var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var valid3 = i.isValid();
console.log(valid3); // true

```

***

#### web3.eth.iban.isDirect

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var direct = i.isDirect();
console.log(direct); // false
```

***

#### web3.eth.iban.isIndirect

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var indirect = i.isIndirect();
console.log(indirect); // true
```

***

#### web3.eth.iban.checksum

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var checksum = i.checksum();
console.log(checksum); // "81"
```

***

#### web3.eth.iban.institution

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var institution = i.institution();
console.log(institution); // 'XREG'
```

***

#### web3.eth.iban.client

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var client = i.client();
console.log(client); // 'GAVOFYORK'
```

***

#### web3.eth.iban.address

```js
var i = new web3.eth.iban('XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS');
var address = i.address();
console.log(address); // '00c5496aee77c1ba1f0854206a26dda82a81d6d8'
```

***

#### web3.eth.iban.toString

```js
var i = new web3.eth.iban('XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS');
console.log(i.toString()); // 'XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS'
```
