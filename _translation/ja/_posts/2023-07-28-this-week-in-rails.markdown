---
author: zzak
categories: お知らせ
date: 2023-07-28
layout: post
published: true
title: '今週Railsにあったこと - 2023年7月28日'
---


こんにちは、[zzak](https://github.com/zzak)です。
Railsのコードベースにあった変更を探索しましょう。

[Rack::LintをRailsのミドルウェアのテストに適用](https://github.com/skipkayhil/rails/issues/5)  
今週は技術的には利用者が目にするものはありませんでしたが、将来のRailsが[Rack
SPEC](https://github.com/rack/rack/blob/main/SPEC.rdoc)との互換性を維持し続けることになるということは重要です。
Rackに依存するライブラリに興味があったり保守していたりする向きには、[Rack
3への更新の手引き](https://github.com/rack/rack/blob/6d16306192349e665e4ec820a9bfcc0967009b6a/UPGRADE-GUIDE.md)で詳細を読めます。
  

[ダンプ時にSchemaCacheの構成子を整列](https://github.com/rails/rails/pull/48824)  
このPRにより生成結果のスキーマキャッシュが一貫したものになり、ファイルダイジェストがキャッシュキーに使えるようになります。
  

[音響分析器のメタ情報にタグを追加](https://github.com/rails/rails/pull/48823)  
この変更により、音響ファイルを分析する際、Active Storageによりメタ情報から追加のタグが提供されます。
例えばエンコーダーにアクセスする必要がある場合に便利でしょう。
  

["capture_emails"と"capture_broadcasts"を導入](https://github.com/rails/rails/pull/48798)  
このPRは "assert_emails" と "assert_broadcasts"
に以前加えられた未リリースの挙動を元に戻し、これら2つの新しいメソッドに追加します。
  

[ActiveRecordの引用符で囲まれた名前がJRuby/TruffleRubyでスレッド安全にキャッシュされるように変更](https://github.com/rails/rails/pull/48773)  
このコミットはTruffleRubyでRailsアプリケーションを走らせる際のスレッド安全上の問題を解決します。
ActiveRecordの引用符で囲まれた名前のキャッシュはSQL問い合わせ構築時に更新されます。
2スレッドが同時に問い合わせを構築する場合、両方ともキャッシュの更新を試みることが有り得ます。
これにより2スレッドが全く異なるキャッシュストアを持つ事態が置こり得るため、異なるスレッド間で複数のキャッシュが生まれることになり、余剰のメモリを消費したり不具合修正で混乱したりする可能性がありました。
  

[不要であればデータ伝送のinspectを抑止](https://github.com/rails/rails/pull/48772)  
現在 "ActionCable::Channel::Base#transmit"
の呼び出し時には毎回、与えられたデータオブジェクトをinspectするデバッグログ文言が生成されます。
この文言はロガーの水準がWARN以上であっても生成されます。
このパッチはログに記録される場合のみにこの文言が生成されるようにしています。
  

[全てのキャッシュストアが"#delete"で真偽値を返すように変更](https://github.com/rails/rails/pull/48638)  
このPRは "Rails.cache.delete('key')" の挙動が一貫したものになるようにし、項目が存在するときは "true"
を（あるいはそうでないときは "false" を）返すようにします。
以前はRedisCacheStoreをFileStoreは異なる挙動をしていました。
  

[列挙体における "where.missing" と "where.associated" の挙動を復元](https://github.com/rails/rails/pull/48738)  
このPRは "associated" メソッドを呼ぶ際の回帰不具合を修正します。
不正確なSQL問い合わせになり紛らわしい結果になっていました。
  

[キャッシュ項目の脱直列化を遅延化](https://github.com/rails/rails/pull/48754)  
この変更はキャッシュ最適化を加え、期限切れであったりバージョンが合わなかったりするキャッシュ項目について、値を脱直列化することなく検出できるようにしました。
この最適化はキャッシュ形式のバージョンが7.1以上のものか自前の直列化器を使っている場合に有効化されます。  
  

[キャッシュ圧縮器の置き換えに対応](https://github.com/rails/rails/pull/48451)  
このPRは直列化されたキャッシュ項目用に使われる圧縮機を置き換える対応を加えます。
置き換えには "config.cache_store" に ":compressor" オプションを指定します。
キャッシュされた値に責務を負う直列化器を置き換える ":coder" オプションと似ており、そちらでは
"ActiveSupport::Cache::Entry" インスタンス全体を直列化することに責務があるのでした。
  


*変更の全一覧については[こちらをご覧下さい](https://github.com/rails/rails/compare/@%7B2023-07-21%7D...main@%7B2023-07-28%7D)。*  
*この1週間に[20名の方々](https://contributors.rubyonrails.org/contributors/in-time-window/20230721-20230728)からRailsのコードベースに貢献していただきました！*

また次回！  

_[購読](https://world.hey.com/this.week.in.rails)するとこうした更新をメールで受け取れます。_
