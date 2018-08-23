---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'パート 4: 管理ビューの追加 |Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: ff46c7cbdf13a048e57ad88ab39077357e35c5b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828910"
---
<a name="part-4-adding-an-admin-view"></a>パート 4: 管理ビューの追加
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>管理ビューを追加します。

ここで、クライアント側を有効にするし、管理コント ローラーからデータを使用できるページを追加します。 ページには、作成、編集、またはコント ローラーに AJAX 要求を送信することによって、製品を削除するユーザーはできます。

ソリューション エクスプ ローラーでは、Controllers フォルダーを展開し、HomeController.cs をという名前のファイルを開きます。 このファイルには、MVC コント ローラーが含まれています。 という名前のメソッドを追加`Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

**HttpRouteUrl**メソッドは、web API への URI を作成し、後でビュー バッグに保存すればです。

次に、テキスト カーソルを置いて、`Admin`アクション メソッドでは、し、右クリックして選択**ビューの追加**します。 これが表示されます、**ビューの追加**ダイアログ。

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

**ビューの追加**ダイアログ ボックスで、"Admin"ビューの名前。 チェック ボックスをオン**厳密に型指定されたビューを作成する**します。 **モデル クラス**、"Product (ProductStore.Models)"を選択します。 既定値として、その他のすべてのオプションのままにします。

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

クリックすると**追加**ビュー/ホーム Admin.cshtml という名前のファイルを追加します。 このファイルを開き、次の HTML を追加します。 この HTML ページの構造を定義しますが、機能は、ワイヤード クライアントをまだ。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>[管理] ページへのリンクを作成します。

ソリューション エクスプ ローラーで Views フォルダーを展開し、共有フォルダーを展開します。 という名前のファイルを開く\_Layout.cshtml します。 検索、 **ul** id を持つ要素 =「メニュー」、および管理ビューのアクション リンク。

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> サンプル プロジェクトでは、いくつかその他の表面的な変更、"Your logo here"という文字列の置換などをしました。 これらのアプリケーションの機能は影響はありません。 プロジェクトをダウンロードして、ファイルを比較することができます。


アプリケーションを実行し、ホーム ページの上部に表示される"Admin"リンクをクリックします。 管理者 ページは、次のようになります。

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

右に、ページは何も行いません。 次のセクションで Knockout.js 動的 UI の作成に使用します。

## <a name="add-authorization"></a>承認を追加します。

管理者 ページがサイトにアクセスするすべてのユーザーに現在アクセスできません。 管理者のアクセス許可を制限するこれを変更してみましょう。

まず、"Administrator"ロールと管理者ユーザーを追加します。 ソリューション エクスプ ローラーでは、フィルター フォルダーを展開し、InitializeSimpleMembershipAttribute.cs という名前のファイルを開きます。 検索、`SimpleMembershipInitializer`コンス トラクター。 呼び出し後**WebSecurity.InitializeDatabaseConnection**、次のコードを追加します。

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

これは、簡便"Administrator"ロールを追加し、ロールのユーザーを作成する方法です。

ソリューション エクスプ ローラーでは、Controllers フォルダーを展開し、HomeController.cs ファイルを開きます。 追加、 **Authorize**属性を`Admin`メソッド。

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

AdminController.cs ファイルを開き、追加、 **Authorize**全体に属性`AdminController`クラス。

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC と Web API の両方を定義**Authorize**異なる名前空間内の属性。 MVC を使用して**System.Web.Mvc.AuthorizeAttribute**Web API を使用しますが、 **System.Web.Http.AuthorizeAttribute**します。


今すぐ管理者のみでは、管理者 ページを表示できます。 また、管理コント ローラーに HTTP 要求を送信する場合、要求は認証クッキーを含める必要があります。 それ以外の場合は、サーバーが HTTP 401 (未承認) 応答を送信します。 これでわかる Fiddler に GET 要求を送信することによって`http://localhost:*port*/api/admin`します。

> [!div class="step-by-step"]
> [前へ](using-web-api-with-entity-framework-part-3.md)
> [次へ](using-web-api-with-entity-framework-part-5.md)
