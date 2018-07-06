---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: ASP.NET Web フォームによる Web API の使用 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 04/03/2012
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: c9da38d290a812e94ed9473386ba6b897447dac5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840889"
---
<a name="using-web-api-with-aspnet-web-forms"></a>ASP.NET Web フォームによる Web API の使用
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

ASP.NET Web API は、ASP.NET MVC にパッケージ化されてが簡単に Web API を従来の ASP.NET Web フォーム アプリケーションに追加できます。 このチュートリアルの手順について説明します。

## <a name="overview"></a>概要

Web フォーム アプリケーションで Web API を使用するには、2 つの主な手順があります。

- 派生した Web API コント ローラーを追加、 **ApiController**クラス。
- ルート テーブルを追加、**アプリケーション\_開始**メソッド。

## <a name="create-a-web-forms-project"></a>Web フォーム プロジェクトを作成します。

Visual Studio を起動し、選択**新しいプロジェクト**から、**開始**ページ。 またはから、**ファイル**メニューの **新規**し**プロジェクト**します。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET Web フォーム アプリケーション**します。 プロジェクトの名前を入力し、クリックして**OK**します。

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>モデルとコント ローラーを作成します。

このチュートリアルと同じモデルとコント ローラー クラスを使用して、 [Getting Started](tutorial-your-first-web-api.md)チュートリアル。

最初に、モデル クラスを追加します。 **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**クラスの追加**します。 製品、クラスの名前を次の実装を追加します。

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

次に、Web API コント ローラーを追加、プロジェクトに、A*コント ローラー*は Web API の HTTP 要求を処理するオブジェクトです。

**ソリューション エクスプ ローラー**プロジェクトを右クリックします。 選択**新しい項目の追加**します。

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

**インストールされたテンプレート**、展開**Visual c#** 選択**Web**します。 次に、テンプレートの一覧から次のように選択します。 **Web API コント ローラー クラス**します。 コント ローラーの名前を"ProductsController"にして**追加**します。

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**新しい項目の追加**ProductsController.cs という名前のファイルを作成します。 ウィザードが含まれているメソッドを削除し、次のメソッドを追加します。

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

このコント ローラー コードの詳細については、次を参照してください。、 [Getting Started](tutorial-your-first-web-api.md)チュートリアル。

## <a name="add-routing-information"></a>ルーティング情報を追加します。

次に、追加します URI ルートのため形式の Uri &quot;/api/製品/&quot;コント ローラーにルーティングされます。

**ソリューション エクスプ ローラー**、Global.asax Global.asax.cs の分離コード ファイルを開く をダブルクリックします。 次の追加**を使用して**ステートメント。

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

次のコードを追加し、**アプリケーション\_開始**メソッド。

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

ルーティング テーブルの詳細については、次を参照してください。 [ASP.NET Web API におけるルーティング](../web-api-routing-and-actions/routing-in-aspnet-web-api.md)します。

## <a name="add-client-side-ajax"></a>クライアント側 AJAX を追加します。

Web クライアントにアクセスできる API を作成する必要があるだけです。 これで、API の呼び出しに jQuery を使用する HTML ページを追加してみましょう。

マスター ページを確認します (たとえば、 *Site.Master*) が含まれています、`ContentPlaceHolder`で`ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Default.aspx ファイルを開きます。 示すように、メイン コンテンツのセクションでは、定型テキストを置き換えます。

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

次に、jQuery のソース ファイルへの参照を追加、`HeaderContent`セクション。

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

注: できます簡単に追加するスクリプト参照をドラッグ アンド ドロップ ファイルから**ソリューション エクスプ ローラー**がコード エディター ウィンドウにします。

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

JQuery スクリプト タグでは、下には、次のスクリプト ブロックを追加します。

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

このスクリプトで、AJAX 要求には、ドキュメントが読み込まれると、 &quot;api/製品&quot;します。 要求は、JSON 形式で製品の一覧を返します。 スクリプトでは、HTML テーブルに製品情報を追加します。

アプリケーションを実行するようになります。

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
