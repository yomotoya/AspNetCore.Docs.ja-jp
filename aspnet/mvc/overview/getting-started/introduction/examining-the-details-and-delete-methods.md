---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: 詳細とその削除方法を確認する |Microsoft ドキュメント
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: f534080fe9aa22eb9092932babc74c5ab96aabbf
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
<a name="examining-the-details-and-delete-methods"></a>詳細とその削除方法を確認します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このチュートリアルでは、を確認する、自動的に生成された`Details`と`Delete`メソッドです。

## <a name="examining-the-details-and-delete-methods"></a>詳細とその削除方法を確認します。

開く、`Movie`コント ローラーを確認し、`Details`メソッドです。

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

このアクション メソッドを作成する MVC のスキャフォールディング エンジンは、メソッドを呼び出すための HTTP 要求を示すコメントを追加します。 ここでは、 `GET` 3 つの URL セグメントを持つ要求、`Movies`コント ローラー、`Details`メソッドおよび`ID`値。

コード最初に簡単にデータを使用して検索、`Find`メソッドです。 メソッドに組み込まれている重要なセキュリティ機能は、コードがあることを確認する、`Find`メソッドは、コードがそれには何も操作する前に、ムービーが見つかりました。 たとえば、ハッカーでしたにエラーが発生、サイトからのリンクによって作成された URL を変更することによって`http://localhost:xxxx/Movies/Details/1`のようなものに`http://localhost:xxxx/Movies/Details/12345`(または実際のムービーを表しません。 その他の値)。 Null のムービーのチェックしなかった場合、null のムービーは、データベース エラーになります。

`Delete` メソッドと `DeleteConfirmed` メソッドを調べます。

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

HTTP get`Delete`メソッドは、指定したビデオを削除しないを送信することができます、ムービーのビューを返します (`HttpPost`) 削除します。 GET 要求の応答で削除操作を実行すると (さらに言えば、編集操作、作成操作、データを変更するその他のあらゆる操作を実行すると)、セキュリティに穴が空きます。 詳細については、Stephen Walther のブログ記事を参照してください。 [ASP.NET MVC ヒント #46-セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)です。

データを削除する `HttpPost` メソッドの名前は「`DeleteConfirmed`」になり、HTTP POST メソッドに一意のシグネチャまたは名前が与えられます。 2 つのメソッド シグネチャは下の画像のようになります。

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

共通言語ランタイム (CLR) は、オーバーロードのメソッドに一意のパラメーター シグネチャを持つことを要求します (メソッド名は同じであるが、パラメーターの一覧が異なる)。 ただし、ここで必要が 2 つの削除のメソッドと POST の 1 つ--GET--同じパラメーター シグネチャを持つ両方ことです。 (いずれも、1 つの整数をパラメーターとして受け取る必要があります。)

この出力を並べ替えるには、いくつかの点を行うことができます。 別の名前を指定してメソッドを 1 つです。 先の例では、スキャフォールディング メカニズムがこれを行いました。 ただし、これは小さな問題を引き起こします。ASP.NET が URL のセグメントをアクション メソッドに名前でマッピングします。メソッドの名前を変更すると、通常、ルーティングでそのメソッドが見つからなくなります。 この解決策はこの例で確認できます。`ActionName("Delete")` 属性を `DeleteConfirmed` メソッドに追加しています。 これが効果的に実行マッピング、ルーティングのシステムの URL を含むように */Delete/* post 要求を検索は、`DeleteConfirmed`メソッドです。

同じ名前とシグネチャを持つメソッドの問題を回避するもう 1 つの一般的な方法を使用すると意図的に未使用のパラメーターを含める POST メソッドのシグネチャを変更します。 一部の開発者がパラメーターの型を追加するなど、 `FormCollection` POST メソッドに渡されると、単にパラメーターを使用しません。

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>まとめ

DB のローカル データベースにデータを格納する完全な ASP.NET MVC アプリケーションがあるようになりました。 ことができますを作成、読み取り、更新、削除、および映画を検索します。

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>次の手順

ビルドして、web アプリケーションをテストした後、次の手順は、インターネット経由で使用するには、他の人に使用できるようにするは。 実行するには、web ホスティング プロバイダーに配置する必要です。 マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、[無料試用版の Azure アカウント](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)です。 次に チュートリアルを実行することをお勧め[メンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET MVC アプリケーションを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。 優れたチュートリアルは、Tom Dykstra の中間レベル[、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 [Stackoverflow](http://stackoverflow.com/help)と[ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)質問を投稿する優れたが配置されます。 次の[me](https://twitter.com/RickAndMSFT)ツイッター 最新のチュートリアルで更新プログラムを取得できるようにします。

フィードバックはへようこそ です。

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [前へ](adding-validation.md)
