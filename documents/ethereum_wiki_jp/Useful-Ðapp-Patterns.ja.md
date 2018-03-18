From: https://github.com/ethereum/wiki/wiki/Useful-%C3%90app-Patterns


以下のページはブロックチェーンとやり取りするDAPPSを使う使用例です。

実装例は完全ではなく、今後変更する可能性があります。

## Examples

- web3をインスタンス化する３つの方法:

https://gist.github.com/frozeman/fbc7465d0b0e6c1c4c23


- コードからコントラクトをデプロイする。:

(時代遅れなので、これを使ってください。 `web3.contract(abiArray).new({}, function(e, res){...})`)
https://gist.github.com/frozeman/655a9325a93ac198416e

- デプロイする前に`call`でコントラクトをテストする:

https://gist.github.com/ethers/2d8dfaaf7f7a2a9e4eaa
