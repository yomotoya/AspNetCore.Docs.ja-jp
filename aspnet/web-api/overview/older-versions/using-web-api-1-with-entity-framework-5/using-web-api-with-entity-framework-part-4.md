---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'パート 4: 管理ビューを追加する |Microsoft ドキュメント'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879531"
---
<a name="part-4-adding-an-admin-view"></a>パート 4: 管理ビューを追加します。
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>管理ビューを追加します。

今すぐおがクライアント側に有効にするし、管理コント ローラーからデータを使用できるページを追加します。 ページは作成、編集、またはコント ローラーに AJAX 要求を送信することによって、製品を削除するようにします。

ソリューション エクスプ ローラーで Controllers フォルダーを展開し、HomeController.cs をという名前のファイルを開きます。 このファイルには、ある MVC コント ローラーが含まれています。 という名前のメソッドを追加`Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl**メソッドは、web API への URI を作成しこれに格納ビュー バッグ後でします。

次に、カーソルの位置のテキスト内で、`Admin`アクション メソッド、し、右クリックして選択**ビューの追加**です。 これが表示されます、**ビューの追加**ダイアログ。

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

**ビューの追加**ダイアログ ボックスで、名前、ビュー"Admin"です。 チェック ボックスをオン**厳密に型指定されたビューを作成する**です。 **モデル クラス**、"Product (ProductStore.Models)"を選択します。 既定値として、その他のすべてのオプションのままにします。

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

クリックすると**追加**ビュー/ホーム Admin.cshtml をという名前のファイルを追加します。 このファイルを開き、次の HTML を追加します。 この HTML ページの構造を定義するが、機能は、ワイヤード クライアントをまだです。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>[管理] ページへのリンクを作成します。

ソリューション エクスプ ローラーで、Views フォルダーを展開し、共有フォルダーを展開します。 という名前のファイルを開く\_Layout.cshtml です。 検索、 **ul** id を持つ要素 =「メニュー」とは、管理ビューのアクション リンク。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> サンプル プロジェクトで変更したいくつか他の表面的な「ここにロゴ」の文字列を置き換えることなどです。 これらのアプリケーションの機能は影響しません。 プロジェクトをダウンロードして、ファイルを比較することができます。


アプリケーションを実行し、ホーム ページの上部に表示される"Admin"リンクをクリックします。 [管理] ページは、次のようになります。

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

右側のようになりましたが、ページ処理しません。 次のセクションで Knockout.js ダイナミック UI の作成に使用されます。

## <a name="add-authorization"></a>承認を追加します。

[管理] ページがサイトにアクセスするすべてのユーザーに現在アクセスできません。 管理者に権限を制限するこれを変更してみましょう。

"Administrator"ロールと管理者ユーザーを追加することで開始します。 ソリューション エクスプ ローラーで、フィルター フォルダーを展開し、InitializeSimpleMembershipAttribute.cs をという名前のファイルを開きます。 検索、`SimpleMembershipInitializer`コンス トラクターです。 呼び出し後**WebSecurity.InitializeDatabaseConnection**、次のコードを追加します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

これは、"Administrator"ロールを追加し、ロールのユーザーを作成する簡便な方法です。

ソリューション エクスプローラで、Controllers フォルダーを展開および HomeController.cs ファイルを開きます。 追加、 **Authorize**属性を`Admin`メソッドです。

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

AdminController.cs ファイルを開き、追加、 **Authorize**属性全体を`AdminController`クラスです。

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC と Web API の両方を定義する**Authorize**異なる名前空間内の属性です。 MVC を使用して**System.Web.Mvc.AuthorizeAttribute**Web API を使用しますが、 **System.Web.Http.AuthorizeAttribute**です。


今すぐ管理者だけでは、[管理] ページを表示できます。 また、管理コント ローラーに HTTP 要求を送信する場合、要求は認証クッキーを含める必要があります。 それ以外の場合は、サーバーは HTTP 401 (未承認) 応答を送信します。 このわかります Fiddler でへの GET 要求を送信することによって`http://localhost:*port*/api/admin`です。

> [!div class="step-by-step"]
> [前へ](using-web-api-with-entity-framework-part-3.md)
> [次へ](using-web-api-with-entity-framework-part-5.md)
