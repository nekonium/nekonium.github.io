<div style="text-align:center;width:100%">
<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/nekonium.png" width="50%"/>
</div>
<br/>
<br/>


NekoniumはEthereumクローンの分散型アプリケーションプラットフォームです。Ethereumコピーコインの実装例の提示と、Ethereumの機能を学習する目的で起動されました。Nekoniumの送受信や採掘を通して、Ethereumと仮想通貨のシステムを学んでいただければ幸いです。

NekoniumはNekonium Projectが開発しています。一般的な非中央集権型の投機通貨ではなく、採掘や投資で利益が得られる可能性は低いので注意してください。

公開されているソフトウェアに起因するあらゆる損害、トラブルに関して、Nekonium Projectは責任を負いません。内部通貨の消失を含むトラブルについても、一切責任を負うことができません。利用者の責任において使用してください。


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

<a href="https://nekonium.github.io/premine.html">Premineについて</a>


# 起動スケジュール

* Closed Beta (～6/28)
 開発チーム内での起動実験を実施します。終了時にジェネシスブロックの変更を含むブロックチェーンの初期化を実施します。
* Open Beta (6/29～6/30)　
 リリース時と同じ状況で、ハッシュレートを制限して各種チェックを実施します。最大で100000ブロックの採掘を実施します。ネットワークが不安定な状態での動作チェックのため、フォークが多発し、トランザクションが巻き戻る可能性があります。公開範囲は限定です。致命的な問題がない限り、ジェネシスブロックの変更は実施しません。
* Release(7月予定)
 ハッシュレートの制限を解除してバイナリを公開します。

現在はClosed Betaです。ジェネシスブロックの変更を含む、ブロックチェーンの巻き戻しが発生する可能性があります。


# ダウンロード

## gnekonium
<a href="https://github.com/nekonium/go-nekonium">https://github.com/go-nekonium/go-nekonium</a> 

go-ethereum(geth)を基にしたnekoniumのクライアントソフトウェアです。

### マイニング
go-nekoniumリリースからバイナリ、またはソースコードをダウンロードしてください。ソースからビルドする場合はEthereumの取説のgethをgnekoniumに読み替えて頑張ってください。

gnekoniumのコンソールからマイニングすることができます。

    $gnekonium console
    :
    :(いろいろたくさん)
    :
    >miner.start(2)

## ウォレット

<a href="https://github.com/nekonium/mist">https://github.com/nekonium/mist</a> 

Ethereum Mistベースのウォレットです。マイニングはできません。


# 配布について
Nekoniumは採掘で手に入れることもできますが、事前に確保したNekoniumの配布も実施しております。ネットワーク維持のためのマイニングや常設ノードの起動、その他サービス（プールやファーセット）の設置にも報酬があります。配布と報酬については<b>Premineについて</b>を参照してください。

<a href="https://nekonium.github.io/premine.html">Premineについて</a>

