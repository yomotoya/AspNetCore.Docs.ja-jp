---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: "Entity Framework 4.0 の新機能 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルについては、Entity Framework 4.0 チュートリアル シリーズの概要を作成した Contoso 大学 web アプリケーションに基づいています。 私。。。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 4c89ca004ad4c9d731868e868cf6723aa4ed625d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-the-entity-framework-40"></a>Entity Framework 4.0 の新機能
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズのビルドによって作成される Contoso 大学 web アプリケーションで、 [Entity Framework の概要](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)一連のチュートリアルです。 前のチュートリアルを完了していない場合このチュートリアルの開始点とすることができます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)作成したとします。 こともできます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)一連の完全なチュートリアルで作成します。 チュートリアルについて質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)です。


前のチュートリアルでは、Entity Framework を使用する web アプリケーションのパフォーマンスを最大化するための一部のメソッドを確認しました。 このチュートリアルは、Entity Framework のバージョン 4 の最も重要な新機能のいくつかを確認し、すべての新しい機能をより詳細な概要を提供しているリソースにリンクします。 このチュートリアルで強調表示されている機能を以下に示します。

- 外部キーの関連付け。
- ユーザー定義の SQL コマンドを実行しています。
- モデル優先の開発。
- POCO サポートします。

さらに、このチュートリアルは導入について簡単に*コード優先の開発*、Entity Framework の次のリリースで読み込まれている機能です。

チュートリアルを開始するには、Visual Studio を起動し、前のチュートリアルで作業していた Contoso 大学 web アプリケーションを開きます。

## <a name="foreign-key-associations"></a>外部キーの関連付け

Entity Framework のバージョン 3.5 にナビゲーション プロパティが含まれていますが、データ モデルに外部キー プロパティを含めることがありませんでした。 たとえば、`CourseID`と`StudentID`の列、`StudentGrade`から省略されるテーブル、`StudentGrade`エンティティです。

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

このアプローチの理由でした、厳密には、外部キー、物理的な実装の詳細は、概念データ モデルに属していません。 ただし、実際には、外部キーに直接アクセスがある場合、コード内のエンティティと処理を容易には多くの場合です。

データ モデル内のどの外部キーの例では、コードを簡略化できますの検討方法がありましたコードに、 *DepartmentsAdd.aspx*せずにページ。 `Department` 、エンティティ、`Administrator`プロパティに対応する外部キーは、`PersonID`で、`Person`エンティティです。 新しい部門とその管理者間の関連付けを確立するための値を設定すべてを行う必要がありましたが、`Administrator`プロパティに、`ItemInserting`データ バインド コントロールのイベント ハンドラー。

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

処理とデータ モデルの外部キーを持たない、`Inserting`の代わりに、データ ソース コントロールのイベント、`ItemInserting`エンティティがエンティティ セットに追加される前に、エンティティ自体への参照を取得するために、データ バインド コントロールのイベントです。 その参照がある場合は、次の例で、そのようなコードを使用してアソシエーションを確立します。

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Entity Framework チームのことがわかります[外部キーの関連付けのブログの投稿](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx)コードの複雑さの差が非常に大きいその他の場合があります。 ここでは単純なコードの概念データ モデルでの実装の詳細と有効期限が好きな方のニーズを満たすために、Entity Framework ようになりましたデータ モデルのような外部キーのオプションです。

Entity Framework の用語で、データ モデルに外部キーを含める場合は使用している*外部キーの関連付け*を使用している外部キーを除外する場合と*独立した関連付け*です。

## <a name="executing-user-defined-sql-commands"></a>ユーザー定義の SQL コマンドを実行します。

Entity Framework の以前のバージョンでは、実行時に、独自の SQL コマンドを作成し、それらを実行する簡単な方法はありませんでした。 Entity Framework は、SQL コマンドを動的に生成するか、ストアド プロシージャを作成し、関数としてインポートする必要がありました。 バージョン 4 が追加されて`ExecuteStoreQuery`と`ExecuteStoreCommand`メソッド、`ObjectContext`任意のクエリをデータベースに直接渡すための簡略化するクラス。

Contoso 大学管理者ができるストアド プロシージャを作成して、データ モデルにインポートの過程を通過することがなく、データベースで一括変更を実行するようにするとします。 最初の要求では、データベース内のすべてのコースのクレジットの数を変更することができ、ページのです。 使用しての値を乗算する数を入力できないようにする web ページ上すべて`Course`行の`Credits`列です。

使用する新しいページを作成、 *Site.Master*マスター ページと名前を付けます*UpdateCredits.aspx*です。 次に示すマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

このマークアップを作成、`TextBox`乗数値を入力できるコントロール、`Button`コマンドを実行するためにクリックするコントロールと`Label`影響を受ける行の数を示すためのコントロールです。

開いている*UpdateCredits.aspx.cs*、次の追加と`using`ステートメントと、ボタンのハンドラー`Click`イベント。

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

このコードは実行 SQL`Update`コマンドのテキスト ボックスに値を使用して、ラベルを使用して、影響を受ける行の数を表示します。 ページを実行する前に、実行、 *Courses.aspx*に把握する"before"一部のデータ ページです。

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

実行*UpdateCredits.aspx*の乗数として「10」を入力し、、クリック**Execute**です。

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

実行、 *Courses.aspx*ページをもう一度変更されたデータを参照してください。

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(で、元の値に戻すクレジットの数を設定する場合*UpdateCredits.aspx.cs*変更`Credits * {0}`に`Credits / {0}`除数として 10 を入力する、ページを再実行します)。

コードで定義するクエリの実行の詳細については、次を参照してください。[する方法: 直接実行コマンドに対して、データ ソース](https://msdn.microsoft.com/en-us/library/ee358769.aspx)です。

## <a name="model-first-development"></a>モデル優先の開発

これらのチュートリアルでは、まず、データベースを作成し、データベース構造に基づくデータ モデルを生成します。 Entity Framework 4 では、代わりに、データ モデルで開始し、データ モデルの構造に基づいて、データベースを生成できます。 モデル優先のアプローチを使用すると、エンティティと物理的な実装の詳細を心配ありません中に、アプリケーションの概念的には意味のあるリレーションシップの作成、データベースが既に存在するアプリケーションを作成する場合. (これは、開発の初期段階でのみ true ただしです。 最終的に、データベースが作成されますが、実稼働データを保有し、モデルから再作成することはできなく実用的です。その時点ですることになります戻るデータベース優先のアプローチです。)

チュートリアルのこのセクションでを単純なデータ モデルを作成し、そこから、データベースを生成します。

**ソリューション エクスプ ローラー**を右クリックし、 *DAL*フォルダーと選択**新しい項目の追加**です。 **新しい項目の追加**ダイアログ ボックスで、**インストールされたテンプレート**選択**データ**し、選択、 **ADO.NET エンティティ データ モデル**テンプレート. 新しいファイルの名前*AlumniAssociationModel.edmx*  をクリック**追加**です。

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

これは、Entity Data Model ウィザードを起動します。 **[モデルのコンテンツ**手順で、**空のモデル**] をクリックし、**完了**です。

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Entity Data Model Designer**空のデザイン画面を開きます。 ドラッグ、**エンティティ**項目を**ツールボックス**デザイン サーフェイスにします。

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

エンティティ名を変更`Entity1`に`Alumnus`、変更、`Id`プロパティ名を`AlumnusId`、という名前の新しいスカラー プロパティを追加および`Name`です。 新しいプロパティを追加して Enter キーを押すの名前を変更した後、`Id`列、またはエンティティを右クリックし **スカラー プロパティの追加**です。 新しいプロパティの既定の種類は`String`、この簡単なデモについては、問題はありません。 これは、もちろんでのデータ型などを変更することができます、**プロパティ**ウィンドウです。

同じ方法で別のエンティティを作成し、名前を付けます`Donation`です。 変更、`Id`プロパティを`DonationId`という名前のスカラー プロパティを追加および`DateAndAmount`です。

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

これら 2 つのエンティティ間の関連付けを追加するを右クリックして、`Alumnus`エンティティで、**追加**、し、**アソシエーション**です。

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

値が既定値、**の関連付けを追加** ダイアログ ボックスが希望の新機能 (一対多、ナビゲーション プロパティを含む、外部キーが含まれます) をクリックするだけのため**ok**です。

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

デザイナーは、関連行と外部キー プロパティを追加します。

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

これで、データベースを作成する準備ができました。 デザイン画面と選択を右クリックして**モデルからのデータベース生成**です。

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

これは、データベース生成ウィザードを起動します。 (エンティティがマップされていないことを示す警告を表示する場合は無視できます当面のものです。)

**データ接続の選択**ステップで、**新しい接続**です。

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

**接続プロパティ** ダイアログ ボックスで、ローカルの SQL Server Express インスタンスを選択し、データベースの名前`AlumniAsssociation`です。

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

をクリックして**はい**データベースを作成するように求められます。 ときに、**データ接続の選択**ステップが再度表示されます をクリックして**次**です。

**の概要と設定**ステップで、**完了**です。

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *.sql*データ定義言語 (DDL) コマンドを使用してファイルを作成すると、コマンドをまだ実行されていません。

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

などのツールを使用して**SQL Server Management Studio**スクリプトを実行し、作成したときに完了したら可能性がありますが、テーブルを作成する、`School`データベース[概要チュートリアル シリーズの最初のチュートリアル](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (しない限り、データベースをダウンロードします。)

使用できるように、`AlumniAssociation`データ モデルで、web ページを使用したのと同様、`School`モデル。 これを試すには、一部のデータをテーブルに追加し、データを表示する web ページを作成します。

使用して**サーバー エクスプ ローラー**、次の行を追加、`Alumnus`と`Donation`テーブル。

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

という名前の新しい web ページを作成する*Alumni.aspx*を使用して、 *Site.Master*マスター ページ。 次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

このマークアップで入れ子になった作成`GridView`外側の卒業生の名前を表示して、内部 donation 日付や金額を表示するを制御します。

開いている*Alumni.aspx.cs*です。 追加、`using`ステートメント、データ アクセス レイヤーと、外側のハンドラー`GridView`コントロールの`RowDataBound`イベント。

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

このコード完成内部`GridView`制御を使用して、`Donations`ナビゲーション プロパティの現在の行の`Alumnus`エンティティです。

ページを実行します。

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(注: このページは、ダウンロード可能なプロジェクトに含まれるが機能するためにする必要がありますでデータベースを作成、ローカルの SQL Server Express インスタンス; データベースとして含まれています、 *.mdf*ファイルで、*アプリ\_データ*フォルダーです)。

詳細については、Entity Framework のモデル優先機能を使用して、次を参照してください。 [Entity Framework 4 でモデル優先](https://msdn.microsoft.com/en-us/data/ff830362.aspx)です。

## <a name="poco-support"></a>POCO サポート

ドメイン ベースの設計方法を使用する場合、データとは、ビジネス ドメインに関連する動作を表すデータ クラスをデザインします。 これらのクラスは独立して、特定のテクノロジを格納するために使用する必要があります (保持) データです。つまり、必要があります*永続性を意識*です。 持続性の無視が適用もやすくクラス単体テスト、単体テスト プロジェクトはどのような永続化テクノロジはテストの最も便利な方法を使用できるためです。 継承するエンティティ クラスがあったために、Entity Framework の以前のバージョンが持続性の無視が適用の制限付きサポートを提供、`EntityObject`クラスし、含まエンティティ フレームワーク固有の機能が大幅に向上します。

Entity Framework 4 から継承しないエンティティ クラスを使用する機能が導入されています、`EntityObject`クラスおよび永続性を意識はそのためです。 Entity Framework のコンテキストで次のようなクラスと通常呼ばれる*従来の CLR オブジェクト*(POCO、またはした poco から)。 POCO クラスを手動で作成することができます、または Entity Framework によって提供されるテキスト テンプレート変換ツールキット (T4) テンプレートを使用して既存のデータ モデルに基づいてそれらを自動的に生成できます。

Entity Framework ではした poco からの使用に関する詳細については、次のリソースを参照してください。

- [POCO エンティティの操作](https://msdn.microsoft.com/en-us/library/dd456853.aspx)です。 これより詳しい情報を他のドキュメントへのリンクをした poco の概要については、MSDN ドキュメントです。
- [チュートリアル: POCO Entity Framework 用のテンプレート](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx)した poco からに関するその他のブログの投稿へのリンクと共に、Entity Framework 開発チームのブログの投稿です。

## <a name="code-first-development"></a>コード優先の開発

Entity Framework 4 で POCO サポートには、データ モデルを作成し、エンティティ クラスをデータ モデルにリンクすることも必要があります。 Entity Framework の次のリリースと呼ばれる機能が含まれます*コード優先の開発*です。 この機能では、データ モデル デザイナーまたはデータ モデルの XML ファイルを使用することがなく POCO クラスと Entity Framework を使用することができます。 (そのため、このオプションも呼び出されて*コードのみ*です。*コード優先*と*コードのみ*両方とも同じ Entity Framework 機能を参照してください)。

詳細については、開発コード優先のアプローチを使用して、次のリソースを参照してください。

- [コードの最初の Entity Framework 4 を使用した開発](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx)です。 これは、ブログ投稿 Scott Guthrie 紹介コード優先の開発でです。
- [Entity Framework 開発チームのブログの投稿タグが付けられた CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Framework 開発チームのブログの投稿タグが付けられた Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [MVC Music Store チュートリアル - パート 4: モデルおよびデータ アクセス](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [はじめに mvc 3 - パート 4: Entity Framework コード優先の開発](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

2011 年のスプリングで発行する Contoso 大学アプリケーションと同様、アプリケーションを構築する新しい MVC コードの最初のチュートリアルを射影するさらに、 [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>説明

これは、新機能、Entity Framework と Entity Framework チュートリアル シリーズを続行するとこのするための概要を完了します。 ここで取り上げていないする Entity Framework 4 の新機能の詳細については、次のリソースを参照してください。

- [ADO.NET の新](https://msdn.microsoft.com/en-us/library/ex6y04yf.aspx)Entity Framework のバージョン 4 の新機能に関する MSDN トピックです。
- [Entity Framework 4 のリリースを発表](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx)バージョン 4 の新機能についての Entity Framework 開発チームのブログの投稿。

>[!div class="step-by-step"]
[前へ](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
