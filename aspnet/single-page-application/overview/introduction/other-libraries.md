---
uid: single-page-application/overview/introduction/other-libraries
title: Knockout 以外のライブラリを認識しますか。 | Microsoft Docs
author: madskristensen
description: Knockout 以外のライブラリを認識しますか。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 6ac260e88fd156bad4b414e93325d5a04c490c88
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872586"
---
<a name="know-a-library-other-than-knockout"></a>Knockout 以外のライブラリを認識しますか。
====================
によって[Mads Kristensen](https://github.com/madskristensen)

[単一ページ アプリケーション (SPA) テンプレート](knockoutjs-template.md)単一ページ アプリケーションの作成を開始する優れた方法です。 テンプレートを使用して[KnockoutJS](http://knockoutjs.com/) DOM 要素にアプリケーション データをバインドします。

Knockout は、リッチ クライアント アプリケーションを作成するための唯一の JavaScript ライブラリではありません。 その他のライブラリは、さまざまな方法で同様の課題を解決します。 しましたコミュニティによって作成されたテンプレートがいくつかダウンロードできるように別、1 つのライブラリしたい場合があります。 これらの各テンプレートは、クライアントの JavaScript ライブラリのさまざまな組み合わせを使用します。

コミュニティが作成したテンプレートをインストールするには、ページは、以下に示すし、[ダウンロード] ボタンをクリックして、テンプレートのいずれかを参照してください。 テンプレートは、VSIX ファイルとして提供されます。

## <a name="backbonejs"></a>BackboneJS

[Backbone.js SPA テンプレート](../templates/backbonejs-template.md)です。 このテンプレートでは、初期のスケルトンを開発するため、 [Backbone.js](http://backbonejs.org/) ASP.NET mvc アプリケーション。 すぐに、ユーザーのサインアップ、サインイン、パスワードのリセットと基本的な電子メール テンプレートを使用してユーザーの確認など、基本的なユーザー ログインの機能を提供します。

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) JavaScript クライアントでの豊富なデータを管理するためのオープン ソース ライブラリです。 簡単では、クエリを実行する、キャッシュ、変更の追跡、検証、および詳細を処理します。 2 つのテンプレートは機能もとても簡単です。

- [簡単/Knockout](../templates/breezeknockout-template.md)テンプレートが簡単にできますアプリケーションをビルドする単一ページもとても簡単でのデータ管理および KnockoutJS データ バインディングの表示、Knockout SPA テンプレートを拡張します。
- [簡単/角](../templates/breezeangular-template.md)テンプレート拡張することも、とても簡単ですを使用して、Knockout SPA テンプレート、 [AngularJS](http://angularjs.org)データ バインディング、依存関係の挿入、および画面管理ライブラリです。

さらに、[ホット タオル SPA テンプレート](../templates/hottowel-template.md)BreezeJS を使用します。

## <a name="emberjs"></a>EmberJS

[EmberJS SPA テンプレート](../templates/emberjs-template.md)です。 このテンプレートを使用して[Ember](http://emberjs.com/)さまざまなリッチ クライアント アプリケーションを構築するための課題を解決する強力な MVC JavaScript ライブラリです。

Ember SPA テンプレートは、EmberJS およびハンドルのテンプレートを使用して、Knockout SPA テンプレートの再実装です。

## <a name="hot-towel"></a>ホット タオル

[ホット タオル SPA テンプレート](../templates/hottowel-template.md)です。 このテンプレートは、とても簡単です、Knockout、RequireJS ブートス トラップの Twitter など、いくつかの JavaScript ライブラリで表示されます。

次のとおり、他のテンプレートと比べると、ホット タオル teample 元となることができますをビルドする独自のより完全なアプリケーションを提供します。 対応する複数の概念がありますがそれらを理解するとこのテンプレートだけあります探しているものです。 秒にする必要があります、SPA およびすべてのツールと、SPA をビルドするが、開始、ホット タオルを使用する場所を決定することはできません、上でビルドする必要があります。

## <a name="feature-table"></a>機能テーブル

各 SPA テンプレートによって提供される機能を次に示します。


|                        | ASP.NET SPA | Backbone | 簡単/角度 | 簡単/KO |  Ember   | ホット タオル |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      ToDo サンプル       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     ベア テンプレート      |             | &#10003; |                |           |          | &#10003;  |
| ナビゲーションおよび履歴 |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        ライブラリ        |             |          |                |           |          |           |
|        angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         とても簡単です。         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

