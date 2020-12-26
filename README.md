<div style="text-align:center;width:100%">
<img src="https://raw.githubusercontent.com/nekonium/nekonium.github.io/master/nekonium.png" width="50%"/>
</div>
<br/>
<br/>

# 注意喚起

日本国内の利用者向けに注意喚起を掲載しました。ソフトウェアの使用状態を確認して、場合によっては使用を停止してください。
<https://github.com/nekonium/nekonium.github.io/blob/master/pr/pr2018061501.md>

---

NekoniumはEthereumクローンの分散型アプリケーションプラットフォームです。Ethereumコピーコインの実装例の提示と、Ethereumの機能を学習する目的で起動されました。Nekoniumの送受信や採掘を通して、Ethereumと仮想通貨のシステムを学んでいただければ幸いです。

NekoniumはNekonium Projectが開発しています。これは一般的な非中央集権型の投機通貨ではなく、採掘や投資で経済的な利益を得ることは難しいので注意してください。また、Nekonium projectはNekoniumを法定通貨へ換金する手段を提供しません。投機目的での入手、第三者からの購入はしないでください。

公開されているソフトウェアに起因するあらゆる損害、トラブルに関して、Nekonium Projectは責任を負いません。内部通貨の消失を含むトラブルについても、一切責任を負うことができません。利用者の責任において使用してください。

# スペック

Nekoniumは、イーサリウムのパラメータを僅かに調整しただけのコピーコインです。
内部通貨として、Nekonium(NUKO)を発行しており、採掘が可能です。

```plain
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

[Premineについて](https://nekonium.github.io/premine.html)

# 起動スケジュール

* ~~Closed Beta (～6/28)~~
* ~~Open Beta (6/29～)~~
* ✅Release　20000ブロックの採掘が完了しましたので、正式版としてリリースしました。ご自由に遊んでください。~~Discordで配布も行っております。giveawayチャンネルにアドレスを張ってください。(100NUKO配布します)~~ 終了しました。

# ダウンロード

## gnekonium

<https://github.com/nekonium/go-nekonium>

go-ethereum(geth)を基にしたnekoniumのクライアントソフトウェアです。

### マイニング

go-nekoniumリリースからバイナリ、またはソースコードをダウンロードしてください。ソースからビルドする場合はEthereumの取説のgethをgnekoniumに読み替えて頑張ってください。 簡単な始め方はこちらをご覧ください。[Get Started](https://nekonium.github.io/getstarted.html)

gnekoniumのコンソールからマイニングすることができます。

```plain
$gnekonium console
:
:(いろいろたくさん)
:
>miner.start(2)
```

## ウォレットアプリケーション

<https://github.com/nekonium/mist>

Ethereum Mistベースのウォレットです。マイニングはできません。現在はオートアップデートが機能しませんので、gnekoniumを起動してから使用してください。

<img src="https://github.com/nekonium/nekonium.github.io/blob/master/img/mistpreview.png?raw=true" width="50%"></img>

## ブラウザエクステンション

<https://chrome.google.com/webstore/detail/nukomask/glchbnjfkbkdhhaclogbdbkkkoahcnmf>

MetamaskをNekonium用にカスタマイズしたブラウザエクステンションです。

## モバイルウォレット
モバイルウォレットは停止しました。
~~<https://play.google.com/store/apps/details?id=org.nekonium.androidnuko>
Android用のモバイルウォレットです。~~


# サービス

## Wallet

* <https://www.nukowallet.com/> officiall webwallet
* <https://monya-wallet.github.io/> もにゃ

## Block exproler

* <http://explorer.nekonium.org/home>
* ~~http[]()://nukoexplorer.oldbeyond.com/~~ (停止中）
* <http://nekonium.network/>
* <https://scout.paws.pink/> Nekoscout

## Pool

* ~~http[]()://nuko.oldbeyond.com/~~ (停止中)
* <http://nuko.ftlpool.com/#/>
* <http://nuko.coinminer.space/#/>
* ~~http[]()://www.nekonium-pool.com/~~(ドメイン失効)
* ~~http[]()://nuko.cep-k.work/~~(接続不可)
* <https://nuko.mofumofu.me/>
* <https://minerpool.net/>
* ~~https[]()://nuko.solopool.org/~~

## Faucet

* ~~http[]()://nuko.oldbeyond.com/#/faucet~~(停止中)
* <http://namuyan.dip.jp/nekoniumFaucet.php>
* ~~https[]()://faucet.nekonium.net/~~(ドメイン失効)
* <http://faucet.nekonium.network/>

## SNS

* Twitter <https://twitter.com/NekoniumDev>
* Discord channel <https://discord.gg/C8mJg44>
* ASKmona <http://askmona.org/5387>
* Bitcointalk <https://bitcointalk.org/index.php?topic=2012213.0>
* ~~Wiki https://nekonium.tk/wiki/~~

## etc

* ~~http[]()://stats.nekonium.io/~~ Network status(停止中)
* <https://stats.mofumofu.me/> Network status
* <http://stuff.cuppadev.co.uk/nuko-report.html> rich list
* <https://minerpool.net/pools/nekonium/> Active Pool list
* <http://nekonium.org/nodewatch/web/summary.html> Node watch
* <http://nekonium.org/tokenfactory/#/> Token Factory
* <http://nekonium.org/nukosousinki/index.html> ぬこ送信機




## Contract-Dev

* Browser-Solidity IDE (NEW) <http://nekonium.org/browser-solidity/02/>
* (OLD) <http://nekonium.org/browser-solidity/01.85884478a57de99508250b0e1b625e9afd1e7eaf>
* [Nekoniumのコントラクトの作り方](https://github.com/nekonium/nekonium.github.io/blob/master/documents/browser-solidity/solidity.JP.md)

# 配布について

Nekoniumは採掘行為、個人間での譲渡で手に入れることができます。
海外のCEXでの取り扱い、相対取引での入手についての質問にはお答えできません。

[Premineについて](https://github.com/nekonium/nekonium.github.io/blob/master/premine.md)

# Resources

* [Logo/Images etc](https://github.com/nekonium/nekonium.github.io/blob/master/resources.image/resources.md)
* [Source codes](https://github.com/nekonium/nekonium.github.io/blob/master/software.md)
* [Documents](https://github.com/nekonium/nekonium.github.io/tree/master/documents/README.md)

# 返礼品制度

返礼品制度は現在行っておりません。
~~Nekoniumの開発やインフラ設置に対する返礼品制度です。詳細はこちらをご覧ください。
<https://docs.google.com/document/d/1_Q3AaRrTTatCiDvhoprR1PQLH0hOT97CzcDMHGEv9jE/pub>~~

# 開発メンバー

* てばさき
* ふとん
* hxcoin
* なむやんぐ
* かばやき
* 7Zz
* mike_theminer
