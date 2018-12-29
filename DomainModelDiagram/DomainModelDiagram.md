# PlantUMLを通じてドメインモデル図の書き方を学ぶ

## はじめに
---
ソフトウェアの仕様書、設計書の作成や管理を効率化するために、Markdown + PlantUMLによる作成方法を日々模索しています。
しかしながら、そもそもUML図の正式な書き方というものをちゃんと分かっていないというのが正直なところなので、PlantUMLを通じてUMLの各種図の書き方を勉強していきます。

- 本稿のテーマは、「UMLによるドメインモデル図の書き方」です。
- 本稿におけるUML図の作成は、Markdown + PlantUMLがベースであることを前提とします。

## 目次
---
<!-- TOC -->

- [PlantUMLを通じてドメインモデル図の書き方を学ぶ](#plantumlを通じてドメインモデル図の書き方を学ぶ)
    - [はじめに](#はじめに)
    - [目次](#目次)
    - [プロジェクトの開始時にやるべきこと](#プロジェクトの開始時にやるべきこと)
    - [ドメインモデル図とは](#ドメインモデル図とは)
    - [ドメインモデル図を描く手順](#ドメインモデル図を描く手順)
        - [1. 「名詞」の抽出](#1-名詞の抽出)
        - [2. モデル同士の関係を線と矢印で表す](#2-モデル同士の関係を線と矢印で表す)
        - [3. 中心となるモデルに色を付ける](#3-中心となるモデルに色を付ける)
    - [ドメインモデル図を用いる際の注意点](#ドメインモデル図を用いる際の注意点)
    - [ドメインモデル図の活用方法](#ドメインモデル図の活用方法)
    - [ドメインモデル図の作成例](#ドメインモデル図の作成例)
        - [例1. 認証システムの場合](#例1-認証システムの場合)
        - [2. 課金システムの場合](#2-課金システムの場合)
        - [3. 本屋の場合](#3-本屋の場合)
        - [4. 投稿システムの場合](#4-投稿システムの場合)
    - [参考資料](#参考資料)

<!-- /TOC -->

## プロジェクトの開始時にやるべきこと
---
- 設計対象であるシステムが取り扱うものの概念について、チーム内で共通認識を築く
- それらの概念の繋がりを表現したのがドメインモデル図である。

## ドメインモデル図とは
---
- ユーザの視点で見た、システムに登場する「もの」の概念(ドメインクラス)を集めた図である。
- プロジェクトの用語集をクラス図風に表現した図ということにもなる。
- 自然言語で構成するため、要件定義や仕様の把握に有効である。

## ドメインモデル図を描く手順
---
例として、通販サイトの要件からドメインモデル図を描く場合を考える。

### 1. 「名詞」の抽出

通販サイトの要件から下記のような名詞が抽出されたとする。  

「商品、型番、価格、カテゴリ、カート、注文、レビュー、ウィッシュリスト、ユーザ、登録情報、メールアドレス」  

これらの名詞を図に並べてみる。  

```plantuml
hide circle
hide method

package DomainModel {
    class 商品
    class カテゴリ
    class レビュー
    class カート
    class 注文
    class ウィッシュリスト
    class 登録情報
    mix_actor ユーザ

    商品 : 型番
    商品 : 価格
    登録情報 : メールアドレス
}
```

名詞は「ドメインクラス、ドメインクラスの属性、アクタ」の３種類に分けられ、判別方法は下記のようになる。  

- アクタ: ユーザや外部システム
- ドメインクラスの属性: 商品の型番や値段など、○○の△△という形で書けるもの
- ドメインクラス: 上記以外の名詞

### 2. モデル同士の関係を線と矢印で表す

線の種類は、「has-one, has-many, is-a-kind-of」の３種類とする。  

```plantuml
left to right direction

hide circle
hide method

package DomainModel {
    class 商品
    class カテゴリ
    class レビュー
    class カート
    class 注文
    class ウィッシュリスト
    class 登録情報
    mix_actor ユーザ

    商品 : 型番
    商品 : 価格
    登録情報 : メールアドレス

    ユーザ -> 登録情報 : has-one
    ユーザ --> カート
    カート o--> 商品 : has-many
    商品 <--o カテゴリ
    ユーザ o--> 注文
    注文 o--> 商品
    ユーザ --> ウィッシュリスト
    ウィッシュリスト o--> 商品
    ユーザ o--> レビュー
    レビュー <--o 商品
}
```
さらに、商品に「予約商品、在庫商品」の２種類があるとすれば下記のようになる。  

```plantuml
left to right direction

hide circle
hide method

package DomainModel {
    class 商品
    class 予約商品
    class 在庫商品
    class カテゴリ
    class レビュー
    class カート
    class 注文
    class ウィッシュリスト
    class 登録情報
    mix_actor ユーザ

    商品 : 型番
    商品 : 価格
    登録情報 : メールアドレス

    ユーザ -> 登録情報 : has-one
    ユーザ --> カート
    カート o--> 商品 : has-many
    商品 <--o カテゴリ
    ユーザ o--> 注文
    注文 o--> 商品
    ユーザ --> ウィッシュリスト
    ウィッシュリスト o--> 商品
    ユーザ o--> レビュー
    レビュー <--o 商品

    商品 <|-- 予約商品 : is-a-kind-of
    商品 <|-- 在庫商品 : is-a-kind-of
}
```

### 3. 中心となるモデルに色を付ける

```plantuml
left to right direction

hide circle
hide method

package DomainModel {
    class 商品 #F5A9BC
    class 予約商品 #F5A9BC
    class 在庫商品 #F5A9BC
    class カテゴリ
    class レビュー
    class カート
    class 注文 #CEF6F5
    class ウィッシュリスト
    class 登録情報 #A9C6F6
    mix_actor ユーザ

    商品 : 型番
    商品 : 価格
    登録情報 : メールアドレス

    ユーザ -> 登録情報 : has-one
    ユーザ --> カート
    カート o--> 商品 : has-many
    商品 <--o カテゴリ
    ユーザ o--> 注文
    注文 o--> 商品
    ユーザ --> ウィッシュリスト
    ウィッシュリスト o--> 商品
    ユーザ o--> レビュー
    レビュー <--o 商品

    商品 <|-- 予約商品 : is-a-kind-of
    商品 <|-- 在庫商品 : is-a-kind-of
}
```

最終的な上記の図を描くPlantUMLのコードは下記の通り。  

```
left to right direction

hide circle
hide method

package DomainModel {
    class 商品 #F5A9BC
    class 予約商品 #F5A9BC
    class 在庫商品 #F5A9BC
    class カテゴリ
    class レビュー
    class カート
    class 注文 #CEF6F5
    class ウィッシュリスト
    class 登録情報 #A9C6F6
    mix_actor ユーザ

    商品 : 型番
    商品 : 価格
    登録情報 : メールアドレス

    ユーザ -> 登録情報 : has-one
    ユーザ --> カート
    カート o--> 商品 : has-many
    商品 <--o カテゴリ
    ユーザ o--> 注文
    注文 o--> 商品
    ユーザ --> ウィッシュリスト
    ウィッシュリスト o--> 商品
    ユーザ o--> レビュー
    レビュー <--o 商品

    商品 <|-- 予約商品 : is-a-kind-of
    商品 <|-- 在庫商品 : is-a-kind-of
}
```

## ドメインモデル図を用いる際の注意点
---
- 後の詳細設計を行う工程でも、意味的なモデルであるところのドメインモデル図の更新を怠らないようにする。
- 要件とコードの間で仕様の食い違いが起こらないようにする。
- 別名として「概念モデル図」と呼ばれることもある。

## ドメインモデル図の活用方法
---
- 最初に図を描いた時点では、必ず図のどこかに「認識があいまいな部分」が出てくる。
- あいまいな部分を見つけたのは勝利と見なして、仕様の再確認を行う。
- プロジェクトが進行して仕様変更が出てきたとき、ドメインモデル図は概念の再確認の場として役に立つ。

## ドメインモデル図の作成例
---
### 例1. 認証システムの場合

```plantuml
left to right direction

hide circle
hide method

class ユーザ
class メール認証
class OAuth認証

ユーザ : ニックネーム
ユーザ : メール
メール認証 : ユーザID
メール認証 : パスワード
OAuth認証 : ユーザID
OAuth認証 : プロバイダ
OAuth認証 : トークン

ユーザ o-- メール認証
ユーザ o-- OAuth認証
```

### 2. 課金システムの場合

```plantuml
left to right direction

hide circle
hide method

class ユーザ
class クレジットカード
class 継続課金
class 本屋
class 決済
class 課金プラン

課金プラン : 期間
課金プラン : 金額
継続課金 : ユーザID
継続課金 : プランID
継続課金 : 本屋ID
決済 : 継続課金ID
決済 : インボイスID
決済 : ステータス（完了、失敗、待ち）
クレジットカード : ユーザID
クレジットカード : トークン

ユーザ o-- 継続課金
ユーザ o- クレジットカード
課金プラン o- 継続課金
継続課金 o- 決済
継続課金 --o 本屋
```

### 3. 本屋の場合

```plantuml
left to right direction

hide circle
hide method

class 投稿
class 本屋
class 従業員
class ユーザ

本屋 : 名前
本屋 : カバーURL
本屋 : アイコンURL
本屋 : メッセージ
従業員 : ユーザID
従業員 : 本屋ID

投稿 --o 本屋
本屋 o- 従業員
従業員 -- ユーザ
```

### 4. 投稿システムの場合

```plantuml
left to right direction

hide circle
hide method

class 投稿
class 本屋
class コメント
class ユーザ
class いいね

ユーザ o-- いいね
ユーザ o-- コメント
いいね --o 投稿
コメント --o 投稿
投稿 --o 本屋
```

## 参考資料
---
- [Asial/Developers/Blog UMLを描こう](http://blog.asial.co.jp/archive/category/UML)
- [僕らが本屋の未来を変えるまで ドメインモデルの設計図](http://little-staff.hatenablog.com/entry/2017/10/07/125845)