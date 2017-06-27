<div style="text-align:center;width:100%">
<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/nekonium.png" width="50%"/>
</div>
<br/>
<br/>


NekoniumはEthereumクローンの分散型アプリケーションプラットフォームです。Ethereumコピーコインの実装例の提示と、Ethereumの機能を学習する目的で起動されました。Nekoniumの送受信や採掘を通して、Ethereumと仮想通貨のシステムを学んでいただければ幸いです。

NekoniumはNekonium Projectが開発しています。一般的な非中央集権型の投機通貨ではなく、採掘や投資で利益が得られる可能性は低いので注意してください。


# スペック
Nekoniumは、イーサリウムのパラメータを僅かに調整しただけのコピーコインです。
内部通貨として、Nekonium(NUKO)を発行しており、採掘が可能です。

```
* 名称 nekonium
* 単位 NUKO
* Premine 12,448,421 NUKO　(1年分の採掘量と同等)
* 採掘上限なし
* TCPポート番号 28568
* UDPポート番号 28569
* RPC(HTTP) 8293
* RPC(WS) 8294
* Reword 7.5NUKO
* BlockTime	19sec
* GasLimitBoundDivisor　1024
* DifficultyBoundDivisor 512
```

[Premineについて](https://nekonium.github.io/premine.html "")

# 起動スケジュール

* Closed Beta (～6/28)
 開発チーム内での起動実験を実施します。終了時にブロックチェーンを初期化します。
* Open Beta (6/29～6/30)　
 リリース時と同じ状況で各種チェックを実施します。最大で10000ブロックの採掘を実施します。
 公開範囲は限定です。致命的な問題がない限りロールバックは実施しません。
* Release(7月予定)
 バイナリを公開します。

現在はClosed Betaです。ブロックチェーンの巻き戻しが発生する可能性があります。


# ダウンロード

## gnekonium
<<gnekoniumリリースへのリンク>> 
gethを基にしたnekoniumのクライアントソフトウェアです。

### マイニング
go-nekoniumは<a href="">go-nekonium</a>からバイナリ、またはソースコードをダウンロードしてください。ソースからビルドする場合はEthereumの取説のgethをgnekoniumに読み替えて頑張ってください。

gnekoniumのコンソールからマイニングすることができます。

    $gnekonium console
    :
    :(いろいろたくさん)
    :
    >miner.start(2)

## ウォレット
<<nekonium mistリリースへのリンク>> 
Ethereum Mistベースのウォレットです。マイニングはできません。


# 配布について
Nekoniumは採掘で手に入れることもできますが、事前に確保したNekoniumの配布も実施しております。ネットワーク維持のためのマイニングや常設ノードの起動、その他サービス（プールやファチェット）の設置にも報酬があります。配布と報酬については__Premineについて__を参照してください。

[Premineについて](https://nekonium.github.io/premine.html "")

