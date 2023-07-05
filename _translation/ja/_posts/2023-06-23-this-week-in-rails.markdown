---
author: gregmolnar
categories: お知らせ
date: 2023-06-23
layout: post
published: true
title: 'Rails Worldウェブサイトとチケット販売、CPKの改善など盛り沢山！'
---


どうも、[Greg](https://github.com/gregmolnar)です。
Railsのコードベースにあった最新の変更をお届けします。

[Rails Worldのサイトが公開されました！](https://rubyonrails.org/world)  
Rails Worldカンファレンスのウェブサイトが本日より公開されました。
こちらはRailsコミュニティの若手開発者の[Shona
Chan](https://www.linkedin.com/in/shona-chan/)により製作されました。
Rails Foundationから委託されたもので、[Katya
Sitko](https://katya-sitko.netlify.app/)により設計され、[Ayush
Newatia](https://radioactivetoy.tech/)の指導の下で製作されました。

[Rails Worldのチケット販売が本日始まりました！](https://twitter.com/rails/status/1671880869706084354)  
Rails
Worldのチケット販売について、枚数限定のアーリーバードチケットを皮切りに、本日6月23日金曜日の中央ヨーロッパ夏時間午後5時に始まりました。
この機会をお見逃しなく、アムステルダムでお会いしましょう！

[EncryptedConfigurationがハッシュらしく振る舞っていなかった点を修正](https://github.com/rails/rails/pull/48556)  
以前、キーがメソッドのように呼べるよう、`EncryptedConfiguration`は`InheritableOptions`を使用していました。
後に`Hash`のように、そして`OrderedOptions`のように振る舞うように変更されました。
この2つ目の変更で意図せず`#keys`、`~#to_h`といったメソッドが壊れてしまい、認証情報が設定されていたとしても、`Rails.application.credentials.to_h`が空のハッシュを返してしまっていました。
本プルリクエストではこの問題を修正しており、修正は[7-0-stableにもバックポート](https://github.com/rails/rails/pull/48557)されました。

[複合主キーのhas_one関係でnullにする対応を追加](https://github.com/rails/rails/pull/48539)  
このプルリクエストが作成されたのは、`has_one`複合主キー (CPK) の`dependent:
:nullify`を伴う関係が壊れていたためです。
`ActiveRecord::Associations::HasOneAssociation`のnull化コードを変更し、外部キーカラムを巡回してモデルの主キーに属さない場合はnull化するようにしています。

[*id*をActive Recordでの列挙値にすることを禁止](https://github.com/rails/rails/pull/48536)  
この変更では`id`をActive Recordの列挙値として禁止しています。
これは、期待されていないジョインと副問い合わせを誘発しかねないからです。
それでも使いたい場合は`\_prefix`ないし`\_suffix`オプションを設定すればできます。

[可能な場合はAction TextがHTML5を使うよう更新](https://github.com/rails/rails/pull/48522)  
このプルリクエストは可能な場合にAction
TextがHTML5サニタイザを使い、また可能な場合に`Nokogiri::HTML5`を使ってマークアップを構文解析するよう更新しています。
これまで、Action Textはこうした操作にNokogiriのHTML4構文解析器を使っていました。

[*Rails.application.config#inspect*で*secret_key_base*を表示しないよう変更](https://github.com/rails/rails/pull/48500)  
`Rails.application.config#inspect`を呼び出すと全ての属性を表示していましたが、それには`secret_key_base`も含まれていました。
本プルリクエストでは`inspect`メソッドをオーバーライドしてクラス名のみ表示するようにしています。
これにより意図せず機密情報を出力してしまうことを避けるのに役立ちます。
同じ変更は[MessageEncryptor](https://github.com/rails/rails/pull/48499)と[EncryptedConfiguration](https://github.com/rails/rails/pull/48498)でもなされています。

[*to_fs*が無開端または無終端の範囲に対応](https://github.com/rails/rails/pull/48494)  
このプルリクエストは`to_fs`に無開端・無終端の範囲への対応を追加しています。
以下の例のような動作をします。
```ruby
> (0..1).to_fs(:db)
=> "BETWEEN '0' AND '1'"
> (..1).to_fs(:db)
=> "< '1'"
> (0..).to_fs(:db)
=> "> '0'"
```

[不具合修正：CPKの一部が変化した際に*has_one*関係が必ず保存されるように変更](https://github.com/rails/rails/pull/48491)  
*has_one*関係が複合主キーを使っており、複合主キーの一部が所有者の元で変更された場合に、これらの変更は従属するオブジェクトの外部キーに反映される必要があります。
これは以前うまく動いていませんでした。
というのも`#_record_changed?`は複合主キーの関係を扱うように実装されていなかったためで、所有者の主キーが変更された際に、従属するオブジェクトの外部キーが更新されなければならないことを認識していなかったためです。

[複合主キーのモデルを積極読み込み](https://github.com/rails/rails/pull/48490)  
このプルリクエストでは、モデルの積極読み込みと複合主キーとの関係を修正する上で、複合主キーを伴うノードを適切に扱うための依存代入のジョインを変更しています。

[has_one関係を自動保存する際に\_read_attributeを使用](https://github.com/rails/rails/pull/48489)  
このプルリクエストでは、複合主キーモデルの関係上の主キーが`:id`に設定された場合に、そのモデルが適切に自動保存されない問題を修正しています。

[関係副問い合わせにスコープを適用](https://github.com/rails/rails/pull/48487)  
このプルリクエストでは関係（*belongs\_to/has\_one/has\_many*）副問い合わせにスコープが適用されるようにするものです。
例えば`has_many :welcome_posts, -> { where(title: "welcome") }`の関係があったとします。

以前：
```ruby
Author.where(welcome_posts: Post.all)
#=> SELECT (...) WHERE "authors"."id" IN (SELECT "posts"."author_id" FROM "posts")
```

以降：
```ruby
Author.where(welcome_posts: Post.all)
#=> SELECT (...) WHERE "authors"."id" IN (SELECT "posts"."author_id" FROM "posts" WHERE "posts"."title" = 'welcome')
```

*変更の全一覧については[こちらをご覧下さい](https://github.com/rails/rails/compare/@%7B2023-06-16%7D...main@%7B2023-06-23%7D)。*  
*この1週間に[21名の方々](https://contributors.rubyonrails.org/contributors/in-time-window/20230616-20230623)からRailsのコードベースに貢献していただきました！*

また次回！  

_[購読](https://world.hey.com/this.week.in.rails)するとこうした更新をメールで受け取れます。_
