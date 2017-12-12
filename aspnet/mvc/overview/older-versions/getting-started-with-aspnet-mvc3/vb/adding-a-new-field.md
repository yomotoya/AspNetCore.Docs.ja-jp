---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: "ムービーのモデルとデータベースのテーブル (VB) に新しいフィールドを追加する |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 377c667a56bb5c0d58ecef5c3550ca510ec52546
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>ムービーのモデルとデータベースのテーブル (VB) に新しいフィールドを追加します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これは、無料版の Microsoft Visual Studio を使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)です。 また、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用している場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)です。
> 
> VB.NET のソース コードと Visual Web Developer プロジェクトは、このトピックを使用できます。 [バージョンをダウンロードして VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)です。 C# を使用する場合に切り替え、 [c# バージョン](../cs/adding-a-new-field.md)このチュートリアルのです。


このセクションでは、モデルのクラスにいくつかの変更を加えるし、モデルの変更に合わせてデータベース スキーマを更新する方法について説明します。

## <a name="adding-a-rating-property-to-the-movie-model"></a>ムービー モデルへの評価プロパティの追加

新しく追加することによって開始`Rating`を既存のプロパティ`Movie`クラスです。 開く、 *Movie.cs*ファイルを追加、`Rating`次のようなプロパティ。

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

完全な`Movie`クラスの現在のような次のコード。

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

アプリケーションを使用して、再コンパイル、**デバッグ** &gt;**作成の映画**メニュー コマンド。

これで、更新した、`Model`クラスも更新する必要が、 *\Views\Movies\Index.vbhtml*と*\Views\Movies\Create.vbhtml*新しいをサポートするためにテンプレートの表示`Rating`プロパティです。

開く、*\Views\Movies\Index.vbhtml*ファイルを追加、`<th>Rating</th>`列見出し直後、**価格**列です。 追加し、`<td>`をレンダリングするテンプレートの末尾付近の列、`@item.Rating`値。 以下はどのような更新*Index.vbhtml*ビュー テンプレートはようになります。

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

次に、開く、 *\Views\Movies\Create.vbhtml*ファイルおよびフォームの末尾付近の次のマークアップを追加します。 これにより、評価を指定するには、新しいムービーの作成時にできるように、テキスト ボックスが表示します。

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>モデルとデータベース スキーマの相違点を管理します。

新しいをサポートするアプリケーション コードを更新するようになりました`Rating`プロパティです。

これで、アプリケーションを実行しに移動し、 */Movies* URL。 ただし、これを行うときに、次のエラーを表示されます。

![](adding-a-new-field/_static/image1.png)

このエラーが表示されている、更新された`Movie`アプリケーションのモデル クラスがのスキーマと異なるようになりました、`Movie`既存のデータベースのテーブルです。 (データベース テーブルに `Rating` 列はありません)。

既定を使用して Entity Framework Code First を自動的に、データベースの作成と同様、このチュートリアルで前に Code First テーブル データベースに追加して、データベースのスキーマはから生成されたモデルのクラスと同期されているかどうかを追跡できます。 同期そうでない場合、Entity Framework は、エラーをスローします。 これにより、それ以外の場合のみがあります (あいまいなエラー) を実行時に開発時に問題を追跡しやすくします。 同期チェック機能は、何が原因で表示されるエラー メッセージだけが表示されます。

これには、エラーを解決するための 2 つの方法があります。

1. Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。 このので方法は非常に便利にテスト データベースに対するアクティブな開発を行うときに簡単にモデルとデータベースのスキーマを一緒に発展させることができます。 、欠点は、データベース内の既存のデータが失われること、— ようにする*しない*を実稼働データベースでこの方法を使用します。
2. モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。 この手法の長所は、データが維持されることです。 この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。

このチュートリアルでは、最初の方法として使用されます:、Entity Framework Code First 自動的にモデルを変更するたびにデータベースを再作成する必要があります。

## <a name="automatically-re-creating-the-database-on-model-changes"></a>モデルの変更のデータベースを自動的に再作成

Code First 自動的に削除し、アプリケーションのモデルを変更するたびに、データベースを再作成できるようにアプリケーションを更新してみましょう。

> [!NOTE] 
> 
> **警告**を自動的に削除して、開発またはテスト データベースを使用している場合にのみ、データベースを再作成するには、この方法を有効にする必要がありますと*決して*実際のデータが含まれている実稼働データベースでします。 実稼働サーバー上で使用すると、データ損失につながることができます。


**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーを選択**追加**、し、**クラス**です。

![](adding-a-new-field/_static/image2.png)

クラスの名前を付けます&quot;MovieInitializer&quot;です。 更新プログラム、`MovieInitializer`クラスに次のコードを含めます。

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

`MovieInitializer`クラスは、モデルによって使用されるデータベースを削除して自動的に再作成モデル クラスを変更することを指定します。 コードが含まれています、`Seed`時間がいくつか自動的にデータベースに追加する、いずれかの既定のデータを指定するメソッドが作成 (または再作成) します。 これは、モデルの変更を加えるたびに手動で設定することがなく、一部のサンプル データでは、データベースを設定する便利な方法を提供します。

これで定義した、`MovieInitializer`クラスにする、アプリケーションを実行するたびにチェックするようモデル クラスは、データベース内のスキーマと異なるかどうかに接続します。 場合は、モデルと一致し、サンプル データをデータベースにデータベースを再作成する初期化子を実行することができます。

開く、 *Global.asax*のルートにあるファイル、`MvcMovies`プロジェクト。

*Global.asax*ファイルには、プロジェクトの完全なアプリケーションを定義し、クラスが含まれています、`Application_Start`アプリケーションを初めて起動したときに実行するイベント ハンドラー。

検索、`Application_Start`メソッド呼び出しを追加および`Database.SetInitializer`次のように、メソッドの先頭に。

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

`Database.SetInitializer`追加したステートメントがによってデータベースが使用されることを示す、`MovieDBContext`インスタンスは自動的に削除して、スキーマとデータベースが一致しない場合を再作成する必要があります。 説明したとおりにでも、データベースに設定で指定されているサンプル データと、`MovieInitializer`クラスです。

閉じる、 *Global.asax*ファイル。

アプリケーションを再実行しに移動し、 */Movies* URL。 アプリケーションの起動時には、モデルの構造が不要になったデータベース スキーマと一致することを検出します。 自動的に新しいモデル構造と一致するデータベースを再作成され、データベース、サンプルのムービーに設定されます。

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

をクリックして、**新規作成**リンクを新しいムービーを追加します。 評価を追加できることに注意してください。

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)


              **[作成]**をクリックします。 この評価を含む、新しいムービーに表示されます、映画を一覧表示します。

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

このセクションではモデル オブジェクトを変更し、データベースの変更との同期を維持する方法を説明しました。 また、シナリオを実行するためのサンプル データ、新しく作成されたデータベースに設定する方法も学習しました。 次に、モデル クラスをより詳細な検証ロジックを追加し、適用するビジネス ルールの一部を有効にする方法を見てみましょう。

>[!div class="step-by-step"]
[前へ](examining-the-edit-methods-and-edit-view.md)
[次へ](adding-validation-to-the-model.md)
