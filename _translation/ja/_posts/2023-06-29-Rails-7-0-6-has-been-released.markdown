---
author: rafaelfranca
categories: リリース
date: '2023-06-29 17:00:00 -04:00'
layout: post
published: true
title: 'Rails 7.0.6がリリースされました！'
---

どうも皆さん。

お蔭様でRails 7.0.6がリリースされました。


## 7.0.5からの変更

各gemの変更についてはGitHubで変更履歴をお読みください。
* [Action
Cableの変更履歴](https://github.com/rails/rails/blob/v7.0.6/actioncable/CHANGELOG.md)
* [Action
Mailboxの変更履歴](https://github.com/rails/rails/blob/v7.0.6/actionmailbox/CHANGELOG.md)
* [Action
Mailerの変更履歴](https://github.com/rails/rails/blob/v7.0.6/actionmailer/CHANGELOG.md)
* [Action
Packの変更履歴](https://github.com/rails/rails/blob/v7.0.6/actionpack/CHANGELOG.md)
* [Action
Textの変更履歴](https://github.com/rails/rails/blob/v7.0.6/actiontext/CHANGELOG.md)
* [Action
Viewの変更履歴](https://github.com/rails/rails/blob/v7.0.6/actionview/CHANGELOG.md)
* [Active
Jobの変更履歴](https://github.com/rails/rails/blob/v7.0.6/activejob/CHANGELOG.md)
* [Active
Modelの変更履歴](https://github.com/rails/rails/blob/v7.0.6/activemodel/CHANGELOG.md)
* [Active
Recordの変更履歴](https://github.com/rails/rails/blob/v7.0.6/activerecord/CHANGELOG.md)
* [Active
Storageの変更履歴](https://github.com/rails/rails/blob/v7.0.6/activestorage/CHANGELOG.md)
* [Active Supportの変更履歴][as]
* [Railtiesの変更履歴][r]

[as]: https://github.com/rails/rails/blob/v7.0.6/activesupport/CHANGELOG.md<
[r]: https://github.com/rails/rails/blob/v7.0.6/railties/CHANGELOG.md

変更の要約についてはGitHubでリリースノートをお読み下さい。

[7.0.6の変更履歴](https://github.com/rails/rails/releases/tag/v7.0.6)

*全体のコード*

変更の全一覧については[GitHubで全コミットをご確認下さい](https://github.com/rails/rails/compare/v7.0.5...v7.0.6)。

## SHA-256

落としてきたgemがアップロードされているものと同じか検証されたい場合は、以下のSHA-256ハッシュをお使いください。

以下は7.0.6のチェックサムです。

```
$ shasum -a 256 *-7.0.6.gem
ac88be1e287cd9f228e6d4c0884278790210e59eaa23b603f58025183a66e2b4  actioncable-7.0.6.gem
083ac338246640b13cb10c93777fe734e30ce89f82411f46b3929e7c9bf3b38b  actionmailbox-7.0.6.gem
33b442a99408d4b96558365ce92debb6c53204f8eaa8237741c594e7640ca107  actionmailer-7.0.6.gem
6c659afbffdbb32323df1d44882830206f5d7dc81ed3d840736ac39c5ae0c634  actionpack-7.0.6.gem
d459eff29352433f5963bd191ff2e22a74cb6b21c2edbed09aef57f01074fe2e  actiontext-7.0.6.gem
ad328cec415058ae083ac471f9a4531050c8980a046fedfb67ce5d57b0c55285  actionview-7.0.6.gem
78148bf1f0f011b5d9c8ed66d787728a174cb01a20cebc5e0f2a8352cce25cbd  activejob-7.0.6.gem
5231fe572d7599237b6b2a4987ebf439d028754a529227630777dd3d1ad8511c  activemodel-7.0.6.gem
38ff1e7bbf57132fe78abf3809e8fe9392b54d00df31ce8972f47bb806b119d0  activerecord-7.0.6.gem
9270183f7fee6ab4677b079a85ec2abeb041b2a7207c49dd81b5965c46dad50c  activestorage-7.0.6.gem
d3abba6b15d8fd7af07f3322c370f00eb8ce6715fb714436342999628c705ab2  activesupport-7.0.6.gem
5dfbd481a23556ad425fc8541399a129a08ed550f877294b44d0170ca5b9f421  rails-7.0.6.gem
fb0ee3c7a3beea7f4b353c655f9a055c31ca36c3a2a565afaed415baca5c8a35  railties-7.0.6.gem
```

毎度のことながら、本リリースでご助力いただいた数多くの貢献者の方々に深く感謝致します。
