---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Entity Framework 4.0 の新機能新機能 |Microsoft Docs
author: tdykstra
description: このチュートリアル シリーズでは、Entity Framework 4.0 のチュートリアル シリーズの概要を作成した Contoso University web アプリケーションに基づいています。 ここには.
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 960aaf7aa7c2cff5c51079ecfe50031fa82d5508
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827842"
---
<a name="whats-new-in-the-entity-framework-40"></a>Entity Framework 4.0 の新機能新機能
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズは、Contoso University web アプリケーションによって作成される、 [Entity Framework の概要](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)チュートリアル シリーズです。 前のチュートリアルを完了していない場合は、このチュートリアルの開始点としてできます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)に、作成します。 できます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完全なチュートリアル シリーズで作成します。 チュートリアルについて質問等がございましたらを投稿できます、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)します。


前のチュートリアルでは、Entity Framework を使用する web アプリケーションのパフォーマンスを最大化するためのいくつかのメソッドを説明しました。 このチュートリアルは、Entity Framework の version 4 で最も重要な新機能の一部を確認し、すべての新しい機能をより詳細な概要を提供するリソースにリンクします。 このチュートリアルでは強調表示されている機能を以下に示します。

- 外部キー アソシエーション。
- ユーザー定義の SQL コマンドを実行します。
- モデル ファースト開発します。
- POCO サポートします。

さらに、このチュートリアルは導入について簡単に*code first 開発*、Entity Framework の次期リリースで予定されている機能です。

チュートリアルを開始するには、Visual Studio を起動し、前のチュートリアルで作業していた Contoso University web アプリケーションを開きます。

## <a name="foreign-key-associations"></a>外部キーの関連付け

Entity Framework のバージョン 3.5 には、ナビゲーション プロパティが含まれていますが、外部キー プロパティ、データ モデルでは作成されませんでした。 たとえば、`CourseID`と`StudentID`の列、`StudentGrade`から省略されるテーブル、`StudentGrade`エンティティ。

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

このアプローチの理由は、厳密に言えば、外部キーが物理的な実装の詳細をし、概念データ モデルに属していませんでした。 ただし、実際には、容易に、外部キーに直接アクセスできる場合、コード内のエンティティを扱うは多くの場合。

データ モデル内のどの外部キーの例では、コードを簡略化できます、どのようにする必要があるコードを検討してください、 *DepartmentsAdd.aspx*せずにページ。 `Department` 、エンティティ、`Administrator`プロパティに対応する外部キーは、`PersonID`で、`Person`エンティティ。 新しい部門の管理者との関連付けを確立するための値を設定すべてを行う必要があるが、`Administrator`プロパティ、`ItemInserting`データ バインド コントロールのイベント ハンドラー。

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

処理とデータ モデルの外部キーは、なし、`Inserting`の代わりに、データ ソース コントロールのイベント、`ItemInserting`エンティティがエンティティ セットに追加する前に、エンティティ自体への参照を取得するために、データ バインド コントロールのイベント。 その参照がある場合は、次の例では、そのようなコードを使用して関連付けを確立します。

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Entity Framework チームのでわかるように[外部キー アソシエーションでブログの投稿](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx)コードの複雑さの違いが非常に大きいその他のケースがあります。 簡単なコードのために、概念データ モデルの実装の詳細を好きな方のニーズを満たすために Entity Framework なりましたなどのデータ モデルの外部キーのオプション。

Entity Framework の用語では、データ モデルに外部キーを含める場合は使用している*外部キー アソシエーション*を使用している外部キーを除外した場合と*独立アソシエーション*します。

## <a name="executing-user-defined-sql-commands"></a>ユーザー定義の SQL コマンドを実行します。

Entity Framework の以前のバージョンでは、その場で独自の SQL コマンドを作成し、実行する簡単な方法はありませんでした。 Entity Framework は、SQL コマンドを動的に生成するか、ストアド プロシージャを作成し、関数としてインポートする必要がありました。 バージョン 4 で追加されて`ExecuteStoreQuery`と`ExecuteStoreCommand`メソッド、`ObjectContext`を任意のクエリをデータベースに直接渡す簡単にするクラス。

Contoso University の管理者は、ストアド プロシージャを作成し、データ モデルにインポートするプロセスを経由することがなく、データベースで一括変更を実行することができるようにするとします。 最初の要求が、ページ、データベース内のすべてのコースの単位数を変更することができます。 値の乗算に使用する番号を入力することができるようにする、web ページでは、すべて`Course`行の`Credits`列。

使用する新しいページを作成、 *Site.Master*マスター ページと名前を付けます*UpdateCredits.aspx*します。 次のマークアップを追加し、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

このマークアップを作成、`TextBox`乗数値を入力できるコントロール、`Button`コマンドを実行するためにクリックするコントロールと`Label`影響を受ける行の数を示すためのコントロール。

開いている*UpdateCredits.aspx.cs*、次の追加と`using`ステートメントと、ボタンのハンドラー`Click`イベント。

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

このコードは、SQL を実行します。`Update`コマンドのテキスト ボックスに値を使用して、ラベルを使用して、影響を受ける行の数を表示します。 ページを実行する前に実行、 *Courses.aspx* "before"画像の一部のデータを取得するページ。

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

実行*UpdateCredits.aspx*、乗数として「10」を入力し、クリックして**Execute**します。

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

実行、 *Courses.aspx*ページをもう一度変更されたデータを参照してください。

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(で、元の値に戻す単位数を設定する場合*UpdateCredits.aspx.cs*変更`Credits * {0}`に`Credits / {0}`除数として 10 を入力するページを再実行します)。

コードで定義するクエリの実行の詳細については、次を参照してください。[方法: 直接実行コマンドに対して、データ ソース](https://msdn.microsoft.com/library/ee358769.aspx)します。

## <a name="model-first-development"></a>モデル ファースト開発

これらのチュートリアルでは、まずデータベースを作成し、データベース構造に基づくデータ モデルを生成し。 Entity Framework 4 では、代わりに、データ モデルで開始し、データ モデルの構造に基づいて、データベースを生成できます。 モデル ファーストのアプローチを使用すると、エンティティといない物理実装の詳細について心配することの中に、アプリケーションの概念的には意味のあるリレーションシップを作成、データベースが存在しない既に対象のアプリケーションを作成する場合. (この状態は、開発の初期段階でのみ true ただしします。 モデルから再作成することはできなく実用的な; と最終的に、データベースが作成されで、運用データになりますその時点でもデータベース優先のアプローチに戻る。)

チュートリアルのこのセクションでは、単純なデータ モデルを作成し、そこから、データベースを生成します。

**ソリューション エクスプ ローラー**を右クリックし、 *DAL*フォルダーと選択**新しい項目の追加**します。 **新しい項目の追加**ダイアログ ボックスで、**インストールされたテンプレート**選択**データ**を選び、 **ADO.NET Entity Data Model**テンプレート. 新しいファイルに名前*AlumniAssociationModel.edmx*クリック**追加**します。

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

これは、Entity Data Model ウィザードを起動します。 **モデルのコンテンツの選択**手順で、**空のモデル**順にクリックします**完了**します。

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Entity Data Model デザイナー**空のデザイン画面を開きます。 ドラッグ、**エンティティ**項目から、**ツールボックス**デザイン サーフェイスにします。

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

エンティティの名前を変更`Entity1`に`Alumnus`、変更、`Id`プロパティ名を`AlumnusId`、という名前の新しいスカラー プロパティを追加および`Name`します。 新しいプロパティを追加する Enter を押しての名前を変更した後、`Id`列、またはエンティティを右クリックし、選択**スカラー プロパティの追加**します。 新しいプロパティの既定の種類は`String`、この簡単なデモについては、問題はありませんが、もちろんなどでのデータ型を変更、**プロパティ**ウィンドウ。

同様に、別のエンティティを作成し、名前`Donation`します。 変更、`Id`プロパティを`DonationId`という名前のスカラー プロパティを追加および`DateAndAmount`します。

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

これら 2 つのエンティティ間の関連付けを追加するを右クリックし、`Alumnus`エンティティで、**追加**、し、**アソシエーション**します。

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

既定値、**の関連付けの追加** ダイアログ ボックスが目的 (一対多はナビゲーション プロパティを含む、外部キーを含める、) をクリックするだけのため**ok**します。

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

デザイナーでは、関連行と外部キー プロパティを追加します。

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

データベースを作成する準備ができました。 デザイン サーフェスと選択を右クリックして**モデルからのデータベース生成**します。

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

これは、データベース生成ウィザードを起動します。 (エンティティがマップされていないことを示す警告が表示された場合は無視できます当面のものです。)

**データ接続の選択**クリック**新しい接続**します。

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

**接続プロパティ** ダイアログ ボックスで、ローカルの SQL Server Express インスタンスを選択し、データベースの名前`AlumniAsssociation`します。

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

をクリックして**はい**かどうかは、データベースを作成するときに尋ねします。 ときに、**データ接続の選択**手順がもう一度表示されたら、をクリックして**次**します。

**概要と設定**クリック**完了**。

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *.sql*データ定義言語 (DDL) コマンドを使用してファイルが作成されますが、コマンドはまだ実行されていません。

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

などのツールを使用して**SQL Server Management Studio**スクリプトを実行し、作成したときに完了する可能性がありますが、テーブルを作成する、`School`データベース[入門チュートリアル シリーズの最初のチュートリアル](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). 除きますするデータベースをダウンロードします。)

使用できるように、`AlumniAssociation`データ モデルで、web ページを使用したのと同様、`School`モデル。 これを試すには、いくつかのデータをテーブルに追加し、データを表示する web ページを作成します。

使用して**サーバー エクスプ ローラー**、次の行を追加、`Alumnus`と`Donation`テーブル。

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

という名前の新しい web ページを作成する*Alumni.aspx*を使用して、 *Site.Master*マスター ページ。 次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

このマークアップで入れ子になった作成`GridView`卒業生の名前を表示する外部の 1 つと寄付日付や金額の表示を内部のいずれかを制御します。

開いている*Alumni.aspx.cs*します。 追加、`using`ステートメント、データ アクセス層と、外側のハンドラー`GridView`コントロールの`RowDataBound`イベント。

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

このコードの完成内部`GridView`制御を使用して、`Donations`ナビゲーション プロパティの現在の行の`Alumnus`エンティティ。

ページを実行します。

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(注: ダウンロード可能なプロジェクトでこのページが含まれているがように動作する必要がありますでデータベースを作成、ローカルの SQL Server Express インスタンスは、データベースとして含まれません、 *.mdf*ファイル、*アプリ\_データ*フォルダー)。

Entity Framework のモデルの最初の機能の使用に関する詳細については、次を参照してください。 [Entity Framework 4 でモデルファースト](https://msdn.microsoft.com/data/ff830362.aspx)します。

## <a name="poco-support"></a>POCO サポート

ドメイン駆動設計の方法論を使用する場合は、データおよびビジネス ドメインに関連する動作を表すデータ クラスを設計します。 これらのクラスを格納するために使用、特定のテクノロジに依存しないにする必要があります (永続化) は、データつまり、必要がある*永続化が依存*します。 永続化非依存こともできますクラス単体テストを簡単にどのような永続化テクノロジでは、テストの最も便利な単体テスト プロジェクトを使用できます。 エンティティ クラスを継承する必要があるため、Entity Framework の以前のバージョンに永続化非依存の制限付きサポートが提供される、`EntityObject`クラスし、Entity Framework 固有の機能の多く含まれています。

Entity Framework 4 から継承しないエンティティ クラスを使用する機能が導入されています、`EntityObject`クラスし、そのため、永続化を依存します。 Entity Framework のコンテキストでこのようなクラスが通常は呼び出し*plain-old CLR object* (、POCO または Poco)。 POCO クラスは、手動で記述できるまたは Entity Framework によって提供されるテキスト テンプレート変換ツールキット (T4) テンプレートを使用して既存のデータ モデルに基づいてそれらを自動的に生成できます。

Entity Framework で Poco の使用に関する詳細については、次のリソースを参照してください。

- [POCO エンティティの操作](https://msdn.microsoft.com/library/dd456853.aspx)します。 これより詳しい情報を他のドキュメントへのリンクには、Poco の概要である MSDN ドキュメントです。
- [チュートリアル: POCO エンティティ フレームワークのテンプレート](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx)Poco に関するその他のブログの投稿へのリンクを Entity Framework 開発チームのブログ投稿です。

## <a name="code-first-development"></a>Code First の開発

Entity Framework 4 における POCO サポートは、引き続き、データ モデルを作成し、エンティティ クラスをデータ モデルにリンクする必要があります。 Entity Framework の次のリリースと呼ばれる機能が含まれます*code first 開発*します。 この機能では、データのモデル デザイナーまたはデータ モデルの XML ファイルを使用して、しなくても、独自の POCO クラスと Entity Framework を使用することができます。 (そのため、このオプションも呼び出された*コードのみ*;*code first*と*コードのみ*両方には、同じの Entity Framework 機能を参照してください)。

詳細については、開発の code first アプローチを使用して、次のリソースを参照してください。

- [Entity Framework 4 を使用した開発の code First](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx)します。 これは、code first 開発の概要 Scott Guthrie によるブログの投稿です。
- [Entity Framework 開発チームのブログ - 投稿のタグが付けられた CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Framework 開発チームのブログ - 投稿タグが付けられた Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC Music Store チュートリアル - パート 4: モデルとデータ アクセス](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [MVC 3 - パート 4 の概要: Entity Framework Code First の開発](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Contoso University アプリケーションと同様に、アプリケーションを構築する新しい Code First MVC チュートリアルは 2011 年春発行を予定してさらに、 [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>説明

これは、新しい Entity Framework と Entity Framework のチュートリアル シリーズで、この継続的には、その概要を完了します。 ここで対象にならない Entity Framework 4 の新機能についての詳細については、次のリソースを参照してください。

- [ADO.NET で新](https://msdn.microsoft.com/library/ex6y04yf.aspx)Entity Framework のバージョン 4 の新機能に関する MSDN トピック。
- [Entity Framework 4 のリリースを発表](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx)バージョン 4 の新機能についての Entity Framework 開発チームのブログ投稿。

> [!div class="step-by-step"]
> [前へ](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
