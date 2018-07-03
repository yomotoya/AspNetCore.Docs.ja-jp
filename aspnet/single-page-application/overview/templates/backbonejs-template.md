---
uid: single-page-application/overview/templates/backbonejs-template
title: Backbone テンプレート |Microsoft Docs
author: madskristensen
description: Backbone.js SPA テンプレート
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 641e149155fbee2655024bec3b76dce5243e7d59
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362506"
---
<a name="backbone-template"></a>Backbone テンプレート
====================
によって[Mads Kristensen](https://github.com/madskristensen)

> Kazi Manzur Rashid によってバックボーンの SPA テンプレートが作成されました
> 
> [Backbone.js SPA テンプレートをダウンロードします。](https://go.microsoft.com/fwlink/?LinkId=293631)


Backbone.js SPA テンプレートを使用して対話型のクライアント側 web アプリをすばやく構築を開始するために設計された[Backbone.js します。](http://backbonejs.org/)

テンプレートは、ASP.NET MVC で Backbone.js アプリケーションの開発の初期のスケルトンを提供します。 既定は、ユーザーのサインアップ、サインイン、パスワードのリセットと基本的な電子メール テンプレートをユーザーの確認など、基本的なユーザーのログイン機能を提供します。

要件:

- [ASP.NET and Web Tools 2012.2 の更新](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Backbone テンプレート プロジェクトを作成します。

ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。 テンプレートは、Visual Studio Extension (VSIX) ファイルとしてパッケージ化されます。 Visual Studio を再起動する必要があります。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。 プロジェクトの名前し、クリックして**OK**します。

![](backbonejs-template/_static/image1.png)

**新しいプロジェクト**Backbone.js SPA プロジェクトのウィザードを選択します。

![](backbonejs-template/_static/image2.png)

ビルド、デバッグを行わずにアプリケーションを実行して ctrl + f5 を押すか、f5 キーを押してデバッグを実行します。

![](backbonejs-template/_static/image3.png)

[アカウント] をクリックすると、ログイン ページが表示されます。

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>チュートリアル: クライアント コード

それでは、クライアント側から始まります。 クライアント アプリケーションのスクリプトは、~/Scripts/application フォルダーに配置されます。 アプリケーションが記述された[TypeScript](http://www.typescriptlang.org/) (.ts ファイル) は、JavaScript (.js ファイル) にコンパイルします。

**アプリケーション**

`Application` application.ts で定義されます。 このオブジェクトは、アプリケーションを初期化し、ルート名前空間として機能します。 ユーザーがサインインしているかどうかなど、アプリケーションで共有されている構成と状態の情報を保持します。

`application.start`メソッドは、モーダル ビューを作成し、ユーザーのサインインなどのアプリケーション レベルのイベントのイベント ハンドラーをアタッチします。 次に、既定のルーターを作成し、任意のクライアント側の URL が指定されているかどうかを確認します。 既定の url にリダイレクトそうでない場合 (#!/)。

**イベント**

イベントは、開発を疎結合コンポーネントときに常に重要です。 アプリケーションは、多くの場合、ユーザー アクションへの応答で複数の操作を実行します。 バックボーンでは、モデル、コレクション、およびビューなどのコンポーネントと組み込みのイベントを提供します。 これらのコンポーネント間の間の依存関係を作成する代わりに、テンプレートは「パブリッシュ/サブスクライブ」モデルを使用: `events` events.ts で定義されているオブジェクトが公開して、アプリケーションのイベントにサブスクライブするため、イベント ハブとして機能します。 `events`オブジェクトはシングルトンです。 次のコードでは、イベントにサブスクライブして、イベントをトリガーする方法を示します。

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**ルーター**

Backbone.js では、ルーターは、クライアント側のページのルーティングとアクションおよびイベントへの接続のためのメソッドを提供します。 テンプレートは、router.ts の 1 つのルーターを定義します。 ルーターは activable ビューを作成し、ビューを切り替えるときに状態を保持します。 (Activable ビューは、次のセクションで説明します)。プロジェクトの 2 つのダミー ビューが最初に、ホームおよびします。 ルートが不明である場合に表示される NotFound ビューもあります。

**ビュー**

ビューは、~/Scripts/application/ビューで定義されます。 ビュー、activable ビューとビューのモーダル ダイアログの 2 種類があります。 Activable ビューは、ルーターによって呼び出されます。 Activable のビューが表示されるとその他のすべての activable ビューは非アクティブになります。 Activable のビューを作成すると、ビューを拡張、`Activable`オブジェクト。

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

による拡張`Activable` ビューに 2 つの新しいメソッドを追加します。`activate`と`deactivate`します。 ルーターをアクティブ化して、ビューを非アクティブのこれらのメソッドを呼び出します。

モーダル ビューとして実装[Twitter Bootstrap](http://twitter.github.com/bootstrap/)モーダル ダイアログ。 `Membership`と`Profile`ビューは、モーダル ビュー。 モデル、ビュー、アプリケーション イベントを呼び出すことができます。 たとえば、 `Navigation` 「個人用アカウント」のリンクをクリックして、ビューでは、いずれかを示します、`Membership`ビューまたは`Profile`によっては、ユーザーがログインしているかどうかのビュー。 `Navigation`アタッチされているすべての子要素にイベント ハンドラーをクリックして、`data-command`属性。 HTML マークアップを次に示します。

[!code-html[Main](backbonejs-template/samples/sample3.html)]

ここで、イベントをフックする navigation.ts でコードを示します。

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**モデル**

モデルは、~/Scripts/application/モデルで定義されます。 すべてのモデルが 3 つの基本的な事項がある: 既定の属性、検証規則、およびサーバー側のエンドポイント。 典型的な例を次に示します。

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**プラグイン**

~/Scripts/application/lib フォルダーには、いくつかの便利な jQuery プラグインが含まれています。Form.ts ファイルは、フォーム データを操作するためのプラグインを定義します。 多くの場合は、シリアル化またはフォームのデータを逆シリアル化し、モデル検証エラーを表示する必要があります。 Form.ts プラグインなどの方法があります`serializeFields`、 `deserializeFields`、および`showFieldErrors`します。 次の例では、モデルにフォームをシリアル化する方法を示します。

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Flashbar.ts プラグインでは、さまざまな種類のフィードバック メッセージをユーザーに与えられます。 メソッドは、 `$.showSuccessbar`、`$.showErrorbar`と`$.showInfobar`します。 バック グラウンドでは、適切にアニメーション化されたメッセージを表示するのに Twitter Bootstrap のアラートを使用します。

プラグインの confirm.ts 置き換えますブラウザーの API では若干異なりますが、ダイアログ ボックスを確認します。

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>チュートリアル: サーバー コード

これで、サーバー側を見てみましょう。

**コントローラー**

シングル ページ アプリケーションでは、サーバーは、ユーザー インターフェイス内の小さなロールのみを再生します。 通常、サーバー、初期ページをレンダリングおよび送信し、JSON データを受信します。

テンプレートが 2 つの MVC コント ローラー: `HomeController` 、最初のページをレンダリングし、`SupportsController`新しいユーザー アカウントを確認し、パスワードをリセットするために使用します。 テンプレートの他のすべてのコント ローラーは、JSON データを送受信する ASP.NET Web API コント ローラーです。 既定では、コント ローラーを使用して、新しい`WebSecurity`をユーザーに関連するタスクを実行するクラス。 ただし、これらのタスクのデリゲートに渡すことができますを省略可能なコンス トラクターもあります。 これにより、テストを簡単になり、置き換えることができます`WebSecurity`IoC コンテナーを使用して、他のものとします。 次に例を示します。

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Views

ビューがモジュールにデザインされています。 ページの各セクションでは、独自の専用のビュー。 シングル ページ アプリケーションでは、共通する任意の対応するコント ローラーがないビューが含まれます。 呼び出すことによって、ビューを含めることができます`@Html.Partial('myView')`、面倒でこれを取得します。 テンプレートは、簡単に、ヘルパー メソッドを定義します`IncludeClientViews`、すべての指定したフォルダーにビューをレンダリングします。

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

フォルダー名が指定されていない場合、既定のフォルダー名は"ClientViews"にします。 名前をアンダー スコア文字の部分ビューのも、クライアントの表示は、部分ビューを使用する場合 (たとえば、 `_SignUp`)。 `IncludeClientViews`メソッドは、アンダー スコアで始まる名前を持つすべてのビューを除外します。 部分ビューをクライアント ビューに含めるには、呼び出す`Html.ClientView('SignUp')`の代わりに`Html.Partial('_SignUp')`します。

**電子メールの送信**

テンプレートを使用して電子メールを送信、[郵便](http://aboutcode.net/postal)します。 ただし、郵便番号を使用してコードの残りの部分から抽象化、`IMailer`インターフェイスのため、別の実装を簡単に置き換えることができます。 電子メール テンプレートは、ビュー、メール フォルダーに配置されます。 送信者の電子メール アドレスがで web.config ファイルで指定された、`sender.email`のキー、 **appSettings**セクション。 また、 `debug="true"` 、web.config ファイル、アプリケーションを必要としない開発を高速化、ユーザーの電子メール確認します。

## <a name="github"></a>GitHub

Backbone.js SPA テンプレートを検索することもできます。 [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa)します。
