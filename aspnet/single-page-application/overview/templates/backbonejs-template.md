---
uid: single-page-application/overview/templates/backbonejs-template
title: "Backbone テンプレート |Microsoft ドキュメント"
author: madskristensen
description: "Backbone.js SPA テンプレート"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="backbone-template"></a>Backbone テンプレート
====================
によって[Mads Kristensen](https://github.com/madskristensen)

> Kazi Manzur Rashid によってバックボーン SPA テンプレートが書き込まれました
> 
> [テンプレートをダウンロードして Backbone.js SPA](https://go.microsoft.com/fwlink/?LinkId=293631)


Backbone.js SPA テンプレートを使用して対話型のクライアント側 web アプリの構築をすばやく開始するように設計された[Backbone.js です。](http://backbonejs.org/)

テンプレートは、ASP.NET MVC で Backbone.js アプリケーションの開発の初期のスケルトンを提供します。 すぐに、ユーザーのサインアップ、サインイン、パスワードのリセットと基本的な電子メール テンプレートを使用してユーザーの確認など、基本的なユーザー ログインの機能を提供します。

要件:

- [ASP.NET および Web ツール 2012.2 の更新](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Backbone テンプレート プロジェクトを作成します。

ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。 テンプレートは、Visual Studio Extension (VSIX) ファイルとしてパッケージされています。 Visual Studio を再起動する必要があります。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#**ノード。 **Visual c#** **Web**です。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。 プロジェクトの名前を指定し、をクリックして**OK**です。

![](backbonejs-template/_static/image1.png)

**新しいプロジェクト**Backbone.js SPA プロジェクトのウィザードを選択します。

![](backbonejs-template/_static/image2.png)

Ctrl キーを押し、f5 キーを押してビルドおよびデバッグせずにアプリケーションを実行するか、f5 キーを押してデバッグを実行します。

![](backbonejs-template/_static/image3.png)

「マイ アカウント」をクリックするとは、ログイン ページを表示します。

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>チュートリアル: クライアント コード

クライアント側で始まてみましょう。 クライアント アプリケーションのスクリプトは ~/Scripts/application フォルダーにあります。 アプリケーションが記述された[TypeScript](http://www.typescriptlang.org/) (.ts ファイル) は、JavaScript (.js ファイル) にコンパイルします。

**アプリケーション**

`Application`application.ts で定義されます。 このオブジェクトは、アプリケーションを初期化し、ルート名前空間として機能します。 ユーザーがサインインしているかどうかなど、アプリケーション全体で共有されている構成と状態の情報を保持します。

`application.start`メソッドは、モーダル ビューを作成し、ユーザー サインインなど、アプリケーション レベルのイベントのイベント ハンドラーをアタッチします。 次に、既定のルーターを作成し、任意のクライアント側の URL が指定されているかどうかを確認します。 既定の url にリダイレクトされていない場合 (#!/)。

**イベント**

イベントは、コンポーネント開発疎結合されたときに常に重要です。 アプリケーションは、多くの場合、ユーザーの操作への応答で複数の操作を実行します。 Backbone は、モデル、コレクション、およびビューなどのコンポーネントでの組み込みのイベントを提供します。 これらのコンポーネント間の間の依存関係を作成するには、代わりに、テンプレートは「パブリッシュ/サブスクライブ」モデルを使用: `events` events.ts で定義されている、オブジェクトの公開とアプリケーションのイベントにサブスクライブするため、イベント ハブとして機能します。 `events`オブジェクトがシングルトンです。 次のコードでは、イベントにサブスクライブし、イベントをトリガーする方法を示します。

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**ルーター**

Backbone.js では、ルーターは、クライアント側のページのルーティングとアクションとイベントへの接続のためのメソッドを提供します。 テンプレートは、router.ts で 1 台のルーターを定義します。 ルーターは、activable ビューを作成し、ビューを切り替えるときに状態を維持します。 (Activable ビューは 次のセクションで説明します)。最初に、プロジェクトでは 2 つのダミー ビュー、およびホームです。 ルートが不明の場合に表示される NotFound ビューもあります。

**ビュー**

ビューは、~/Scripts/application/ビューで定義されます。 ビュー、activable ビューおよびモーダル ダイアログ ビューの 2 種類があります。 Activable ビューは、ルーターによって呼び出されます。 Activable ビューが表示されるとその他のすべての activable ビューは非アクティブになります。 Activable ビューを作成するビューを拡張、`Activable`オブジェクト。

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

による拡張`Activable` ビューに 2 つの新しいメソッドを追加`activate`と`deactivate`です。 ルーターは、これらのメソッドをアクティブにして、ビューを非アクティブ化を呼び出します。

モーダル ビューとして実装されて[Twitter のブートス トラップ](http://twitter.github.com/bootstrap/)モーダル ダイアログ ボックス。 `Membership`と`Profile`ビューは、モーダル ビュー。 モデル ビューは、アプリケーション イベントによって呼び出すことができます。 たとえば、 `Navigation` 「マイ アカウント」のリンクをクリックして、ビューを表示するか、`Membership`ビューまたは`Profile`ユーザーがログインしているかどうかに応じて、表示します。 `Navigation`アタッチ クリックしてイベント ハンドラーを持つすべての子要素、`data-command`属性。 HTML マークアップを次に示します。

[!code-html[Main](backbonejs-template/samples/sample3.html)]

次に、イベントをフック navigation.ts でコードを示します。

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**モデル**

モデルは、~/Scripts/application/モデルで定義されます。 すべてのモデルは、次の 3 つの基本的な項目をある: 既定の属性、検証規則、およびサーバー側のエンド ポイント。 一般的な使用例を次に示します。

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**プラグイン**

~/Scripts/application/lib フォルダーには、いくつかの便利な jQuery プラグインが含まれています。Form.ts ファイルは、フォームのデータを操作するためのプラグインを定義します。 多くの場合は、シリアル化またはフォームのデータを逆シリアル化し、モデルの検証エラーを表示する必要があります。 プラグインの form.ts などのメソッドがあります`serializeFields`、 `deserializeFields`、および`showFieldErrors`です。 次の例では、モデルにフォームをシリアル化する方法を示します。

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Flashbar.ts プラグインでは、さまざまな種類のフィードバック メッセージをユーザーに与えられます。 メソッドは、 `$.showSuccessbar`、`$.showErrorbar`と`$.showInfobar`です。 背後では、適切にアニメーション化されたメッセージを表示するのにブートス トラップの Twitter のアラートを使用します。

プラグインの confirm.ts 置換ブラウザーの API では若干異なりますが、ダイアログ ボックスを確認します。

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>チュートリアル: サーバー コード

これで、サーバー側を見てみましょう。

**コントローラー**

単一ページ アプリケーションでは、サーバーは、ユーザー インターフェイス内の小さなロールのみを再生します。 通常、サーバーの初期ページを表示および送信し、JSON データを受信します。

テンプレートが 2 つの MVC コント ローラー:`HomeController`最初のページを表示および`SupportsController`を新しいユーザー アカウントを確認し、パスワードをリセットするために使用します。 テンプレートの他のすべてのコント ローラーは、JSON データを送受信する ASP.NET Web API コント ローラーです。 既定では、コント ローラーを使用して、新しい`WebSecurity`をユーザーに関連するタスクを実行するクラス。 ただし、それらも省略可能なコンス トラクターを持つこれらのタスクのデリゲートに渡すことが許可します。 これにより、テストを容易になり、置き換えることができます`WebSecurity`IoC コンテナーを使用して、他のものとします。 次に例を示します。

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>ビュー

ビューはモジュール形式であるように設計されています。 ページの各セクションでは、独自の専用のビューです。 単一ページ アプリケーションに共通する任意の対応するコント ローラーを持たないビューを含めるは。 呼び出して、ビューを含めることができます`@Html.Partial('myView')`、面倒でこれを取得します。 テンプレートが、ヘルパー メソッドを定義しやすくにするには、 `IncludeClientViews`、すべての指定したフォルダーにビューを表示します。

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

フォルダー名が指定されていない場合、既定のフォルダー名は"ClientViews"がします。 クライアントの表示は、部分ビューを使用しても、アンダー スコア文字の部分ビューの名前を付けます (たとえば、 `_SignUp`)。 `IncludeClientViews`メソッドは、アンダー スコアで始まる名前を持つすべてのビューを除外します。 ビューに含める部分ビュー、クライアント、呼び出す`Html.ClientView('SignUp')`の代わりに`Html.Partial('_SignUp')`です。

**電子メールを送信します。**

テンプレートを使用して電子メールを送信するには、[郵便](http://aboutcode.net/postal)です。 ただし、郵便を使用してコードの残りの部分から切り離されます、`IMailer`インターフェイスのため、別の実装と簡単に置き換えることができます。 電子メール テンプレートは、ビューや電子メール フォルダーにあります。 送信者の電子メール アドレスが web.config ファイル内で指定された、`sender.email`のキー、 **appSettings**セクションです。 また、 `debug="true"` 、web.config ファイル、アプリケーションを必要としない開発を高速化するのには、ユーザーは電子メール確認します。

## <a name="github"></a>GitHub

Backbone.js SPA テンプレートを検索することも[GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa)です。
