---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Web フォームの作業の Entity Framework 4.0 Database で最初に開始および ASP.NET 4 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: fbfbc667b4031c67827dc158ffca4d1b4dbfb23e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394295"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Web フォームの作業の Entity Framework 4.0 Database で最初に開始および ASP.NET 4
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、架空の Contoso University の web サイトです。 学生の受け付け、講座の作成、講師の割り当てなどの機能が含まれています。
> 
> このチュートリアルでは、c# で例を示します。 [ダウンロード可能なサンプル](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)c# および Visual Basic の両方でコードが含まれています。
> 
> ## <a name="database-first"></a>最初のデータベースします。
> 
> Entity Framework 内のデータを使用する 3 つの方法があります: *Database First*、 *Model First*、および*Code First*します。 このチュートリアルでは、データベースの最初の。 シナリオに最適なものを選択する方法に関するこれらのワークフローとガイダンスの違いについては、次を参照してください。 [Entity Framework 開発ワークフロー](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)します。
> 
> ## <a name="web-forms"></a>Web フォーム
> 
> このチュートリアル シリーズでは、ASP.NET Web フォーム モデルを使用して、Visual Studio で ASP.NET Web フォームを使用する方法を理解する前提としています。 表示されない場合、 [ASP.NET 4.5 Web フォームの概要](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)します。 ASP.NET MVC フレームワークで作業する場合を参照してください。 [ASP.NET MVC を使用して Entity Framework の概要](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。
> 
> ## <a name="software-versions"></a>ソフトウェアのバージョン
> 
> | **このチュートリアルで示すように** | **でも使用できます。** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web です。 このチュートリアルは以降のバージョンの Visual Studio でテストされていません。 メニューのダイアログ ボックス、およびテンプレートで多くの違いがあります。 |
> | .NET 4 | .NET 4.5 は .NET 4 では、旧バージョンと互換性のあるが、このチュートリアルは .NET 4.5 でテストされていません。 |
> | Entity Framework 4 | このチュートリアルは Entity Framework の以降のバージョンでテストされていません。 既定では、Entity Framework 5 以降では、EF で使用、`DbContext API`は EF 4.1 で導入されました。 EntityDataSource コントロールが使用するように設計、 `ObjectContext` API。 コントロールを EntityDataSource を使用する方法については、 `DbContext` API を参照してください[このブログの投稿](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx)します。 |
> 
> ## <a name="questions"></a>質問
> 
> チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)、 [Entity Framework と LINQ to エンティティ フォーラム](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)、または[StackOverflow.com](http://stackoverflow.com/)します。


## <a name="overview"></a>概要

これらのチュートリアルで作成するアプリケーションは、簡単な大学向け web サイトです。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

ユーザーは学生、講座、講師の情報を見たり、更新したりできます。 いくつかの画面を作成しますが、以下に示します。

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Web アプリケーションを作成します。

チュートリアルを開始する Visual Studio を開き、使用して新しい ASP.NET Web アプリケーション プロジェクトを作成し、 **ASP.NET Web アプリケーション**テンプレート。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

このテンプレートでは、スタイル シートとマスター ページに既に含まれている web アプリケーション プロジェクトを作成します。

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

開く、 *Site.Master*ファイルし、"Contoso University"に"My ASP.NET Application"に変更します。

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

検索、*メニュー*という名前のコントロール`NavigationMenu`し作成するページのメニュー項目を追加します。 次のマークアップに置き換えます。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

開く、 *Default.aspx*ページし、変更、`Content`という名前のコントロール`BodyContent`この。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

作成するさまざまなページへのリンクを含む単純なホーム ページがあるようになりました。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>データベースの作成

これらのチュートリアルについては、既存のデータベースに基づくデータ モデルを自動的に作成、Entity Framework データ モデル デザイナーを使用します (多くの場合と呼ばれる、*データベース優先*アプローチ)。 代わりにこのチュートリアル シリーズでは説明しませんが、データ モデルを手動で作成し、データベースを作成するスクリプトのデザイナー生成する (、*モデルファースト*アプローチ)。

このチュートリアルで使用されるデータベース優先方法の場合は、次の手順は、サイト データベースを追加するは。 最も簡単な方法では、まずこのチュートリアルではプロジェクトをダウンロードします。 右クリックし、*アプリ\_データ*フォルダーで、**既存項目の追加**を選択し、 *School.mdf*ダウンロードしたプロジェクトからデータベース ファイル。

代替策」の手順に従うは[School サンプル データベースを作成する](https://msdn.microsoft.com/library/bb399731.aspx)します。 データベースをダウンロードまたは作成するかどうかをコピー、 *School.mdf*ファイルは、次のフォルダーから、アプリケーションの*アプリ\_データ*フォルダー。

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(この場所の *.mdf*ファイルでは、SQL Server 2008 Express を使用している前提としています)。

スクリプトからデータベースを作成する場合は、データベース ダイアグラムを作成するには、次の手順を実行します。

1. **サーバー エクスプ ローラー**、展開**データ接続**、展開*School.mdf*、右クリックして**データベース ダイアグラム**を選択します **。新しいダイアグラムの追加**します。

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. すべてのテーブルを選択し、クリックして**追加**します。

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server では、テーブル、列、テーブル、およびテーブル間のリレーションシップを表示するデータベース ダイアグラムを作成します。 自由に整理するためにテーブルを移動することができます。
3. "SchoolDiagram"としてダイアグラムを保存して閉じます。

ダウンロードする場合、 *School.mdf*このチュートリアルでは、付いたファイルをダブルクリックして、データベース ダイアグラムを表示する**SchoolDiagram** **データベース ダイアグラム**で**サーバー エクスプ ローラー**します。

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

図は、次のような (テーブルをここに表示される内容から別の場所にあります)。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Entity Framework データ モデルを作成します。

これでこのデータベースから Entity Framework データ モデルを作成できます。 アプリケーションのルート フォルダーで、データ モデルを作成できますが、このチュートリアルではありますに配置するという名前のフォルダー *DAL* (のデータ アクセス層)。

**ソリューション エクスプ ローラー**、という名前のプロジェクト フォルダーの追加*DAL* (ソリューションではなく、プロジェクトの下にあることを確認ください)。

右クリックし、 *DAL*クリックしてフォルダー**追加**と**新しい項目の**します。 **インストールされたテンプレート**を選択します**データ**を選択、 **ADO.NET Entity Data Model**テンプレート、名前を付けます*SchoolModel.edmx*とクリックして**追加**します。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

これは、Entity Data Model ウィザードを起動します。 ウィザードの最初の手順で、**データベースから生成**オプションは既定で選択されます。 **[次へ]** をクリックします。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

**データ接続の選択**ステップ、既定値のまま をクリックして**次**します。 既定では、School データベースが選択されているし、では、接続設定を保存してください。、 *Web.config*ファイルとして**SchoolEntities**します。

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

**データベース オブジェクトの選択**ウィザードの手順を除く、テーブルをすべて選択`sysdiagrams`(以前に作成したダイアグラムの作成された) 順にクリックします**完了**します。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

これには、モデルの作成が終了したら、Visual Studio に、データベース テーブルに対応する Entity Framework オブジェクト (エンティティ) のグラフィカル表現が示します。 (データベース ダイアグラムで、個々 の要素の場所異なる可能性がありますこの図で参照してください。 要素をドラッグできますする場合は、図を一致するように約。)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Entity Framework データ モデルの検証

エンティティの図は、いくつかの相違点と、データベース ダイアグラムによく似て ことを確認できます。 1 つの違いは、関連付けの種類を示す各アソシエーションの end のシンボルの追加 (テーブルのリレーションシップと呼ばれるエンティティの関連付けデータ モデル内)。

- 0 または 1 に 1 つのアソシエーションは、「1」と「0..1」で表されます。

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    ここで、`Person`エンティティがまたはに関連付けられていない可能性があります、`OfficeAssignment`エンティティ。 `OfficeAssignment`にエンティティが関連付けられている必要がありますが、`Person`エンティティ。 つまり、インストラクター、オフィスに割り当てることはできませんし、office 講師が 1 つだけに割り当てることができます。
- 一対多のアソシエーションは、「1」で表される、"\*"。

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    ここで、`Person`エンティティのことがありますまたは関連付けることはできません`StudentGrade`エンティティ。 A`StudentGrade`エンティティが 1 つを関連付ける必要がある`Person`エンティティ。 `StudentGrade` エンティティが実際にこのデータベースに登録されているコースを表します学生がコースに登録されているし、グレードがまだない場合、`Grade`プロパティは null です。 つまり、受講者コースに登録されていない可能性があります、1 つのコースに登録することがあります。 または複数のコースに登録することがあります。 各学年の登録済みのコースは、学生の 1 つだけに適用されます。
- 多対多のアソシエーションとして表されます"\*「と」\*"。

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    ここで、`Person`エンティティのことがありますまたは関連付けることはできません`Course`エンティティ、およびその逆は true も:`Course`エンティティのことがありますまたは関連付けることはできません`Person`エンティティ。 つまり、インストラクターが複数のコースを教えることがあり、コースは、複数の講師が担当することがあります。 (、このデータベースにこの関係はインストラクターのみに適用されます。 コースを受講者はリンクされません。 受講者にリンクされてコース StudentGrades テーブルによって。)

データベース ダイアグラムとデータ モデルにはもう 1 つの違いは、追加**ナビゲーション プロパティ**各エンティティのセクション。 エンティティのナビゲーション プロパティは、関連エンティティを参照します。 たとえば、`Courses`プロパティ、`Person`エンティティには、すべてのコレクションが含まれています、`Course`に関連付けられているエンティティ`Person`エンティティ。

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

データベースとデータ モデルのもう 1 つの違いがない場合は、まだ、`CourseInstructor`関連テーブルにリンクする、データベースで使用される、`Person`と`Course`多対多リレーションシップのテーブル。 ナビゲーション プロパティでは、関連を取得できます。`Course`からエンティティ、`Person`エンティティと関連`Person`からエンティティ、`Course`エンティティ データ モデルに関連テーブルを表示する必要はありませんので。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

このチュートリアルの目的で、たとえば、`FirstName`の列、`Person`テーブルには実際には、個人の姓とミドル ネームの両方が含まれています。 これを反映するフィールドの名前を変更したいが、データベース管理者 (DBA) が、データベースに変更しない可能性があります。 名前を変更することができます、`FirstName`まま、そのデータベースは同等のデータ モデルでプロパティが変更されません。

デザイナーで、右クリック**FirstName**で、`Person`エンティティ、および選択**の名前を変更**します。

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

新しい名前"FirstMidName"を入力します。 これは、データベースを変更することがなくコード内の列を参照する方法を変更します。

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

モデル ブラウザーでは、データベース構造、データ モデルの構造、およびそれらの間のマッピングを表示する別の方法を提供します。 表示は、エンティティ デザイナーでの空白領域を右クリックし、順にクリックします**モデル ブラウザー**します。

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

**モデル ブラウザー**ウィンドウには、ツリー ビューが表示されます。 (、**モデル ブラウザー**ウィンドウとドッキング可能性があります、**ソリューション エクスプ ローラー**ウィンドウ)。**SchoolModel**ノードが、データ モデルの構造を表す、 **SchoolModel.Store**ノードは、データベースの構造を表します。

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

展開**SchoolModel.Store**テーブルを表示する展開**テーブル/ビュー**テーブルを参照し、展開に**コース**にテーブル内の列を参照してください。

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

展開**SchoolModel**、展開**エンティティ型**の順に展開し、**コース**エンティティと、エンティティ内でプロパティを表示するノード。

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

いずれか、デザイナーで、または**モデル ブラウザー**ウィンドウで、Entity Framework が 2 つのモデルのオブジェクトを関連付ける方法を参照してください。 右クリックし、`Person`エンティティと選択**テーブル マッピング**します。

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

開き、**マッピングの詳細**ウィンドウ。 このウィンドウを使うとことを確認できることに注意してください、データベース列`FirstName`にマップされます`FirstMidName`、どのような名前を変更したをデータ モデルであります。

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework では、XML を使用して、データベース、データ モデル、およびそれらの間のマッピングに関する情報を格納します。 *SchoolModel.edmx*ファイルは、実際にこの情報を含む XML ファイル。 デザイナーは、グラフィカルな形式で情報を表示しますを右クリックし、XML としてファイルを表示することもできます、 *.edmx*ファイル**ソリューション エクスプ ローラー**、 **から開く**を選択して**XML (テキスト) エディター**します。 (データのモデル デザイナーと XML エディターを開くと、デザイナーを開くし、同時に、XML エディターでファイルを開くことはできませんが、同じファイルで作業のさまざまな方法が 2 つだけです)。

Web サイト、データベース、およびデータ モデルを作成したようになりました。 次のチュートリアルでは、データ モデルと ASP.NET を使用してデータの操作を開始するを`EntityDataSource`コントロール。

> [!div class="step-by-step"]
> [次へ](the-entity-framework-and-aspnet-getting-started-part-2.md)
