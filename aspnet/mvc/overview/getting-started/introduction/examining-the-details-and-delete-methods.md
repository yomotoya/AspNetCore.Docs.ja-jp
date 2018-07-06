---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Details メソッドと Delete メソッドを調べること |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: ce4413f81e49c1467b574347f69f855ae4ba5bf8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839048"
---
<a name="examining-the-details-and-delete-methods"></a>Details メソッドと Delete メソッドを調べる
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

このチュートリアルでは、自動的に生成された調べます`Details`と`Delete`メソッド。

## <a name="examining-the-details-and-delete-methods"></a>Details メソッドと Delete メソッドを調べる

開く、`Movie`コント ローラーを調べ、`Details`メソッド。

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

このアクション メソッドを作成した MVC スキャフォールディング エンジンは、メソッドを呼び出す HTTP 要求を示すコメントを追加します。 ここでは、 `GET` 3 つの URL セグメントで要求を`Movies`コント ローラー、`Details`メソッドと`ID`値。

コード最初に簡単にデータを使用して検索、`Find`メソッド。 メソッドに組み込まれている、重要なセキュリティ機能は、コードが確認します、`Find`メソッドは、コードが、それ以降を実行する前に、ムービーが検出されます。 たとえば、ハッカーでしたにエラーが発生、サイトからのリンクが作成する URL を変更することで`http://localhost:xxxx/Movies/Details/1`ようなものに`http://localhost:xxxx/Movies/Details/12345`(または実際のムービーを表していないその他のいくつかの値)。 Null のムービーを確認しなかった、null のムービーのデータベース エラーになります。

`Delete` メソッドと `DeleteConfirmed` メソッドを調べます。

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

HTTP get`Delete`メソッドは、指定されたビデオを削除しないを送信することができます、ムービーのビューを返します (`HttpPost`)、削除します。 GET 要求の応答で削除操作を実行すると (さらに言えば、編集操作、作成操作、データを変更するその他のあらゆる操作を実行すると)、セキュリティに穴が空きます。 詳細については、Stephen Walther のブログ記事を参照してください。 [ASP.NET MVC ヒントと 46: セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)します。

データを削除する `HttpPost` メソッドの名前は「`DeleteConfirmed`」になり、HTTP POST メソッドに一意のシグネチャまたは名前が与えられます。 2 つのメソッド シグネチャは下の画像のようになります。

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

共通言語ランタイム (CLR) は、オーバーロードのメソッドに一意のパラメーター シグネチャを持つことを要求します (メソッド名は同じであるが、パラメーターの一覧が異なる)。 ただし、ここで必要 2 つ削除メソッド--GET の 1 つ--、投稿の両方が同じパラメーター シグネチャを持つこと。 (いずれも、1 つの整数をパラメーターとして受け取る必要があります。)

この出力を並べ替えるには、いくつかの点を行うことができます。 メソッドの別の名前を付けて 1 つです。 先の例では、スキャフォールディング メカニズムがこれを行いました。 ただし、これは小さな問題を引き起こします。ASP.NET が URL のセグメントをアクション メソッドに名前でマッピングします。メソッドの名前を変更すると、通常、ルーティングでそのメソッドが見つからなくなります。 この解決策はこの例で確認できます。`ActionName("Delete")` 属性を `DeleteConfirmed` メソッドに追加しています。 これが効果的に実行マッピング、ルーティング システムの URL を含むように */delete/* 要求を検索する投稿について、`DeleteConfirmed`メソッド。

同じ名前とシグネチャを持つメソッドでの問題を回避するもう 1 つの一般的な方法では、意図的に未使用のパラメーターを含める POST メソッドのシグネチャを変更します。 たとえば、一部の開発者がパラメーターの型を追加`FormCollection`は POST メソッドに渡されると、単にパラメーターを使用しません。

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>まとめ

ローカル DB データベースにデータを格納する完全な ASP.NET MVC アプリケーションがあるようになりました。 ことができますを作成、読み取り、更新、削除、および映画を検索します。

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>次の手順

ビルドして、web アプリケーションをテストした後、次の手順は、インターネット経由で使用するには、他のユーザーが使用できるようにするは。 そのためには、web ホスティング プロバイダーにデプロイするがあります。 マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、[無料 Azure 試用版アカウント](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)します。 拙著のチュートリアルの次の利用をお勧めします[メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC アプリを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。 優れたチュートリアルは、Tom Dykstra の中間レベル[、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。 [Stackoverflow](http://stackoverflow.com/help)と[ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)は質問への優れたを配置します。 次の[me](https://twitter.com/RickAndMSFT) twitter で最新のチュートリアルでは、更新プログラムを取得できるようにします。

フィードバックを歓迎します。

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [前へ](adding-validation.md)
