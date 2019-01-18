---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: 取得と表示を使用してデータ モデルのバインディングと web フォーム |Microsoft Docs
author: Rick-Anderson
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396286"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>取得して、モデル バインディングと web フォームでデータの表示
====================

> このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。 このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。
> 
>  モデル バインドのパターンは、任意のデータ アクセス テクノロジと連携します。 このチュートリアルでは、Entity Framework を使用するが、最も使い慣れたデータ アクセス テクノロジを使用できます。 GridView、ListView、DetailsView、または FormView コントロールなどのデータ バインド サーバー コントロールから選択すると、更新、削除、およびデータの作成に使用するメソッドの名前を指定します。 このチュートリアルでは、SelectMethod の値を指定します。 
> 
> そのメソッド内では、データを取得するロジックを提供します。 次のチュートリアルでは、UpdateMethod、InsertMethod、DeleteMethod の値を設定します。
>
> できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)で完全なプロジェクトC#または Visual Basic です。 ダウンロード可能なコードは、Visual Studio 2012 以降のバージョンとは動作します。 これは、このチュートリアルで示すように、Visual Studio 2017 のテンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。
> 
> チュートリアルでは、Visual Studio でアプリケーションを実行します。 ホスティング プロバイダーにアプリケーションを展開し、インターネット経由で使用できるようにもできます。 マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、  
>  [無料 Azure 試用版アカウント](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)します。 Visual Studio web プロジェクトを Azure App Service Web Apps にデプロイする方法については、次を参照してください。、 [Visual Studio を使用して ASP.NET Web 配置](../../deployment/visual-studio-web-deployment/introduction.md)シリーズ。 そのチュートリアルでは、Entity Framework Code First Migrations を使用して Azure SQL Database に SQL Server データベースをデプロイする方法も示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> - Microsoft Visual Studio 2017 または Microsoft Visual Studio Community 2017
>   
> このチュートリアルは Visual Studio 2012 と Visual Studio 2013 も機能しますはユーザー インターフェイスとプロジェクト テンプレートでは、いくつか違いがあります。


## <a name="what-youll-build"></a>構築します

このチュートリアルではあります。

* 受講者コースに登録されていると、大学を反映するデータ オブジェクトを構築します。
* データベース テーブル オブジェクトを構築します。
* テスト データでデータベースを設定します。
* Web フォームでデータを表示

## <a name="create-the-project"></a>プロジェクトの作成

1. Visual Studio 2017 では、作成、 **ASP.NET Web アプリケーション (.NET Framework)** という名前のプロジェクト**ContosoUniversityModelBinding**します。

   ![プロジェクトを作成します。](retrieving-data/_static/image19.png)

2. **[OK]** を選択します。 テンプレートを選択するダイアログ ボックスが表示されます。

   ![web フォームを選択します。](retrieving-data/_static/image3.png)

3. 選択、 **Web フォーム**テンプレート。 

4. 必要に応じて、変更への認証**個々 のユーザー アカウント**します。 

5. **[OK]** をクリックして、プロジェクトを作成します。

## <a name="modify-site-appearance"></a>サイトの外観を変更します。

   サイトの外観をカスタマイズするいくつかの変更を加えます。 
   
   1. Site.Master ファイルを開きます。
   
   2. 表示するタイトルを変更する**Contoso University**なく**My ASP.NET Application**します。

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. ヘッダー テキストを変更する**アプリケーション名**に**Contoso University**します。

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. 適切なサイトへのナビゲーション ヘッダーのリンクを変更します。 
   
      リンクを削除**について**と**連絡先**し、代わりに、リンク、**学生**ページで、作成されます。

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Site.Master を保存します。

## <a name="add-a-web-form-to-display-student-data"></a>受講者データを表示する web フォームを追加します。

   1. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加**し**新しい項目の**します。 
   
   2. **新しい項目の追加**ダイアログ ボックスで、**マスター ページを使用した Web フォーム**テンプレートと名前を付けます**Students.aspx**します。

      ![ページを作成します。](retrieving-data/_static/image5.png)

   3. **[追加]** を選びます。
   
   4. Web フォームのマスター ページで、選択**Site.Master**します。
   
   5. **[OK]** を選択します。
   

## <a name="add-the-data-model"></a>データ モデルを追加する

**モデル**フォルダー、という名前のクラスを追加**UniversityModels.cs**します。

   1. 右クリックして**モデル**を選択します**追加**、し**新しい項目の**します。 **[新しい項目の追加]** ダイアログ ボックスが表示されます。

   2. 左側のナビゲーション メニューから選択**コード**、し**クラス**します。

      ![モデル クラスを作成します。](retrieving-data/_static/image20.png)

   3. クラスの名前**UniversityModels.cs**選択**追加**します。

      このファイルで定義、 `SchoolContext`、 `Student`、`Enrollment`と`Course`クラスの次のようにします。

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      `SchoolContext`クラスから派生`DbContext`、データベース接続の管理し、データに変更します。

      `Student`クラス、適用される属性に注意してください、 `FirstName`、 `LastName`、および`Year`プロパティ。 このチュートリアルは、データ検証用のこれらの属性を使用します。 コードを簡略化するのには、これらのプロパティのみがデータ検証属性でマークされます。 実際のプロジェクトでは、検証を必要とするすべてのプロパティに検証属性が適用されます。

   4. UniversityModels.cs を保存します。

## <a name="set-up-the-database-based-on-classes"></a>クラスに基づくデータベースを設定します。

このチュートリアルでは[Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/)オブジェクトを作成し、データベース テーブル。 これらのテーブルでは、受講者とそのコースに関する情報を格納します。

   1. 選択**ツール** > **NuGet パッケージ マネージャー** > **パッケージ マネージャー コンソール**します。

   2. **パッケージ マネージャー コンソール**、このコマンドを実行します。  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      コマンドが正常に完了すると、移行を有効になっていることを示すメッセージが表示されます。

      ![移行を有効にします。](retrieving-data/_static/image8.png)

      という名前のファイルに注意してください*Configuration.cs*が作成されました。 `Configuration`クラスには、`Seed`メソッドは、事前にテスト データ、データベース テーブルに設定できます。

## <a name="pre-populate-the-database"></a>データベースを事前設定します。

   1. Configuration.cs を開きます。
   
   2. `Seed` メソッドに次のコードを追加します。 また、追加、`using`のステートメント、`ContosoUniversityModelBinding. Models`名前空間。

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Configuration.cs を保存します。

   4. パッケージ マネージャー コンソールで、コマンドを実行**追加移行の初期**します。

   5. コマンドを実行**更新データベース**します。

      このコマンドを実行するときに例外が発生した場合、`StudentID`と`CourseID`値が異なる可能性があります、`Seed`メソッドの値。 これらのデータベース テーブルを開きの既存の値を検索`StudentID`と`CourseID`します。 これらの値をシード処理のコードを追加、`Enrollments`テーブル。

## <a name="add-a-gridview-control"></a>GridView コントロールを追加します。

設定されているデータベースのデータで、そのデータを取得し表示する準備ができましたがようになりました。 

1. Students.aspx を開きます。

2. 検索、`MainContent`プレース ホルダーです。 プレース ホルダーを追加、 **GridView**コントロールをこのコードが含まれています。

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   点に注意してください。
   * 設定値は、通知、 `SelectMethod` GridView 要素プロパティ。 この値は、次の手順で作成するは、GridView データの取得に使用する方法を指定します。 
   
   * `ItemType`プロパティに設定されて、`Student`クラスの前に作成します。 この設定では、マークアップでクラスのプロパティを参照できます。 たとえば、`Student`クラスという名前のコレクションには`Enrollments`します。 使用することができます`Item.Enrollments`そのコレクションを取得し、使用する[LINQ 構文](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq)各受講者を取得するクレジットの合計を登録します。
   
3. Students.aspx を保存します。

## <a name="add-code-to-retrieve-data"></a>データを取得するコードを追加します。

   Students.aspx 分離コード ファイルで指定されたメソッドを追加、`SelectMethod`値。 
   
   1. Students.aspx.cs を開きます。
   
   2. 追加`using`のステートメント、`ContosoUniversityModelBinding. Models`と`System.Data.Entity`名前空間。

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. 指定したメソッドを追加`SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      `Include`句は、クエリのパフォーマンスを向上しますは必要ありません。 なし、`Include`を使用して句では、データを取得[*遅延読み込み*](https://en.wikipedia.org/wiki/Lazy_loading)データの取得に関連するたびにデータベースに個別のクエリを送信する処理も行われます。 `Include`を使用して句では、データを取得*一括読み込み*、つまり、1 つのデータベース クエリには、すべての関連データを取得します。 関連するデータが使用されていない場合より多くのデータが取得されるため、一括読み込みは効率が低下します。 ただし、この場合、一括読み込みでは、最高のパフォーマンス レコードごとに、関連するデータが表示されるためです。

      データ関連の読み込み時にパフォーマンスに関する考慮事項の詳細についてを参照してください、**非同期 (lazy)、Eager、および明示的な読み込みの関連データ**セクション、 [Entity Framework では、ASP.NET で関連データの読み取りMVC アプリケーション](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)記事。

      既定では、データは、キーとしてマークされているプロパティの値で並べ替えられます。 追加することができます、`OrderBy`句を異なる並べ替え値を指定します。 この例では、既定では`StudentID`プロパティの並べ替えに使用します。 [並べ替え、ページング、およびデータのフィルター処理](sorting-paging-and-filtering-data.md)記事では、ユーザーを有効にすると、並べ替えの列を選択します。
 
   4. Students.aspx.cs を保存します。

## <a name="run-your-application"></a>アプリケーションを実行します。 

Web アプリケーションを実行 (**F5**) に移動し、**学生**ページで、次の表示。

   ![データを表示します。](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>モデル バインド メソッドの自動生成

このチュートリアル シリーズを通して、チュートリアルからプロジェクトに、コードを単にコピーできます。 ただし、このアプローチの欠点の 1 つは、モデル バインド メソッドのコードを自動的に生成する Visual Studio によって提供される機能の対応はならないことにします。 独自のプロジェクトで作業して、ときに自動コード生成は時間とヘルプができるように操作を実装する方法を把握するを節約できます。 このセクションでは、自動コード生成の機能について説明します。 ここでは情報提供のみし、プロジェクトに実装する必要があるコードが含まれていません。 

値を設定するときに、 `SelectMethod`、 `UpdateMethod`、 `InsertMethod`、または`DeleteMethod`マークアップ コード内のプロパティ を選択できます、**新しいメソッドの作成**オプション。

![メソッドを作成します。](retrieving-data/_static/image18.png)

Visual Studio では、適切なシグネチャを持つ、分離コードでメソッドを作成するだけでなく、操作を実行する実装コードも生成されます。 最初に設定した場合、`ItemType`自動コード生成を使用する前にプロパティの機能、生成されたコードを使用して入力の操作。 たとえば、設定するときに、`UpdateMethod`プロパティでは、次のコードが自動的に生成されます。

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

ここでも、このコードをプロジェクトに追加する必要はありません。 次のチュートリアルでは、更新、削除、および新しいデータを追加するためのメソッドを実装します。

## <a name="summary"></a>まとめ

このチュートリアルでは、データ モデル クラスを作成し、それらのクラスからデータベースを生成します。 テスト データには、データベース テーブルを入力します。 モデル バインド、データベースからデータを取得するために使用して GridView にデータが表示されます。

次の[チュートリアル](updating-deleting-and-creating-data.md)でこのシリーズでは、更新、削除、およびデータの作成を有効にします。

> [!div class="step-by-step"]
> [次へ](updating-deleting-and-creating-data.md)
