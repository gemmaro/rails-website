---
author: wojtek
categories: お知らせ
date: 2023-07-07
layout: post
published: true
title: '前処理されたActive Storageの変種などなど'
---

どうも、こちら[Wojtek](https://twitter.com/morgoth85)です。
Railsのコードベースで何が変わったか見てみましょう。

[バックグラウンドのジョブでActive Storageの変種を前処理するオプションを追加](https://github.com/rails/rails/pull/47473)  
Active Storageの変種 (variant) は必要なときにその場で処理されます。
しかしこれらがアクセス済みであれば予め処理しておきたいことがあります。
```ruby
class User < ApplicationRecord
  has_one_attached :avatar do |attachable|
    attachable.variant :thumb, resize_to_limit: [100, 100], preprocessed: true
  end
end
```

[Action Textに構成sanitizer_vendorを導入](https://github.com/rails/rails/pull/48644)  
対応している場合、既定のRails 7.1の構成で`Rails::HTML5::SafeListSanitizer`を使用します。
Active Textのサニタイザは`config.action_text.sanitizer_vendor`を設定して構成できます。

[ActiveSupport::JSON.encodeの効率を向上](https://github.com/rails/rails/pull/48614)  
`.to_json`/`ActiveSupport::JSON.encode`の性能が約2倍になります。
プルリクエストにベンチマークが含まれています。

[`stylesheet_link_tag`ないし`javascript_include_tag`が呼ばれたときに`Link preload`ヘッダのオプトイン・オプトアウトができるようにします](https://github.com/rails/rails/pull/48553)  
```ruby
# 設定で有効でもヘッダを除外
javascript_include_tag("http://example.com/all.js", preload_links_header: false)
```

[クエリログが有効の場合にデータベースのプリペアドステートメントを無効化](https://github.com/rails/rails/pull/48631)  
プリペアドステートメントとクエリログの両機能は釣り合いが取れません。
クエリログは全てのクエリを一意なものにし、Active Recordはそれぞれについて新しいプリペアドステートメントを作成するためです。
これによりシステムリソースの枯渇を招く可能性がありました。

[逆向きに単一の構築を経由するhas_oneを修正](https://github.com/rails/rails/pull/48674)  
逆向きに単一の関係を経由する*has_one*を持つ関係からレコードが構築できるようになりました。
関係を経由する*belongs_to*において外部キーを主キーのモデルに紐付ける必要はありません。

[入れ子の`field_id`と`field_name`のインデックスを二重にエンコードしないようにする](https://github.com/rails/rails/pull/47436)  
これは、深く入れ子になった関係に対しフォーム構築子に余剰のインデックス引数を追加していた不具合を修正しています。

*変更の全一覧については[こちらをご覧下さい](https://github.com/rails/rails/compare/@%7B2023-06-30%7D...main@%7B2023-07-07%7D)。*  
*この1週間に[24名の方々](https://contributors.rubyonrails.org/contributors/in-time-window/20230630-20230707)からRailsのコードベースに貢献していただきました！*

また次回！  

_[購読](https://world.hey.com/this.week.in.rails)するとこうした更新をメールで受け取れます。_
