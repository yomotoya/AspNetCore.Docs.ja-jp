---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: Details メソッドと Delete メソッド (VB) の向上 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 0741603b8a280e4ab74b03018b5395b185542343
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399913"
---
<a name="improving-the-details-and-delete-methods-vb"></a>Details メソッドと Delete メソッド (VB) の向上
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 C# を使用する場合に切り替えて、 [c# バージョン](../cs/improving-the-details-and-delete-methods.md)このチュートリアルの。


このチュートリアルでは、これを自動的に生成されたいくつかの機能強化を行います`Details`と`Delete`メソッド。 これらの変更は不要ですが、コードのほんの小さなビット アプリケーションを簡単に拡張できます。

## <a name="improving-the-details-and-delete-methods"></a>詳細と Delete メソッドの改善

スキャフォールディングすると、`Movie`コント ローラーで、ASP.NET MVC が、正しく動作したをすることも、いくつかの小さな変更をより堅牢なコードを生成します。

開く、`Movie`コント ローラーを変更し、`Details`メソッドを返すことによって`HttpNotFound`ムービーが見つからない場合。 変更することも必要があります、`Details`メソッドに渡された ID の既定値を設定します。 (するような変更を加えた、`Edit`メソッド[パート 6](examining-the-edit-methods-and-edit-view.md)このチュートリアルの)。ただし、戻り値の型を変更する必要があります、`Details`メソッドから`ViewResult`に`ActionResult`ため、`HttpNotFound`メソッドが返さない、`ViewResult`オブジェクト。 次の例は、変更された`Details`メソッド。

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

コード最初に簡単にデータを使用して検索、`Find`メソッド。 メソッドに作成した重要なセキュリティ機能は、コードが確認します、`Find`メソッドは、コードが、それ以降を実行する前に、ムービーが検出されます。 たとえば、ハッカーでしたにエラーが発生、サイトからのリンクが作成する URL を変更することで`http://localhost:xxxx/Movies/Details/1`ようなものに`http://localhost:xxxx/Movies/Details/12345`(または実際のムービーを表していないその他のいくつかの値)。 Null のムービー チェックがない場合は、データベース エラー、この可能性があります。

同様に、変更、`Delete`と`DeleteConfirmed`メソッド ID パラメーターの既定値を指定して、返される`HttpNotFound`ムービーが見つからない場合。 更新された`Delete`メソッド、`Movie`コント ローラーを以下に示します。

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

なお、`Delete`メソッド データは削除されません。 GET 要求の応答で削除操作を実行すると (さらに言えば、編集操作、作成操作、データを変更するその他のあらゆる操作を実行すると)、セキュリティに穴が空きます。 詳細については、Stephen Walther のブログ記事を参照してください。 [ASP.NET MVC ヒントと 46: セキュリティ ホールを作成するため、削除のリンクを使用しない](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx)します。

データを削除する `HttpPost` メソッドの名前は「`DeleteConfirmed`」になり、HTTP POST メソッドに一意のシグネチャまたは名前が与えられます。 2 つのメソッド シグネチャは下の画像のようになります。

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

共通言語ランタイム (CLR) では、オーバー ロードされたメソッドが、一意のシグネチャを使用している (同じ名前、パラメーターのリストが異なる) が必要です。 ただし、ここで必要 2 つ削除メソッド--GET の 1 つ--、投稿の 1 つと同じシグネチャをどちらも必要とします。 (いずれも、1 つの整数をパラメーターとして受け取る必要があります。)

この出力を並べ替えるには、いくつかの点を行うことができます。 メソッドの別の名前を付けて 1 つです。 前の例で行ったです。 ただし、これは小さな問題を引き起こします。ASP.NET が URL のセグメントをアクション メソッドに名前でマッピングします。メソッドの名前を変更すると、通常、ルーティングでそのメソッドが見つからなくなります。 この解決策はこの例で確認できます。`ActionName("Delete")` 属性を `DeleteConfirmed` メソッドに追加しています。 これが効果的に実行マッピング、ルーティング システムの URL を含むように<em>/delete/</em>要求を検索する投稿について、`DeleteConfirmed`メソッド。

同じ名前とシグネチャを持つメソッドでの問題を回避するもう 1 つの方法は、意図的に未使用のパラメーターを含める POST メソッドのシグネチャを変更します。 たとえば、一部の開発者がパラメーターの型を追加`FormCollection`は POST メソッドに渡されると、単にパラメーターを使用しません。

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>まとめ

SQL Server Compact データベースにデータを格納する完全な ASP.NET MVC アプリケーションがあるようになりました。 ことができますを作成、読み取り、更新、削除、および映画を検索します。

![](improving-the-details-and-delete-methods/_static/image1.png)

この基本的なチュートリアルでは、コント ローラー、ビューと関連付けると、ハード コーディングされたデータの受け渡しを行う作業を開始する必要があります。 作成され、データ モデルを設計します。 Entity Framework Code First をその場でデータ モデルからデータベースを作成し、ASP.NET MVC のスキャフォールディング システムは、アクション メソッドと基本的な CRUD 操作のビューに自動的に生成します。 ユーザーがデータベースを検索できる検索フォームを追加しました。 データの新しい列を含めるようにデータベースを変更し、2 つのページを作成してこの新しいデータの表示を更新します。 検証を追加した属性を使用してデータ モデルをマークすることによって、`DataAnnotations`名前空間。 結果の検証には、クライアントとサーバーが実行されます。

アプリケーションをデプロイする場合は、最初のテスト、ローカルの IIS 7 サーバーでアプリケーションをことをお勧めします。 これを使用することができます[Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) ASP.NET アプリケーション用の IIS 設定を有効にリンクします。 次の展開のリンクを参照してください。

- [ASP.NET 配置コンテンツ マップ](https://msdn.microsoft.com/library/dd394698.aspx)
- [IIS を有効にする 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Web アプリケーション プロジェクトの配置](https://msdn.microsoft.com/library/dd394698.aspx)

中間レベルに移動するようになりました[、ASP.NET MVC アプリケーション用の Entity Framework データ モデルを作成する](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)と[MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md)チュートリアルでは、探索、 [ASP.NETmsdn の記事](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)と多くのビデオとリソースをチェック アウトする[ https://asp.net/mvc ](https://asp.net/mvc) ASP.NET MVC についてさらに学習します。 [ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)質問する絶好の場所します。

それではお楽しみください。

— Scott Hanselman ([ http://hanselman.com ](http://hanselman.com)と[ @shanselman ](http://twitter.com/shanselman) twitter) と Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [前へ](adding-validation-to-the-model.md)
