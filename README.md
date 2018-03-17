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
* Premine 12,448,421 NUKO
* 採掘上限なし
* TCP/UDPポート番号 28568
* UDPポート番号 28566
* RPC(HTTP) 8293
* RPC(WS) 8294
* Reward 7.5NUKO
* BlockTime	
*   0 - 0  Block      Frontier 19sec
*   - 7776 Block      Homestead 10-20sec target DiffBoundDiv=512
*   7777 - Block      Homestead 19-29sec target　Without Exp BOM DiffBoundDiv=1024
* GasLimitBoundDivisor　1024
* DifficultyBoundDivisor 1024
```

<a href="https://nekonium.github.io/premine.html">Premineについて</a>


# 起動スケジュール

* <s>Closed Beta (～6/28)</s>
* <s>Open Beta (6/29～)</s>　
* ✅Release　20000ブロックの採掘が完了しましたので、正式版としてリリースしした。ご自由に遊んでください。<s>Discordで配布も行っております。giveawayチャンネルにアドレスを張ってください。(100NUKO配布します)</s>終了しました。




# ダウンロード

## gnekonium
<a href="https://github.com/nekonium/go-nekonium">https://github.com/nekonium/go-nekonium</a> 

go-ethereum(geth)を基にしたnekoniumのクライアントソフトウェアです。

### マイニング
go-nekoniumリリースからバイナリ、またはソースコードをダウンロードしてください。ソースからビルドする場合はEthereumの取説のgethをgnekoniumに読み替えて頑張ってください。 簡単な始め方はこちらをご覧ください。<a href="https://nekonium.github.io/getstarted.html">Get Started</a>

gnekoniumのコンソールからマイニングすることができます。

    $gnekonium console
    :
    :(いろいろたくさん)
    :
    >miner.start(2)

## ウォレットアプリケーション

<a href="https://github.com/nekonium/mist">https://github.com/nekonium/mist</a> 

Ethereum Mistベースのウォレットです。マイニングはできません。現在はオートアップデートが機能しませんので、gnekoniumを起動してから使用してください。

<img src="https://github.com/nekonium/nekonium.github.io/blob/master/img/mistpreview.png?raw=true" width="50%"></img>

## モバイルウォレット
<a href="https://play.google.com/store/apps/details?id=org.nekonium.androidnuko">https://play.google.com/store/apps/details?id=org.nekonium.androidnuko</a>

Android用のモバイルウォレットです。

<img src="http://nekonium.github.io/pr/imgs/cp01-1.png" width="33%"/>

# サービス
## Wallet
* <a href="http://www.nukowallet.com/">http://www.nukowallet.com/</a> officiall webwallet

## Block exproler
* <a href="http://explorer.nekonium.org/home/home">http://explorer.nekonium.org/home</a>
* <a href="http://nukoexplorer.oldbeyond.com/">http://nukoexplorer.oldbeyond.com/</a>
* <a href="http://nekonium.network/">http://nekonium.network/</a>


## Pool
* <a href="http://nuko.oldbeyond.com/">http://nuko.oldbeyond.com/</a>
* <a href="http://nuko.ftlpool.com/#/">http://nuko.ftlpool.com/#/</a>
* <a href="http://nuko.coinminer.space/#/">http://nuko.coinminer.space/#/</a>
* <a href="http://www.nekonium-pool.com/">http://www.nekonium-pool.com/</a>
* <a href="http://nuko.cep-k.work/">http://nuko.cep-k.work/</a>
* <a href="http://nuko.mofumofu.me/">http://nuko.mofumofu.me/</a>
* <a href="https://minerpool.net/">https://minerpool.net/</a>

## Faucet
* <a href="http://nuko.oldbeyond.com/#/faucet">http://nuko.oldbeyond.com/#/faucet</a>
* <a href="http://namuyan.dip.jp/nekoniumFaucet.php">http://namuyan.dip.jp/nekoniumFaucet.php</a>
* <a href="https://faucet.nekonium.net/">https://faucet.nekonium.net/</a>
* <a href="http://faucet.nekonium.network/">http://faucet.nekonium.network/</a>

## SNS
* Twitter <a href="https://twitter.com/NekoniumDev">https://twitter.com/NekoniumDev</a>
* Discord channel <a href="https://discord.gg/C8mJg44">https://discord.gg/C8mJg44</a>
* ASKmona <a href="http://askmona.org/5387">http://askmona.org/5387</a>
* Bitcointalk <a href="https://bitcointalk.org/index.php?topic=2012213.0">https://bitcointalk.org/index.php?topic=2012213.0</a>


## etc
* <a href="http://stats.nekonium.io/">http://stats.nekonium.io/</a> Network status
* <a href="http://stuff.cuppadev.co.uk/nuko-report.html">http://stuff.cuppadev.co.uk/nuko-report.html</a> rich list
* <a href="https://minerpool.net/pools/nekonium/">https://minerpool.net/pools/nekonium/</a> Active Pool list
* <a href="http://nekonium.org/nodewatch/web/summary.html">http://nekonium.org/nodewatch/web/summary.html</a> Node watch


## QR generator
* <a href="http://qr.nekonium.info/">http://qr.nekonium.info/</a>
* <a href="https://nuko-qr.azurewebsites.net/">https://nuko-qr.azurewebsites.net/</a>


## Contract-Dev
* Browser-Solidity IDE (NEW) <a href="http://nekonium.org/browser-solidity/02/">http://nekonium.org/browser-solidity/02/</a>
* (OLD) <a href="http://nekonium.org/browser-solidity/01.85884478a57de99508250b0e1b625e9afd1e7eaf">http://nekonium.org/browser-solidity/01.85884478a57de99508250b0e1b625e9afd1e7eaf</a>
* <a href="https://github.com/nekonium/nekonium.github.io/blob/master/documents/browser-solidity/solidity.JP.md">Nekoniumのコントラクトの作り方</a>

# 配布について
Nekoniumは採掘で手に入れることもできますが、事前に確保したNekoniumの配布も実施しております。ネットワーク維持のためのマイニングや常設ノードの起動、その他サービス（プールやファーセット）の設置にも報酬があります。配布と報酬については<b>Premineについて</b>を参照してください。

<a href="https://github.com/nekonium/nekonium.github.io/blob/master/premine.md">Premineについて</a>

# Resources

* <a href="https://github.com/nekonium/nekonium.github.io/blob/master/resources.image/resources.md">Logo/Images etc</a>
* <a href="https://github.com/nekonium/nekonium.github.io/blob/master/software.md">Source codes</a>
* <a href="https://github.com/nekonium/nekonium.github.io/tree/master/documents/README.md">Documents</a>

# 開発メンバー

* てばさき
* ふとん
* hxcoin
* なむやんぐ
* かばやき
* 7Zz
* mike_theminer
