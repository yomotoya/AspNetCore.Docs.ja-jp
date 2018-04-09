---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: 取得してを使用してデータを表示するモデル バインドと web フォーム |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 26873efa5dbfdbdab39a52cfb9cfd5a65c8231a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>取得して、モデル バインディング機能と web フォームでデータの表示
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。 この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。
> 
> モデル バインド パターンは、任意のデータ アクセス テクノロジと連携します。 このチュートリアルでは、Entity Framework に使用するが、最もなじみのあるデータ アクセス テクノロジを使用する可能性があります。 GridView、ListView、DetailsView、またはフォーム ビューのコントロールなどのデータ バインドされたサーバー コントロールからを選択すると、更新、削除、およびデータの作成に使用するメソッドの名前を指定します。 このチュートリアルでは、SelectMethod の値を指定します。
> 
> このメソッドは、内でデータを取得するロジックを提供します。 チュートリアルでは、[次へ]、UpdateMethod、DeleteMethod および InsertMethod の値を設定します。
> 
> 実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。 ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。
> 
> このチュートリアルでは、Visual Studio でアプリケーションを実行します。 利用できるアプリケーション、インターネット経由でホスティング プロバイダーに展開することで。 マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、  
>  [無料試用版の Azure アカウント](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)です。 Azure App Service Web アプリを Visual Studio web プロジェクトを展開する方法については、次を参照してください。、 [Visual Studio を使用して ASP.NET Web 配置](../../deployment/visual-studio-web-deployment/introduction.md)系列です。 そのチュートリアルでは、Entity Framework Code First Migrations を使用して Azure SQL Database に、SQL Server データベースを展開する方法も示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Microsoft Visual Studio 2013 または Microsoft Visual Studio Express 2013 for Web
>   
> 
> このチュートリアルでは Visual Studio 2012 もが、ユーザー インターフェイスとプロジェクト テンプレートでは、いくつか違いがあります。


## <a name="what-youll-build"></a>新機能のビルド

このチュートリアルでは、次の手順を。

1. 受講者コースに登録されていると、大学を反映するデータ オブジェクトを構築します。
2. データベース テーブル オブジェクトを構築します。
3. データベースにテスト データへの追加します。
4. Web フォームにデータを表示します。

## <a name="set-up-project"></a>プロジェクトを設定します。

Visual Studio 2013 で作成、新しい**ASP.NET Web アプリケーション**と呼ばれる**ContosoUniversityModelBinding**です。

![プロジェクトを作成します。](retrieving-data/_static/image2.png)

Web フォーム テンプレートを選択し、その他の既定のオプションのままにします。 プロジェクトをセットアップするには、[ok] をクリックします。

![web フォームを選択します。](retrieving-data/_static/image3.png)

最初に、いくつかのサイトの外観のカスタマイズを少し変更してください。 開く、 **Site.Master**ファイル、および含めるマイ ASP.NET アプリケーションではなく Contoso 大学のタイトルを変更します。

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

次に、ヘッダー テキストを変更します**アプリケーション名**に**Contoso 大学**です。

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

また、Site.Master では、このサイトに関連するページを反映するように、ヘッダーに表示されるナビゲーション リンクを変更します。 いずれかの必要はありません、**に関する**ページまたは**連絡先** ページが表示これらのリンクを削除することができます。 代わりに、必要がありますと呼ばれるページへのリンク**受講者**です。 このページはまだ作成されていません。

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

保存して Site.Master を閉じます。

ここで、学生のデータを表示するための web フォームを作成します。 プロジェクトを右クリックし、**追加**、**新しい項目の**します。 選択、**マスター ページを含む Web フォーム**テンプレート、名前を付けます**Students.aspx**です。

![作成 ページ](retrieving-data/_static/image5.png)

選択**Site.Master**新しい web フォームのマスター ページとして。

## <a name="create-the-data-models-and-database"></a>データ モデルとデータベースを作成します。

オブジェクトと、対応するデータベース テーブルを作成するのには、Code First Migrations を使用します。 これらのテーブルでは、受講者と、コースに関する情報を格納します。

という名前の新しいクラスを追加、Models フォルダーに**UniversityModels.cs**です。

![モデル クラスを作成します。](retrieving-data/_static/image7.png)

このファイルには、次のよう SchoolContext、学生、登録、および Course クラスを定義します。

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

SchoolContext クラスは、データベースの接続とデータの変更を管理する DbContext から派生します。

Student クラスで属性に適用されていることに注意してください。、 **FirstName**、 **LastName**、および**年**プロパティです。 これらの属性は、このチュートリアルでのデータ検証に使用されます。 このデモ プロジェクトのコードを簡略化これらのプロパティだけがデータ検証属性でマークされています。 実際のプロジェクトなど、登録およびコース クラスのプロパティの検証済みのデータが必要なすべてのプロパティに検証属性を適用します。

UniversityModels.cs を保存します。

Code First Migrations のツールを使用するこれらのクラスに基づくデータベースを設定します。

**Package Manager Console**コマンドを実行します。  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

コマンドが正常に完了した場合は、移行を有効になっていることを示すメッセージが表示されます。

![移行を有効にします。](retrieving-data/_static/image8.png)

新しいファイルの名前に注意してください**される Configuration.cs**が作成されました。 Visual Studio で、作成した後、このファイルが自動的に開きます。 構成クラスが含まれています、**シード**メソッドを事前にテスト データ、データベース テーブルに設定することができます。

Seed メソッドに次のコードを追加します。 追加する必要があります、**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**名前空間。

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

される Configuration.cs を保存します。

パッケージ マネージャー コンソールで、コマンドを実行`add-migration initial`です。

コマンドを実行して、`update-database`です。

このコマンドを実行している場合に、例外が発生する場合は、可能であればシード メソッド内の値から StudentID、CourseID の値が異なりますをします。 データベースでそれらのテーブルを開き、StudentID、CourseID の既存の値を検索します。 「登録」表は、シード処理のコードでそれら値を追加します。

データベース ファイルが追加されましたが現在のプロジェクトで非表示します。 をクリックして**すべてのファイル**ファイルを表示します。

![すべてのファイルを表示します。](retrieving-data/_static/image10.png)

.Mdf ファイルをアプリに表示されるように注意してください。\_データ フォルダーです。

![データベース ファイル](retrieving-data/_static/image12.png)

.Mdf ファイルをダブルクリックし、サーバー エクスプ ローラーを開きます。 テーブルは今すぐに存在し、データが挿入されます。

![データベース テーブル](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>受講者との関連テーブルからデータを表示します。

データベース内のデータでデータを取得して、web ページに表示する準備ができました。 使用して、 **GridView**列および行のデータを表示するコントロール。

Students.aspx を開き、検索、**メイン**プレース ホルダーです。 そのプレース ホルダー内に、追加、 **GridView**コントロールを次のコードが含まれています。

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

このマークアップ コードを確認するための重要な概念についていくつかあります。 最初に、値が に設定されていることに注意してください、 **SelectMethod** GridView 要素内のプロパティです。 この値は、GridView のデータの取得に使用されるメソッドの名前を指定します。 このメソッドは、次の手順で作成します。 次に、ことに注意して、 **ItemType**プロパティが以前に作成した Student クラスに設定します。 この値を設定するでは、マークアップのコードでは、そのクラスのプロパティを参照できます。 たとえば、Student クラスには、「登録」という名前コレクションが含まれていますいます。 使用することができます**Item.Enrollments**をそのコレクションを取得し、LINQ 構文を使用して、各学生の登録済みのクレジットの合計を取得します。

分離コード ファイルで指定されているメソッドを追加する必要があります、 **SelectMethod**値。 開いている**Students.aspx.cs**、し、追加**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**と**System.Data.Entity**名前空間。

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

次に、次のメソッドを追加します。 このメソッドの名前が、SelectMethod の指定した名前と一致することに注意してください。

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

**Include**句はこのクエリのパフォーマンスが向上しますが、作業するクエリは必須ではありません。 Include 句を使用せず、データをデータベースを別のクエリごとに関連するデータの取得の送信が行われる、遅延読み込みを使用して取得はします。 Include 句を指定して、データベースの 1 つのクエリからのすべての関連するデータを取得することを意味する一括読み込みを使用してデータを取得します。 関連するデータの大部分ができない場合に多くのデータが取得されるので、使用される、一括読み込みは効率が低下することができます。 ただし、この場合、一括読み込みは、最適なパフォーマンス レコードごとに、関連するデータが表示されるためです。

読み込むときのパフォーマンスに関する考慮事項の詳細については、データを関連、というタイトルのセクションを参照してください**Lazy、Eager、および明示的な読み込みの関連するデータ**で、 [、ASP では、Entity Framework と関連するデータの読み取り.NET MVC アプリケーション](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)トピックです。

既定では、データは、キーとしてマークされているプロパティの値によって並べ替えられます。 並べ替え用の別の値を指定する、OrderBy 句を追加することができます。 この例では既定の学生 Id プロパティは並べ替えに使用されます。 [並べ替え、ページング、およびデータのフィルター処理](sorting-paging-and-filtering-data.md)トピックを有効にしたユーザーを並べ替え列を選択します。

Web アプリを実行し、受講者のページに移動します。 受講者ページには、次の受講者情報が表示されます。

![データを表示します。](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>モデル バインド メソッドの自動生成

このチュートリアルの一連の操作、チュートリアルからプロジェクトに、コードを単にコピーできます。 ただし、このアプローチの欠点の 1 つではモデル バインド メソッドのコードを自動的に生成する Visual Studio によって提供される機能の対応はならないことです。 使用する場合、独自のプロジェクトで、自動コード生成は時間と、操作を実装する方法の意味を獲得するヘルプするを保存できます。 このセクションでは、自動コード生成機能について説明します。 ここでは情報提供のみし、プロジェクトに実装する必要があります。 コードが含まれていません。

値を設定するときに、 **SelectMethod**、 **UpdateMethod**、 **InsertMethod**、または**DeleteMethod**マークアップのコードでのプロパティ選択することができます、**新しいメソッドの作成**オプション。

![新しいメソッドを作成します。](retrieving-data/_static/image18.png)

Visual Studio は、分離コードで適切なシグネチャを持つメソッドを作成するだけでなく、操作の実行に役立つ実装コードも生成されます。 最初に設定する場合、 **ItemType** 、生成されたコードの自動コード生成機能を使用する前にプロパティは、操作の型を使用します。 たとえば、UpdateMethod プロパティを設定するときに自動的に次のコードが生成されます。

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

もう一度、上記のコードは、プロジェクトに追加する必要はありません。 次のチュートリアルでは、更新、削除、および新しいデータを追加するためのメソッドを実装します。

## <a name="conclusion"></a>まとめ

このチュートリアルでは、データ モデル クラスを作成し、それらのクラスからデータベースを生成します。 データベースのテーブルは、テスト データが入力されます。 データベースからデータを取得するモデル バインドを使用して GridView にデータを表示します。

次の[チュートリアル](updating-deleting-and-creating-data.md)この系列には有効にする更新、削除、およびデータを作成します。

> [!div class="step-by-step"]
> [次へ](updating-deleting-and-creating-data.md)
