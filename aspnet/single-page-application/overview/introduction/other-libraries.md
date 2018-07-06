---
uid: single-page-application/overview/introduction/other-libraries
title: Knockout 以外のライブラリを認識しますか。 | Microsoft Docs
author: madskristensen
description: Knockout 以外のライブラリを認識しますか。
ms.author: aspnetcontent
ms.date: 02/05/2013
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 69006df6a5de3290ac294a309756eb0a53baa975
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834595"
---
<a name="know-a-library-other-than-knockout"></a>Knockout 以外のライブラリを認識しますか。
====================
によって[Mads Kristensen](https://github.com/madskristensen)

[Single Page Application (SPA) テンプレート](knockoutjs-template.md)はシングル ページ アプリケーションの記述を開始する優れた方法です。 テンプレートを使用して[KnockoutJS](http://knockoutjs.com/)アプリケーション データを DOM 要素にバインドします。

Knockout はリッチ クライアント アプリケーションを作成するための唯一の JavaScript ライブラリではありません。 その他のライブラリは、さまざまな方法でこのような課題を解決します。 行いましたコミュニティが作成したテンプレートがいくつかダウンロードできるように別、1 つのライブラリしたい場合があります。 これらの各テンプレートは、クライアントの JavaScript ライブラリの異なる組み合わせを使用します。

コミュニティが作成したテンプレートをインストールするには、ページは、以下に示すし、[ダウンロード] ボタンをクリックします。 テンプレートのいずれかを参照してください。 テンプレートは、VSIX ファイルとして提供されます。

## <a name="backbonejs"></a>BackboneJS

[Backbone.js SPA テンプレート](../templates/backbonejs-template.md)します。 このテンプレートを開発するため、初期のスケルトンには、 [Backbone.js](http://backbonejs.org/) ASP.NET MVC アプリケーションです。 既定は、ユーザーのサインアップ、サインイン、パスワードのリセットと基本的な電子メール テンプレートをユーザーの確認など、基本的なユーザーのログイン機能を提供します。

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) JavaScript クライアント内での豊富なデータを管理するためのオープン ソース ライブラリです。 簡単では、クエリを実行する、キャッシュ、変更の追跡、検証、および詳細を処理します。 2 つのテンプレート機能の簡単。

- [Breeze/knockout](../templates/breezeknockout-template.md)テンプレートは、シングル ページ アプリケーションを簡単にデータ管理および KnockoutJS のデータ バインディングをビルドする簡単にする方法を示す、Knockout SPA テンプレートを拡張します。
- [Breeze/angular](../templates/breezeangular-template.md)拡張し、Knockout SPA テンプレートを簡単を使用して、テンプレート、 [AngularJS](http://angularjs.org)データ バインド、依存関係の挿入、および画面の管理用のライブラリです。

さらに、[ホット タオル SPA テンプレート](../templates/hottowel-template.md)BreezeJS を使用します。

## <a name="emberjs"></a>EmberJS

[EmberJS SPA テンプレート](../templates/emberjs-template.md)します。 このテンプレートでは[Ember](http://emberjs.com/)さまざまなリッチ クライアント アプリケーションを構築するための課題を解決する強力な MVC JavaScript ライブラリです。

Ember SPA テンプレートは、EmberJS と Handlebars のテンプレートを使用して、Knockout SPA テンプレートの再実装です。

## <a name="hot-towel"></a>ホットのタオル

[ホットのタオル SPA テンプレート](../templates/hottowel-template.md)します。 このテンプレートは、簡単、Knockout、RequireJS Twitter Bootstrap など、いくつかの JavaScript ライブラリで表示されます。

次のとおり、他のテンプレートと比べると、ホット タオル teample できますをビルドする独自のより完全なアプリケーションを提供します。 、注意すべきその他の概念がありますが、このテンプレートがある可能性がありますだけそれらを理解すると探しているもの。 秒にする必要があります、SPA とすべてのツールとホット タオルの使用を開始する場所を決定することはできませんが、SPA を構築する場合は、その上に構築する必要があります。

## <a name="feature-table"></a>機能テーブル

各 SPA テンプレートによって提供される機能を次に示します。


|                        | ASP.NET SPA | バックボーン | Breeze/Angular | Breeze/KO |  Ember   | ホットのタオル |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      ToDo サンプル       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     ベア テンプレート      |             | &#10003; |                |           |          | &#10003;  |
| ナビゲーションと履歴 |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        ライブラリ        |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;バックボーン     |             | &#10003; |                |           |          |           |
|         簡単です。         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

