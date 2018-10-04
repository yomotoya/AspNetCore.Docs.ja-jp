---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Movie モデルとデータベース テーブル (VB) に新しいフィールドの追加 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、これを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明しています.
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 8fb0fb5c1db24f1961bba08f7b1c2182caca39ca
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577432"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Movie モデルとデータベース テーブル (VB) に新しいフィールドの追加
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> このチュートリアルでは、Microsoft Visual Web Developer 2010 Express Service Pack 1、Microsoft Visual Studio の無料版であるを使用して ASP.NET MVC Web アプリケーションの構築の基礎を説明します。 始める前に、以下の前提条件がインストールされていることを確認します。 次のリンクをクリックしてそれらのすべてをインストールすることができます: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)します。 または、次のリンクを使用して、前提条件を個別にインストールできます。
> 
> - [Visual Studio Web Developer Express SP1 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update します。](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(ランタイムとツールのサポート)
> 
> Visual Web Developer 2010 ではなく Visual Studio 2010 を使用する場合は、次のリンクをクリックして、前提条件をインストール: [Visual Studio 2010 の前提条件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)します。
> 
> VB.NET のソース コードでの Visual Web Developer プロジェクトは、このトピックと共に使用できます。 [VB.NET のバージョンをダウンロード](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)します。 C# を使用する場合に切り替えて、 [c# バージョン](../cs/adding-a-new-field.md)このチュートリアルの。


このセクションではモデル クラスにいくつかの変更を加えるし、モデルの変更に合わせてデータベース スキーマを更新する方法について説明します。

## <a name="adding-a-rating-property-to-the-movie-model"></a>ムービー モデルへの評価プロパティの追加

まず、新しい追加`Rating`プロパティを既存の`Movie`クラス。 開く、 *Movie.cs*追加ファイルを開き、`Rating`次のようなプロパティ。

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

完全な`Movie`今のような次のコードをクラスします。

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

アプリケーションを使用して、再コンパイル、**デバッグ** &gt;**ビルド ムービー**メニュー コマンド。

更新した、`Model`クラスもする必要がある更新、 *\Views\Movies\Index.vbhtml* と *\Views\Movies\Create.vbhtml* 新しいをサポートするためにテンプレートを表示`Rating`プロパティ。

開く、<em>\Views\Movies\Index.vbhtml</em>追加ファイルを開き、`<th>Rating</th>`直後の列ヘッダー、<strong>価格</strong>列。 追加し、`<td>`をレンダリングするテンプレートの末尾付近の列、`@item.Rating`値。 以下はどのような更新<em>Index.vbhtml</em>ビュー テンプレートのようになります。

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

次に、開く、 *\Views\Movies\Create.vbhtml*ファイルを開き、フォームの末尾付近の次のマークアップを追加します。 これにより、新しいムービーの作成時に、評価を指定できるように、テキスト ボックスを表示します。

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>モデルとデータベース スキーマの相違点を管理します。

新しいをサポートするアプリケーション コードを更新するようになりました`Rating`プロパティ。

これで、アプリケーションを実行しに移動し、 */Movies* URL。 ただし、これを行うときに、次のエラーを表示されます。

![](adding-a-new-field/_static/image1.png)

ため、このエラーが表示されている更新された`Movie`アプリケーションでモデル クラスは、スキーマの異なる、`Movie`既存のデータベースのテーブル。 (データベース テーブルに `Rating` 列はありません)。

既定を使用する Entity Framework Code First のデータベースを自動的に作成するように、このチュートリアルで前に行ったときに Code First テーブル データベースに追加して、データベースのスキーマはから生成されたモデル クラスと同期されているかどうかを追跡できます。 同期していない、Entity Framework は、エラーをスローします。 これにより、表示されている可能性がありますそれ以外の場合のみ (原因不明のエラー) を実行時に、開発時に問題を追跡しやすくします。 同期チェック機能は、原因は何を表示するエラー メッセージがあります。

エラーを解決する 2 つの方法はあります。

1. Entity Framework に、新しいモデル クラス スキーマに基づいてデータベースを自動的にドロップさせ、再作成させます。 この方法は非常に便利な場合、テスト データベースでアクティブな開発を行うため、簡単にまとめてモデルとデータベース スキーマを進化させることができます。 短所は、データベース内の既存のデータが失われる-ようにする*しない*実稼働データベースでこのアプローチを使用する!
2. モデル クラスに一致するように、既存のデータベースのスキーマを明示的に変更します。 この手法の長所は、データが維持されることです。 この変更は手動で行うことも、データベース変更スクリプトを作成して行うこともできます。

このチュートリアルでは、最初のアプローチを使用します:、Entity Framework Code First 自動的にモデルを変更するたびにデータベースを再作成する必要があります。

## <a name="automatically-re-creating-the-database-on-model-changes"></a>モデルの変更のデータベースを自動的に再作成

Code First 自動的に削除し、アプリケーションのモデルを変更するたびに、データベースを再作成できるようにアプリケーションを更新してみましょう。

> [!NOTE] 
> 
> **警告**を自動的に削除して、開発またはテスト データベースを使用している場合にのみ、データベースを再作成するには、この方法を有効にする必要がありますと*決して*実際のデータを格納している実稼働データベースでします。 実稼働サーバー上で使用すると、データ損失につながることができます。


**ソリューション エクスプ ローラー**を右クリックして、*モデル*フォルダーで、**追加**、し、**クラス**します。

![](adding-a-new-field/_static/image2.png)

クラスの名前&quot;MovieInitializer&quot;します。 更新プログラム、`MovieInitializer`クラスに次のコードを含めます。

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

`MovieInitializer`クラスは、モデルによって使用されるデータベースを削除してモデル クラスを変更する場合を自動的に再作成することを指定します。 コードが含まれています、`Seed`時間がいくつか既定のデータを自動的にデータベースに追加する、いずれかを指定するメソッドが作成 (または再作成) します。 これは、モデルを変更するたびに手動で設定することがなく、データベースにいくつかサンプル データを設定する便利な方法を提供します。

定義したところ、`MovieInitializer`クラスに確認するように、アプリケーションが実行されるたびに、モデル クラスが、データベース内のスキーマと異なるかどうかを接続する必要あります。 その場合は、モデルと一致し、サンプル データをデータベースにデータベースを再作成する初期化子を実行できます。

開く、 *Global.asax*ファイルのルートにある、`MvcMovies`プロジェクト。

*Global.asax*ファイルが含まれています、プロジェクトの完全なアプリケーションを定義するクラスが含まれています、`Application_Start`初回起動時にアプリケーションを実行しているイベント ハンドラー。

検索、`Application_Start`メソッド呼び出しを追加および`Database.SetInitializer`次に示すよう、メソッドの先頭。

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

`Database.SetInitializer`ステートメントを追加したが、データベースを使用することを示します、`MovieDBContext`インスタンスは自動的に削除してスキーマとデータベースが一致しない場合を再作成する必要があります。 で指定されているサンプル データでデータベースが設定されますも学習したように、`MovieInitializer`クラス。

閉じる、 *Global.asax*ファイル。

アプリケーションを再実行しに移動し、 */Movies* URL。 アプリケーションの起動時には、モデル構造に、データベース スキーマが一致しなくを検出します。 自動的に、新しいモデルの構造と一致するデータベースを再作成および、サンプルのムービーのデータベースを設定します。

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

をクリックして、**新規作成**のリンクを新しいムービーを追加します。 評価を追加できることに注意してください。

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

**[作成]** をクリックします。 評価を含む、新しいムービーに表示されるビデオを一覧表示します。

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

このセクションでは、モデル オブジェクトを変更し、データベースの変更との同期を維持する方法を説明しました。 シナリオを試すことができますので、サンプル データを使って新しく作成したデータベースを設定する方法も学習しました。 次に、高度な検証ロジックはモデル クラスを追加し、いくつかのビジネス ルールを適用するを有効にする方法を見てみましょう。

> [!div class="step-by-step"]
> [前へ](examining-the-edit-methods-and-edit-view.md)
> [次へ](adding-validation-to-the-model.md)
