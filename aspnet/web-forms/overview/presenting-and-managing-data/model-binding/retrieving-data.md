---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: 取得と表示を使用してデータ モデルのバインディングと web フォーム |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c04e4c94378c8143c919e83af90312dd003b8c84
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832689"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>取得して、モデル バインディングと web フォームでデータの表示
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。 このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。
> 
> モデル バインドのパターンは、任意のデータ アクセス テクノロジと連携します。 このチュートリアルでは、Entity Framework を使用するが、最も使い慣れたデータ アクセス テクノロジを使用できます。 GridView、ListView、DetailsView、または FormView コントロールなどのデータ バインド サーバー コントロールから選択すると、更新、削除、およびデータの作成に使用するメソッドの名前を指定します。 このチュートリアルでは、SelectMethod の値を指定します。
> 
> そのメソッド内では、データを取得するロジックを提供します。 次のチュートリアルでは、UpdateMethod、InsertMethod、DeleteMethod の値を設定します。
> 
> できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。
> 
> チュートリアルでは、Visual Studio でアプリケーションを実行します。 利用できるアプリケーション、インターネット経由でホスティング プロバイダーへのデプロイで。 マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、  
>  [無料 Azure 試用版アカウント](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)します。 Visual Studio web プロジェクトを Azure App Service Web Apps にデプロイする方法については、次を参照してください。、 [Visual Studio を使用して ASP.NET Web 配置](../../deployment/visual-studio-web-deployment/introduction.md)シリーズ。 そのチュートリアルでは、Entity Framework Code First Migrations を使用して Azure SQL Database に SQL Server データベースをデプロイする方法も示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Microsoft Visual Studio 2013 または Microsoft Visual Studio Express 2013 for Web
>   
> 
> このチュートリアルは Visual Studio 2012 でも機能しますが、ユーザー インターフェイスとプロジェクト テンプレートでは、いくつか違いがあります。


## <a name="what-youll-build"></a>構築します

このチュートリアルではあります。

1. 受講者コースに登録されていると、大学を反映するデータ オブジェクトを構築します。
2. データベース テーブル オブジェクトを構築します。
3. テスト データでデータベースを設定します。
4. Web フォームでデータを表示

## <a name="set-up-project"></a>プロジェクトを設定します。

Visual Studio 2013 で作成する新しい**ASP.NET Web アプリケーション**と呼ばれる**ContosoUniversityModelBinding**します。

![プロジェクトを作成します。](retrieving-data/_static/image2.png)

Web フォーム テンプレートを選択し、その他の既定のオプションのままにします。 プロジェクトをセットアップするには、[ok] をクリックします。

![web フォームを選択します。](retrieving-data/_static/image3.png)

最初に、いくつかのサイトの外観をカスタマイズする小さな変更が作成されます。 開く、 **Site.Master**ファイルを開き、含める My ASP.NET アプリケーションではなく、Contoso University のタイトルを変更します。

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

ヘッダー テキストを変更し、**アプリケーション名**に**Contoso University**します。

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

また、Site.Master では、このサイトに関連するページを反映するように、ヘッダーに表示されるナビゲーション リンクを変更します。 いずれかの必要はありません、**について**ページまたは**連絡先**ページのでこれらのリンクを削除できます。 というページにリンクする必要が代わりに、**学生**します。 このページはまだ作成されていません。

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

保存して Site.Master を閉じます。

ここで、生徒のデータを表示するための web フォームを作成します。 プロジェクトを右クリックし、**追加**、**新しい項目の**。 選択、**マスター ページを使用した Web フォーム**テンプレート、名前を付けます**Students.aspx**します。

![ページを作成します。](retrieving-data/_static/image5.png)

選択**Site.Master**新しい web フォームのマスター ページとして。

## <a name="create-the-data-models-and-database"></a>データ モデルとデータベースを作成します。

Code First Migrations を使用すると、オブジェクトと、対応するデータベース テーブルを作成します。 これらのテーブルでは、受講者とそのコースに関する情報を格納します。

という名前の新しいクラスを追加、Models フォルダーに**UniversityModels.cs**します。

![モデル クラスを作成します。](retrieving-data/_static/image7.png)

このファイルでは、よう SchoolContext、学生、登録、およびコースのクラスを定義します。

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

SchoolContext クラスは、データベース接続とデータの変更を管理する DbContext から派生します。

Student クラスで属性に適用されていることを確認、 **FirstName**、 **LastName**、および**年**プロパティ。 これらの属性は、このチュートリアルではデータの検証に使用されます。 このデモ プロジェクトのコードを簡素化するには、これらのプロパティのみがデータ検証属性でマークされています。 実際のプロジェクトでは、登録とコースのクラスのプロパティなど、検証済みのデータが必要なすべてのプロパティに検証属性が適用されます。

UniversityModels.cs を保存します。

Code First Migrations のツールを使用して、これらのクラスに基づいてデータベースを設定します。

**パッケージ マネージャー コンソール**のコマンドを実行します。  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

移行を有効になっていることを示すメッセージが表示されます、コマンドが正常に完了する場合

![移行を有効にします。](retrieving-data/_static/image8.png)

新しいファイルの名前に注意してください**Configuration.cs**が作成されました。 Visual Studio で、作成後に自動的に、このファイルが開きます。 構成クラスが含まれています、**シード**メソッド テスト データをデータベース テーブルを事前に設定することができます。

Seed メソッドを次のコードを追加します。 追加する必要があります、**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**名前空間。

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Configuration.cs を保存します。

パッケージ マネージャー コンソールで、コマンドを実行`add-migration initial`します。

コマンドを実行して、`update-database`します。

このコマンドを実行するときに、例外が発生した場合は、Seed メソッド内の値から StudentID、CourseID の値があるさまざまなことができます。 データベースでそれらのテーブルを開き、StudentID、CourseID の既存の値を検索します。 登録のテーブルのシード処理のコードでは、これらの値を追加します。

データベース ファイルが追加されていますが、プロジェクトが現在非表示。 クリックして**すべてのファイル**ファイルを表示します。

![すべてのファイルを表示します。](retrieving-data/_static/image10.png)

アプリが表示されます、.mdf ファイルに注意してください。\_データ フォルダー。

![データベース ファイル](retrieving-data/_static/image12.png)

.Mdf ファイルをダブルクリックし、サーバー エクスプ ローラーを開きます。 これで、テーブルは存在し、データが設定されます。

![データベース テーブル](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>受講者と関連テーブルからデータを表示します。

データベース内のデータとそのデータを取得し、web ページに表示する準備ができました。 使用する、 **GridView**列と行のデータを表示するコントロール。

Students.aspx を開き、検索、 **MainContent**プレース ホルダーです。 プレース ホルダーを追加、 **GridView**コントロールを次のコードが含まれています。

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

いくつかのことを確認するには、このマークアップ コードで重要な概念があります。 まずの値が設定されていることを確認、 **SelectMethod** GridView 要素プロパティ。 この値は、GridView のデータを取得するために使用されるメソッドの名前を指定します。 このメソッドは、次の手順で作成します。 次に、注意、 **ItemType**を先ほど作成した学生クラスにプロパティを設定します。 この値を設定するには、マークアップのコードでは、そのクラスのプロパティを参照できます。 たとえば、Student クラスには、登録をという名前のコレクションが含まれています。 使用することができます**Item.Enrollments**をそのコレクションを取得し、LINQ 構文を使用して、各学生の登録済みのクレジットの合計を取得します。

分離コード ファイルで指定されているメソッドを追加する必要があります、 **SelectMethod**値。 開いている**Students.aspx.cs**、追加**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**と**System.Data.Entity**名前空間。

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

次に、次のメソッドを追加します。 このメソッドの名前が、SelectMethod の指定した名前と一致することに注意してください。

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

**Include**句は、このクエリのパフォーマンスが向上しますが、クエリの動作に必須ではありません。 Include 句を指定せず、データを遅延読み込みは、データベースに別のクエリを毎回関連データの取得を送信する必要がありますを使用して取得ができます。 Include 句を指定して、データベースの 1 つのクエリですべての関連データを取得することを意味する一括読み込みを使用してデータを取得します。 関連データのほとんどができない場合に使用される、一括読み込みはより多くのデータが取得されるので、効率が低下します。 ただし、ここでは、一括読み込みは、最高のパフォーマンス レコードごとに、関連するデータが表示されるためです。

データ関連の読み込み時にパフォーマンスに関する考慮事項の詳細については、「セクションを参照してください。**非同期 (lazy)、Eager、および明示的な読み込みの関連データ**で、 [ASP で Entity Framework に関連するデータの読み取り.NET MVC アプリケーション](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)トピック。

既定では、データは、キーとしてマークされているプロパティの値で並べ替えられます。 別の値の並べ替えを指定する OrderBy 句を追加できます。 この例での並べ替え、既定の学生 Id プロパティに使用します。 [並べ替え、ページング、およびデータのフィルター処理](sorting-paging-and-filtering-data.md)トピックでは、並べ替え列を選択するユーザーにによりします。

Web アプリケーションを実行し、Students ページに移動します。 Students のページには、次の生徒の情報が表示されます。

![データを表示します。](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>モデル バインド メソッドの自動生成

このチュートリアル シリーズを通して、チュートリアルからプロジェクトに、コードを単にコピーできます。 ただし、このアプローチの欠点の 1 つは、モデル バインド メソッドのコードを自動的に生成する Visual Studio によって提供される機能の対応はならないことにします。 独自のプロジェクトで作業して、ときに自動コード生成は時間とヘルプができるように操作を実装する方法を把握するを節約できます。 このセクションでは、自動コード生成の機能について説明します。 ここでは情報提供のみし、プロジェクトに実装する必要があるコードが含まれていません。

値を設定するときに、 **SelectMethod**、 **UpdateMethod**、 **InsertMethod**、または**DeleteMethod**マークアップのコードでのプロパティ選択することができます、**新しいメソッドの作成**オプション。

![新しいメソッドを作成します。](retrieving-data/_static/image18.png)

Visual Studio では、適切なシグネチャを持つ、分離コードでメソッドを作成するだけでなく、操作を実行する際の実装コードも生成されます。 最初に設定した場合、 **ItemType**操作のため、生成されたコードの自動コード生成機能を使用する前にプロパティがその型を使用します。 たとえば、UpdateMethod プロパティを設定するときに、次のコードが自動的に生成します。

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

ここでも、上記のコードは、プロジェクトに追加する必要はありません。 次のチュートリアルでは、更新、削除、および新しいデータを追加するためのメソッドを実装します。

## <a name="conclusion"></a>まとめ

このチュートリアルでは、データ モデル クラスを作成し、それらのクラスからデータベースを生成します。 テスト データには、データベース テーブルを入力します。 モデル バインド、データベースからデータを取得するために使用して GridView にデータが表示されます。

次の[チュートリアル](updating-deleting-and-creating-data.md)このシリーズでは有効にすると、更新、削除、およびデータを作成します。

> [!div class="step-by-step"]
> [次へ](updating-deleting-and-creating-data.md)
