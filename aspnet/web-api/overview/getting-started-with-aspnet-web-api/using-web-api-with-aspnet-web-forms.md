---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: ASP.NET Web フォームで Web API の使用 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/01/2017
ms.locfileid: "26536081"
---
<a name="using-web-api-with-aspnet-web-forms"></a>ASP.NET Web フォームで Web API の使用
====================
によって[Mike Wasson](https://github.com/MikeWasson)

ASP.NET Web API は、ASP.NET MVC にパッケージ化されては簡単に Web API を従来の ASP.NET Web フォーム アプリケーションに追加します。 このチュートリアルでは、手順を示します。

## <a name="overview"></a>概要

を Web フォーム アプリケーションで Web API を使用するのには、次の 2 つの主要な手順があります。

- 派生する Web API コント ローラーを追加、 **ApiController**クラスです。
- ルート テーブルを追加、**アプリケーション\_開始**メソッドです。

## <a name="create-a-web-forms-project"></a>Web フォーム プロジェクトを作成します。

Visual Studio を起動し、選択**新しいプロジェクト**から、**開始**ページ。 またはから、**ファイル**メニューの **新規**し**プロジェクト**です。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#** **Web**です。 プロジェクト テンプレートの一覧で選択**ASP.NET Web フォーム アプリケーション**です。 プロジェクトの名前を入力し、クリックして**OK**です。

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>モデルとコント ローラーを作成します。

このチュートリアルは、同じモデルとコント ローラーとしてのクラスを使用して、[作業の開始](tutorial-your-first-web-api.md)チュートリアルです。

最初に、モデル クラスを追加します。 **ソリューション エクスプ ローラー**、プロジェクトを右クリックし **クラスの追加**です。 クラスの製品の名前を次の実装を追加します。

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

次に、Web API コント ローラーを追加、プロジェクトに、A*コント ローラー* Web API の HTTP 要求を処理するオブジェクトです。

**ソリューション エクスプ ローラー**プロジェクトを右クリックします。 選択**新しい項目の追加**です。

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

**インストールされたテンプレート**、展開**Visual c#** 選択**Web**です。 次に、テンプレートの一覧から次のように選択します。 **Web API コント ローラー クラス**です。 コント ローラー"ProductsController"という名前をクリックして**追加**です。

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**新しい項目の追加**ProductsController.cs をという名前のファイルを作成します。 ウィザードが含まれているメソッドを削除し、次のメソッドを追加します。

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

このコント ローラーでコードの詳細については、次を参照してください。、[作業の開始](tutorial-your-first-web-api.md)チュートリアルです。

## <a name="add-routing-information"></a>ルーティング情報を追加します。

次に、追加 URI ルートのため、フォームの Uri &quot;/api製品/&quot;コント ローラーにルーティングされます。

**ソリューション エクスプ ローラー**、Global.asax Global.asax.cs 分離コード ファイルを開く をダブルクリックします。 次の追加**を使用して**ステートメントです。

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

次のコードを追加、**アプリケーション\_開始**メソッド。

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

ルーティング テーブルの詳細については、次を参照してください。 [ASP.NET Web API でルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)です。

## <a name="add-client-side-ajax"></a>クライアント側の AJAX を追加します。

Web クライアントがアクセスできる API を作成する必要がありますすべてです。 これで、API の呼び出しに jQuery を使用する HTML ページを追加してみましょう。

マスター ページを確認してください (たとえば、 *Site.Master*) が含まれています、`ContentPlaceHolder`で`ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Default.aspx ファイルを開きます。 ように、メイン コンテンツのセクションでは、定型のテキストを置き換えます。

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

次に、jQuery ソース ファイルへの参照を追加、`HeaderContent`セクション。

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

注: 簡単に、スクリプト参照を追加できますをドラッグ アンド ドロップ ファイルから**ソリューション エクスプ ローラー**コード エディター ウィンドウにします。

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

JQuery スクリプト タグの下には、次のスクリプト ブロックを追加します。

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

このスクリプトで AJAX 要求は、ドキュメントが読み込まれると、 &quot;api/製品&quot;です。 要求は、JSON 形式での製品の一覧を返します。 スクリプトは、製品情報を HTML テーブルに追加します。

アプリケーションを実行するときに次のようになります。

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
