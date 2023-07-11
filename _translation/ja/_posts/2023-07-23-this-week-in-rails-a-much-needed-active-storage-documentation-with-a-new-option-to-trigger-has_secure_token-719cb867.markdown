---
author: siaw23
categories: お知らせ
date: 2023-07-23
layout: post
published: true
title: '待望のActive Storageのドキュメントとhas_secure_tokenをトリガーする新しいオプション'
---

こちら[Emmanuel](https://hayford.dev/)です。
久しくお知らせしてきませんでしたが、恙無く過ごしています。
以下は共有したいマージされたプルリクエストの一部です。

[MessageVerifier#inspectとKeyGenerator#inspectで秘密情報を表示しないように変更](https://github.com/rails/rails/pull/48680)。
コントソールで暗号鍵を呼び出すとき、常に暗号化器の秘密情報が開示されていました。
inspectメソッドをオーバーライドしてクラス名のみを表示するようにし、機密情報が漏洩される事故を防いでいます。

[Active
Recordの返値、中断、例外発生時のトランザクションのコミット](https://github.com/rails/rails/pull/48600)。
このPRに関しては沢山の経緯がありました。
手短かに言うと、Rails 7.1では新しい構成オプションが追加されます。
このオプションでは、トランザクションのブロック内で**返値**や**処理の中断**や**例外発生**があったとき、トランザクションがコミットされるかロールバックされるかを定義します。
こちらの例を見てみましょう。

```ruby
Model.transaction do
  model.save
  return
  other_model.save
end
```

**config.active\_record.commit\_transaction\_on\_non\_local\_return**が**false**に設定されているとき、トランザクションは返値に突き当たるや否やロールバックします。
**true**に設定されている場合、トランザクションはコミットされます。

[よくあるActiveStorageの問題をドキュメント化](https://github.com/rails/rails/pull/48722)。
**has\_many\_attached**関係へファイルを添付するとき、既定の挙動では既存の添付物を置換します。
しかし、既存の添付物を保存しておいて新しいものを追加したい場合は、**Rails.application.config.replace\_on\_assign\_to\_many**を**false**に設定することでアーカイブできます。
このPRではこの挙動が適切にドキュメント化されています。


[has\_secure\_tokenを生成するタイミングを指定](https://github.com/rails/rails/pull/47420)。
Railsには**has\_secure\_token**メソッドがあり、**SecureRandom::base58**を使ってモデル用の24文字の一意なトークンを生成しています。
このPRにより、**on:**オプションを介してモデルのライフサイクルのどの時点でトークンが生成されるかを指定できるようになりました。
そのため以下のようなことができます。

```ruby
class User < ApplicationRecord
  has_secure_token on: :initialize
end
```

**_on: :initialize_**を渡すと、**after\_initialize**コールバックでトークンが生成されます。
これは既定の挙動である**before\_\***コールバックとは対照的です。

この14日間に[37名の方々](https://contributors.rubyonrails.org/contributors/in-time-window/20230707-20230721)から惜しみない貢献をいただきました。

今日のところは以上です！




[**Rails**](https://github.com/rails/rails)からの注目すべきコミット、プルリクエストなどを週報でお届けします。

<p><i><a href="https://world.hey.com/this.week.in.rails">購読</a>するとこうした更新をメールで受け取れます。</i></p>
