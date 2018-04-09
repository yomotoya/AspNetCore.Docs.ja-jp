---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: データベースの概要 Entity Framework 4.0 最初に、ASP.NET 4 Web フォームの |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: ad504b02d801f9513787f9fde1a4d00d7b0afff0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>データベースの概要 Entity Framework 4.0 最初に、ASP.NET 4 Web フォームします。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、架空の Contoso 大学の web サイトです。 学生の受け付け、講座の作成、講師の割り当てなどの機能が含まれています。
> 
> チュートリアルでは、c# の例を示します。 [ダウンロード可能なサンプル](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)c# および Visual Basic の両方でコードが含まれています。
> 
> ## <a name="database-first"></a>データベースの最初
> 
> Entity Framework でデータを操作できる 3 つの方法があります: *Database First*、 *Model First*、および*Code First*です。 このチュートリアルでは、データベースの最初のです。 シナリオに合った最適なものを選択する方法でこれらのワークフローとガイダンスの違いの詳細については、次を参照してください。 [Entity Framework 開発ワークフロー](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)です。
> 
> ## <a name="web-forms"></a>Web フォーム
> 
> このチュートリアルのシリーズ ASP.NET Web フォーム モデルを使用して Visual Studio での ASP.NET Web フォームを使用する方法を理解して、想定しています。 表示されない場合、 [ASP.NET 4.5 Web フォームの概要](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)です。 ASP.NET MVC フレームワークを使用する場合を参照してください。 [ASP.NET MVC を使用して Entity Framework の概要](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> | **このチュートリアルで示す** | **でも動作します。** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web です。 このチュートリアルは以降のバージョンの Visual Studio でテストされていません。 メニューのダイアログ ボックス、およびテンプレートの多くの違いがあります。 |
> | .NET 4 | .NET 4.5 は .NET 4 との下位互換性ですが、このチュートリアルは .NET 4.5 でテストされていません。 |
> | Entity Framework 4 | このチュートリアルは Entity Framework のバージョンでテストされていません。 EF は Entity Framework 5 以降では、既定では使用して、 `DbContext API` EF 4.1 で導入されたことです。 EntityDataSource コントロールを使用するように設計されました、 `ObjectContext` API です。 EntityDataSource を使用する方法については、コントロールを`DbContext`API を参照してください[このブログの投稿](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx)です。 |
> 
> ## <a name="questions"></a>質問
> 
> チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)、 [Entity Framework でも LINQ to Entities フォーラム](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)、または[StackOverflow.com](http://stackoverflow.com/)です。


## <a name="overview"></a>概要

これらのチュートリアルで構築するアプリケーションは、単純な大学の web サイトです。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

ユーザーは学生、講座、講師の情報を見たり、更新したりできます。 作成、画面のいくつかは、以下に示します。

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Web アプリケーションを作成します。

チュートリアルを開始するに Visual Studio を開き、新しい ASP.NET Web アプリケーション プロジェクトを使用して、作成、 **ASP.NET Web アプリケーション**テンプレート。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

このテンプレートでは、スタイル シートおよびマスター ページに既に含まれている web アプリケーション プロジェクトを作成します。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

開く、 *Site.Master*ファイルし、「Contoso 大学」に"My ASP.NET Application"に変更します。

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

検索、*メニュー*という名前のコントロール`NavigationMenu`し、作成する、ページのメニュー項目を追加、次のマークアップに置き換えます。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

開く、 *Default.aspx*ページし、変更、`Content`という名前のコントロール`BodyContent`に。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

作成するさまざまなページへのリンクを含む単純なホーム ページがあるようになりました。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>データベースの作成

これらのチュートリアルについては、既存のデータベースに基づくデータ モデルを自動的に作成する Entity Framework データ モデル デザイナーを使用します (多くの場合と呼ばれる、*データベース優先*アプローチ)。 このチュートリアルの系列に含まれていないことが手動でデータ モデルを作成し、データベースを作成するスクリプトの生成のデザイナーには (、*モデル優先*アプローチ)。

このチュートリアルで使用される、データベース優先メソッドを次の手順は、サイト データベースを追加するは。 最も簡単な方法では、まずこのチュートリアルでは、プロジェクトをダウンロードします。 右クリックし、*アプリ\_データ*フォルダーを選択**既存項目の追加**を選択し、 *School.mdf*データベース プロジェクト ファイルのダウンロードします。

代わりに、ある手順に従ってし[School サンプル データベースを作成する](https://msdn.microsoft.com/library/bb399731.aspx)です。 データベースをダウンロードするか、作成するかどうかをコピー、 *School.mdf*ファイルは、次のフォルダーから、アプリケーションの*アプリ\_データ*フォルダー。

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(この場所の*.mdf*ファイルでは、SQL Server 2008 Express を使用している前提としています)。

スクリプトからデータベースを作成する場合は、データベース ダイアグラムを作成する次の手順を実行します。

1. **サーバー エクスプ ローラー**、展開**データ接続**、展開*School.mdf*を右クリックして**データベース ダイアグラム**を選択して**新しいダイアグラムに追加**です。

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. すべてのテーブルを選択し、クリックして**追加**です。

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server では、テーブル、テーブル、およびテーブル間のリレーションシップの列を表示するデータベース ダイアグラムを作成します。 自由に整理するためにテーブルを移動することができます。
3. "SchoolDiagram"としてダイアグラムを保存して閉じます。

ダウンロードした場合、 *School.mdf*このチュートリアルでは、付いたファイルをダブルクリックして、データベース ダイアグラムを表示することができます**SchoolDiagram** **データベース ダイアグラム**で**サーバー エクスプ ローラー**です。

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

図は、次のような (テーブルをここに表示される内容から別の場所にあります)。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Entity Framework データ モデルを作成します。

ここでこのデータベースから、Entity Framework データ モデルを作成できます。 アプリケーションのルート フォルダーに、データ モデルを作成する可能性がありますただしこのチュートリアルを配置することという名前のフォルダーで*DAL* (データ アクセス層) 用です。

**ソリューション エクスプ ローラー**、という名前のプロジェクト フォルダーを追加*DAL* (ソリューションではなく、プロジェクトの下であることを確認してください)。

右クリックし、 *DAL*クリックしてフォルダー**追加**と**新しい項目の**します。 **インストールされたテンプレート****データ**を選択、 **ADO.NET エンティティ データ モデル**テンプレート、名前を付けます*SchoolModel.edmx*とをクリックして**追加**です。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

これは、Entity Data Model ウィザードを起動します。 ウィザードの最初の手順で、**データベースから生成**オプションは既定で選択します。 **[次へ]**をクリックします。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

**データ接続の選択**ステップ、既定値のままにしをクリックして**次**です。 School データベースが既定で選択されているし、の接続の設定を保存、 *Web.config*ファイルとして**SchoolEntities**です。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

**データベース オブジェクトの選択**以外のテーブルをすべて選択して、ウィザードの手順`sysdiagrams`(これは以前に作成したダイアグラムの作成) をクリックし、**完了**です。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

これには、モデルの作成が終了したら、Visual Studio に、データベース テーブルに対応する Entity Framework オブジェクト (エンティティ) のグラフィック表示が表示されます。 (と同様に、データベース ダイアグラムでは、個々 の要素の位置異なる可能性がありますから次の図に参照してください。 要素をドラッグできますの図に一致する場合に約。)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Entity Framework データ モデルの検証

あるエンティティの図によく似たいくつかの相違点を含む、データベース ダイアグラムを表示できます。 1 つの違いは、関連付けの種類を示す各アソシエーションの末尾のシンボルの追加 (テーブルのリレーションシップと呼ばれるエンティティの関連付け、データ モデル内)。

- 0 または 1 を 1 つの関連付けは、「1」と「0..1」で表されます。

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    ここで、`Person`エンティティがまたはに関連付けられていない可能性があります、`OfficeAssignment`エンティティです。 `OfficeAssignment`エンティティに関連付ける必要があります、`Person`エンティティです。 つまり、あるインストラクター、オフィスに割り当てることはできず、office は、1 つだけインストラクターに割り当てることが。
- 一対多の関連付けが「1」で表される、"\*"です。

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    ここで、`Person`エンティティ可能性がありますまたは関連付けることはできません`StudentGrade`エンティティです。 A`StudentGrade`エンティティが 1 つに関連付ける必要がある`Person`エンティティです。 `StudentGrade` エンティティが実際にこのデータベースに登録済みのコースを表しますコースに受講者を登録し、グレードがまだない場合、`Grade`プロパティが null です。 つまり、受講者コースに登録されていない可能性がありますをまだ 1 つのコースに登録することがあります。 または複数のコースに登録することがあります。 各学年の登録済みのコースは、1 つだけの学生を適用します。
- 多対多のアソシエーションとして表されます"\*「と」\*"です。

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    ここで、`Person`エンティティ可能性がありますまたは関連付けることはできません`Course`エンティティ、およびその逆の場合も:`Course`エンティティ可能性がありますまたは関連付けることはできません`Person`エンティティです。 つまり、インストラクターは、複数のコースを教えることがあり、複数講習においてインストラクターでコースを教える可能性があります。 (このデータベースでこのリレーションシップは講習においてインストラクターにのみ適用されます。 コースに受講者をリンクしないです。 受講者にリンクされてコース StudentGrades テーブルによって。)

データベース ダイアグラムとデータ モデルの別の違いは、追加**ナビゲーション プロパティ**セクションの各エンティティ。 エンティティのナビゲーション プロパティは、関連エンティティを参照します。 たとえば、`Courses`プロパティに、`Person`エンティティには、すべてのコレクションが含まれています、`Course`に関連付けられているエンティティ`Person`エンティティです。

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

データベースとデータ モデルとの違いがない場合は、まだ、`CourseInstructor`関連テーブルにリンクするデータベースで使用される、`Person`と`Course`多対多リレーションシップ内のテーブルです。 ナビゲーション プロパティを使用すると、関連する取得`Course`からエンティティ、`Person`エンティティと関連する`Person`からエンティティ、`Course`エンティティ、ため、アソシエーション モデルのテーブル データを表現する必要はありません。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

このチュートリアルの目的で、たとえば、`FirstName`の列、`Person`テーブルには実際には、個人の姓とミドル ネームの両方が含まれています。 これを反映するフィールドの名前を変更するが、データベース管理者 (DBA) することも、データベースを変更します。 名前を変更することができます、`FirstName`と同等のデータベースを中断している間、データ モデル内のプロパティが変更されません。

デザイナーを右クリックして**FirstName**で、`Person`クリックし、エンティティ**の名前を変更**です。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

"FirstMidName"の新しい名前を入力します。 これは、データベースを変更することがなく、コード内の列を参照する方法を変更します。

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

モデル ブラウザーでは、データベース構造、データ モデルの構造、およびそれらの間のマッピングを表示する別の方法を提供します。 表示する、エンティティ デザイナーで空白の領域を右クリックし、をクリックして**モデル ブラウザー**です。

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**モデル ブラウザー**ペインのツリーに表示します。 (、**モデル ブラウザー**でウィンドウをドッキングする可能性があります、**ソリューション エクスプ ローラー**ウィンドウです)。**SchoolModel**ノードが、データ モデルの構造を表す、 **SchoolModel.Store**ノードは、データベースの構造を表します。

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

展開**SchoolModel.Store**テーブルを表示するには、展開**テーブル/ビュー**テーブルを参照して、順に展開して**コース**にテーブル内の列を参照してください。

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

展開**SchoolModel**、展開**エンティティ型**の順に展開し、**コース**エンティティとエンティティに含まれるプロパティを表示するノードです。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

いずれか、デザイナーで、または**モデル ブラウザー**ペイン、Entity Framework が 2 つのモデルのオブジェクトを関連付ける方法を参照してください。 右クリックし、`Person`エンティティと選択**テーブル マッピング**です。

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

開き、**マッピングの詳細**ウィンドウです。 このウィンドウを使うとことを確認できることに注意してください、データベース列`FirstName`にマップされて`FirstMidName`、どのような名前を変更したをデータ モデルであります。

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework では、XML を使用して、データベース、データ モデル、およびそれらの間のマッピングに関する情報を格納します。 *SchoolModel.edmx*ファイルは、実際にこの情報を含む XML ファイルです。 デザイナーがグラフィカルな形式で情報をレンダリングしますを右クリックして、XML としてファイルを表示することも、 *.edmx*ファイル**ソリューション エクスプ ローラー**をクリックすると、 **を開く**を選択して**XML (テキスト) エディター**です。 (データ モデル デザイナーと XML エディターは開始タグと、デザイナーを開くし、同時に、XML エディターでファイルを開くことはできませんので、同じファイルの操作のさまざまな方法は 2 つだけ)。

Web サイト、データベース、およびデータ モデルが作成されました。 次のチュートリアルでは、データ モデルと ASP.NET を使用してデータの操作を開始するあります`EntityDataSource`コントロール。

> [!div class="step-by-step"]
> [次へ](the-entity-framework-and-aspnet-getting-started-part-2.md)
