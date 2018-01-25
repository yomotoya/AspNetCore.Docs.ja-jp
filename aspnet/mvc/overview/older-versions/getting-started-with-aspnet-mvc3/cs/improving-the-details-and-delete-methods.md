---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: "改善の詳細および Delete メソッド (c#) |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: e46616d45ad0e4a0ab861e6fb53f33bc567cbdea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="improving-the-details-and-delete-methods-c"></a>改善の詳細および Delete メソッド (c#)
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > このチュートリアルの更新バージョンが利用可能な[ここ](../../../getting-started/introduction/getting-started.md)ASP.NET MVC 5 と Visual Studio 2013 を使用します。 より安全な非常に簡単に従うしより多くの機能を示します。
> 
> 
> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。
> 
> C# ソース コードの Visual Web Developer プロジェクトは、このトピックを使用できます。 [C# バージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 Visual Basic を使用する場合に切り替えて、 [Visual Basic バージョン](../vb/intro-to-aspnet-mvc-3.md)このチュートリアルのです。


このチュートリアルでは、すれば、一部の機能強化、自動的に生成された`Details`と`Delete`メソッドです。 これらの変更が必要ではありませんが、コードのいくつかの小さなビット、アプリケーションを簡単に拡張できます。

## <a name="improving-the-details-and-delete-methods"></a>詳細とその削除方法の向上

スキャフォールディングすると、`Movie`コント ローラーで、ASP.NET MVC で生成されたコードをできるいくつかの小さな変更をより堅牢では非常によく作業をします。

開いている、`Movie`コント ローラーを変更し、`Details`メソッドを返すことによって`HttpNotFound`ムービーが見つからない。 変更することも必要があります、`Details`に渡された ID の既定値を設定します。 (同様に変更を加える、`Edit`メソッド[パート 6](examining-the-edit-methods-and-edit-view.md)このチュートリアルのです)。ただし、戻り値の型を変更する必要があります、`Details`メソッドから`ViewResult`に`ActionResult`であるため、`HttpNotFound`メソッドを返さない、`ViewResult`オブジェクト。 次の例は、変更された`Details`メソッドです。

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

コード最初に簡単にデータを使用して検索、`Find`メソッドです。 メソッドには組み込まれている重要なセキュリティ機能は、コードがあることを確認する、`Find`メソッドは、コードがそれには何も操作する前に、ムービーが見つかりました。 たとえば、ハッカーでしたにエラーが発生、サイトからのリンクによって作成された URL を変更することによって`http://localhost:xxxx/Movies/Details/1`のようなものに`http://localhost:xxxx/Movies/Details/12345`(または実際のムービーを表しません。 その他の値)。 Null のムービーのチェックがない場合は、データベース エラー、この可能性があります。

同様に、変更、`Delete`と`DeleteConfirmed`メソッド ID パラメーターの既定値を指定して、返される`HttpNotFound`ムービーが見つからない。 更新された`Delete`内のメソッド、`Movie`コント ローラーを以下に示します。

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

なお、`Delete`データは削除されません。 GET 要求の応答で削除操作を実行すると (さらに言えば、編集操作、作成操作、データを変更するその他のあらゆる操作を実行すると)、セキュリティに穴が空きます。 詳細については、Stephen Walther のブログ記事を参照してください。 [ASP.NET MVC ヒント #46-セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)です。

データを削除する `HttpPost` メソッドの名前は「`DeleteConfirmed`」になり、HTTP POST メソッドに一意のシグネチャまたは名前が与えられます。 2 つのメソッド シグネチャは下の画像のようになります。

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

共通言語ランタイム (CLR) には、オーバー ロードされたメソッドが、一意の署名を使用している (同じ名前、パラメーターのリストが異なる) が必要です。 ただし、ここで必要が 2 つの削除のメソッドと POST の 1 つ--GET--同じシグネチャをどちらも必要とします。 (いずれも、1 つの整数をパラメーターとして受け取る必要があります。)

この出力を並べ替えるには、いくつかの点を行うことができます。 別の名前を指定してメソッドを 1 つです。 彼は前の例で行ったです。 ただし、これは小さな問題を引き起こします。ASP.NET が URL のセグメントをアクション メソッドに名前でマッピングします。メソッドの名前を変更すると、通常、ルーティングでそのメソッドが見つからなくなります。 この解決策はこの例で確認できます。`ActionName("Delete")` 属性を `DeleteConfirmed` メソッドに追加しています。 これが効果的に実行マッピング、ルーティングのシステムの URL を含むように*/Delete/*post 要求を検索は、`DeleteConfirmed`メソッドです。

同じ名前とシグネチャを持つメソッドの問題を回避する別の方法では、意図的に未使用のパラメーターを含める POST メソッドのシグネチャを変更します。 一部の開発者がパラメーターの型を追加するなど、 `FormCollection` POST メソッドに渡されると、単にパラメーターを使用しません。

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>まとめ

SQL Server Compact データベースにデータを格納する完全な ASP.NET MVC アプリケーションがあるようになりました。 ことができますを作成、読み取り、更新、削除、および映画を検索します。

![](improving-the-details-and-delete-methods/_static/image1.png)

この基本的なチュートリアルでは、コント ローラー、ビューに関連付けると、ハード コードされたデータを渡すことを行う作業を開始する取得しました。 作成し、データ モデルを設計します。 Entity Framework Code First、実行時に、データ モデルからデータベースを作成し、ASP.NET MVC のスキャフォールディング システムは、アクション メソッドと基本的な CRUD 操作のビューに自動的に生成します。 ユーザーがデータベースを検索できる検索フォームが追加されます。 データの新しい列を含めるようにデータベースを変更し、作成し、この新しいデータを表示する 2 つのページを更新します。 データ モデルの属性をマークすることによって検証を追加した、`DataAnnotations`名前空間。 結果として得られる検証には、クライアントとサーバーが実行されます。

アプリケーションを配置する場合は、最初のテストをローカルの IIS 7 サーバーのアプリケーションに便利です。 これを行うこともできます[Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) ASP.NET アプリケーションの IIS 設定を有効にリンクします。 次の展開へのリンクを参照してください。

- [ASP.NET の展開のコンテンツ マップ](https://msdn.microsoft.com/library/dd394698.aspx)
- [IIS を有効にする 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Web アプリケーション プロジェクトの配置](https://msdn.microsoft.com/library/dd394698.aspx)

これで皆さんが、中間レベルに移動する[、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)と[MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md)チュートリアルについては、探索する、 [ASP.NETMSDN の記事](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)、および多くのビデオおよびにリソースをチェック アウトする[https://asp.net/mvc](https://asp.net/mvc) ASP.NET MVC についてさらに学習します。 [ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)質問を投稿する絶好の場所します。

それではお楽しみください。

-Scott Hanselman ([http://hanselman.com](http://hanselman.com)と[ @shanselman ](http://twitter.com/shanselman) Twitter の) と Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

>[!div class="step-by-step"]
[前へ](adding-validation-to-the-model.md)
