NekoniumはEthereumクローンの分散型アプリケーションプラットフォームです。Ethereumコピーコインの実装例の提示と、Ethereumの機能を学習する目的で起動されました。

現在のNekoniumはNekonium Projectが管理しています。非中央集権型の投機通貨ではなく、採掘や投資で利益が得られる可能性は低いので注意してください。

## スペック
Nekoniumは、イーサリウムのパラメータを僅かに調整しただけのコピーコインです。
内部通貨として、Nekonium(NUKO)を発行しており、採掘が可能です。

```
* 名称 nekonium
* 単位 NUKO
* Premine 12,448,421 NUKO
* TCPポート番号 28568
* UDPポート番号 28569
* RPC(HTTP) 8293
* RPC(WS) 8294
* Reword 7.5NUKO
* BlockTime	19sec
* GasLimitBoundDivisor　1024
* DifficultyBoundDivisor 512
```

## 起動スケジュール

* Closed Beta (～6/28)
ブロックチェーンのロールバックを含む
* Open Beta (6/29～6/30)　
リリース時と同じ状況で各種チェックを実施します。最大で10000ブロックの採掘を実施します。
公開範囲は限定です。致命的な問題がない限りロールバックは実施しません。
* Release(7月予定)
一般向けにバイナリを公開します。

現在はクローズドベータ段階です。ブロックチェーンの巻き戻しが発生する可能性があります。



## ダウンロード

# gnekonium
gethを基にしたnekoniumのクライアントソフトウェアです。
go-nekoniumは<a href="">go-nekonium</a>からバイナリ、またはソースコードをダウンロードしてください。ソースからビルドする場合はEthereumの取説のgethをgnekoniumに読み替えて頑張ってください。

gnekoniumのコンソールからマイニングすることができます。
    $gnekonium console
    :
    :(いろいろたくさん)
    :
    >miner.start(2)
<<gnekoniumリリースへのリンク>>
## ウォレット
Ethereum Mistベースのウォレットです。マイニングはできません。
<<nekonium mistリリースへのリンク>>


## 配布について
Nekoniumは採掘で手に入れることもできますが、お問い合わせいただければ事前に確保したNekonium(NUKO)を無償提供いたします。
また、ネットワーク維持のためのマイニングや常設ノードの起動、その他サービス（プールやファチェット）の設置には薄謝を差し上げます。
＜＜詳細＞＞

