---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Entity Framework 4.0 ASP.NET 4 Web アプリケーションでの同時実行の処理 |Microsoft ドキュメント
author: tdykstra
description: この一連のチュートリアルについては、Entity Framework 4.0 チュートリアル シリーズの概要を作成した Contoso 大学 web アプリケーションに基づいています。 I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: f40695270006e4f8b0c9ad8e94049e5239f06e63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Entity Framework 4.0 ASP.NET 4 Web アプリケーションでの同時実行の処理
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズのビルドによって作成される Contoso 大学 web アプリケーションで、 [Entity Framework 4.0 の概要](https://asp.net/entity-framework/tutorials#Getting%20Started)一連のチュートリアルです。 前のチュートリアルを完了していない場合このチュートリアルの開始点とすることができます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)作成したとします。 こともできます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)一連の完全なチュートリアルで作成します。 チュートリアルについて質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)です。


前のチュートリアルでは、並べ替え方法およびフィルターを使用してデータを説明しました、`ObjectDataSource`コントロールおよび Entity Framework。 このチュートリアルでは、Entity Framework を使用する ASP.NET web アプリケーションで同時実行を処理するためのオプションを使用します。 講師 office の割り当てを更新する専用の新しい web ページを作成します。 そのページで、および先ほど作成した部門ページで、同時実行の問題を処理します。

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>同時実行の競合

同時実行の競合は、1 人のユーザーがレコードを編集し、最初のユーザーの変更がデータベースに書き込まれる前に、別のユーザーが同じレコードを編集するときに発生します。 Entity Framework このような競合を検出するように設定しない場合、その他のユーザーの変更によって上書きされますだれでもデータベースを更新します。 多くのアプリケーションでは、このリスクは、許容と可能な同時実行の競合を処理するアプリケーションを構成する必要はありません。 (一部のユーザー、またはいくつかの更新プログラムがある場合、または場合いない本当に重要な場合は、いくつかの変更が上書きされると、同時実行のプログラミングのコストがのメリットを上回る可能性があります)。同時実行の競合について心配する必要としない場合は、このチュートリアルでは; を省略できます。系列の残りの 2 つのチュートリアルは、この 1 で作成済みのアイテムに依存しません。

### <a name="pessimistic-concurrency-locking"></a>ペシミスティック同時実行制御 (ロック)

同時実行で偶発的にデータが失われる事態をアプリケーションで回避する必要があれば、その方法としてデータベース ロックがあります。 これと呼ばれる*ペシミスティック同時実行制御*です。 たとえば、データベースから行を読む前に、読み取り専用か更新アクセスでロックを要求します。 更新アクセスで行をロックすると、他のユーザーはその行を読み取り専用または更新アクセスでロックできなくなります。変更中のデータのコピーが与えられるためです。 読み取り専用で行をロックすると、他のユーザーはその行を読み取り専用でロックできますが、更新アクセスではロックできません。

ロックの管理と、いくつかの欠点があります。 プログラムが複雑になります。 必要な管理リソースは大量のデータベース、およびアプリケーションのユーザーの数とパフォーマンスの問題を引き起こす可能性が増加 (つまり、適切にスケールされない)。 そのような理由から、一部のデータベース管理システムはペシミスティック同時実行制御に対応していません。 Entity Framework には、組み込みのサポートはありません、このチュートリアルは示してそれを実装する方法。

### <a name="optimistic-concurrency"></a>オプティミスティック同時実行制御

ペシミスティック同時実行する代わりに、*オプティミスティック同時実行制御*です。 オプティミスティック同時実行制御では、同時実行の競合の発生を許し、発生したら適切に対処します。 John はたとえば、実行、 *Department.aspx*ページで、クリック、**編集**履歴部門用のリンクし、削減、**予算**$ $1,000,000.00 から量125,000.00 です。 (John の競合する部門の管理し、コストの自分の部門を解放する必要がある)。

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

John が前に**更新**、加藤さんは同じページを実行するをクリックする、**編集**履歴部門や、変更のリンク、 **Start Date** 2011 年 1 月 10 日からフィールドを 1/1/1999 です。 (Jane の履歴部門の管理し、複数勤続を付与する必要がある)。

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John が**更新**最初に、加藤さんクリックして**更新**です。 ジェーンのブラウザーの現在のリスト、**予算**金額が $125,000.00 に John が変更されたためドル 1,000,000.00 がこの金額が正しくありません。

このシナリオで実行できる操作の一部を以下に示します。

- ユーザーが変更したプロパティを追跡記録し、それに該当する列だけをデータベースで更新できます。 例のシナリオでは、2 人のユーザーが異なるプロパティを更新したため、データは失われません。 次回履歴部門を参照する他のユーザーと、1/1/1999 が表示およびドル 125,000.00 です。 

    これは、Entity Framework の既定の動作であり、データが失われる可能性のある競合の数を大幅に短縮することができます。 ただし、この動作は、同じエンティティのプロパティに競合する変更が加えられた場合、データ損失を回避しません。 さらに、この動作は必ずしも可能です。ストアド プロシージャをエンティティ型にマップするときに、エンティティへの変更は、データベースに加えられたときにすべてのエンティティのプロパティが更新されます。
- John の変更が上書きジェーンの変更することができます。 加藤さんがクリックした後**更新**、**予算**金額が $1,000,000.00 に戻ります。 これは *Client Wins* (クライアント側に合わせる) シナリオまたは *Last in Wins* (最終書き込み者優先) シナリオと呼ばれています。 (クライアントの値よりも優先、データ ストアには、新機能です。)
- ジェーンの変更は、データベースで更新されているを妨害することができます。 通常、エラー メッセージが表示、データの現在の状態を表示するユーザー、および彼女ようにする必要がある場合は、自分が加えた変更を再入力できるようにはします。 ユーザー入力を保存し、それを再入力しなくても再適用する機会を与える彼女プロセスを自動化することがさらにします。 これは *Store Wins* (ストア側に合わせる) シナリオと呼ばれています。 (クライアントが送信した値よりデータストアの値が優先されます。)

### <a name="detecting-concurrency-conflicts"></a>同時実行の競合を検出します。

Entity Framework では、処理することにより競合を解決できる`OptimisticConcurrencyException`Entity Framework がスローする例外。 このような例外がスローされるタイミングを認識する目的で、Entity Framework は競合を検出できなければなりません。 そのため、データベースとデータ モデルを適宜構成する必要があります。 競合検出を有効にするためのオプションには次のようなものがあります。

- データベースには特定の行が変更されたときに使用できるテーブルの列が含まれます。 その列を含めるに Entity Framework を構成することができますし、 `Where` SQL の句`Update`または`Delete`コマンド。

    目的は、`Timestamp`内の列、`OfficeAssignment`テーブル。

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    データ型、`Timestamp`列とも呼びます`Timestamp`です。 ただし、列は、日付または時刻の値を実際には含んでいません。 代わりに、値は、行が更新されるたびにインクリメントが連続番号です。 `Update`または`Delete`コマンド、`Where`句に含まれる元`Timestamp`値。 別のユーザーの値によって、更新された行が変更された場合`Timestamp`元の値とは異なるため、`Where`句が更新する行を返しません。 Entity Framework が現在の行が更新されてないことを見つけたとき`Update`または`Delete`コマンドの (つまり、影響を受けた行の数が 0 である場合)、同時実行の競合として解釈します。
- テーブルのすべての列の元の値を含める Entity Framework の構成、`Where`の句`Update`と`Delete`コマンド。

    最初のオプションの場合は、行の任意のものが変更されたため、行が読み取られた最初のように、`Where`句しない行が返されます、更新するには、Entity Framework は同時実行の競合として解釈します。 このメソッドは効率的に行えるようにを使用して、`Timestamp`フィールドでは、効率的なことができます。 多くの列を持つデータベース テーブルでは、する可能性が非常に大きな`Where`句、web アプリケーションで、必要と大量の状態を維持することです。 大量の状態を保持することと、サーバー リソース (たとえば、セッション状態) が必要か web ページ自体 (たとえば、ビュー状態) に含める必要があるためにアプリケーションのパフォーマンスが影響することができます。

このチュートリアルでは、追跡プロパティがエンティティの競合をオプティミスティック同時実行のエラー処理を追加します (、`Department`エンティティ) とは、追跡プロパティがエンティティの (、`OfficeAssignment`エンティティ)。

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>追跡プロパティを持たないオプティミスティック同時実行の処理

オプティミスティック同時実行制御を実装する、`Department`追跡用のないエンティティ (`Timestamp`) プロパティでは、次のタスクを行います。

- 同時実行の追跡を有効にするデータ モデルが変更される`Department`エンティティです。
- `SchoolRepository`クラスでの同時実行例外の処理、`SaveChanges`メソッドです。
- *Departments.aspx*  ページで、ユーザーを変更しようとしたが成功したことを警告するメッセージを表示して同時実行例外を処理します。 ユーザーの現在の値を表示し、それらが必要である場合は、変更を再試行してくださいします。

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>同時実行データ モデルでの追跡を有効にします。

Visual Studio で、このシリーズの前のチュートリアルで作業していたを Contoso 大学 web アプリケーションを開きます。

開いている*SchoolModel.edmx*、データ モデル デザイナーで右クリックして、`Name`プロパティに、`Department`エンティティをクリックして**プロパティ**です。 **プロパティ**ウィンドウで、変更、`ConcurrencyMode`プロパティを`Fixed`です。

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

でも同様に、他の主キーの非スカラー プロパティ (`Budget`、 `StartDate`、および`Administrator`)。(できませんこれを行うのナビゲーション プロパティ。)これが指定されるたびに、Entity Framework によって生成された、`Update`または`Delete`を更新する SQL コマンド、`Department`データベース内のエンティティ、(元の値) のこれらの列で含める必要がある、`Where`句。 場合は行が見つからない場合に、`Update`または`Delete`コマンドが実行されると、Entity Framework では、オプティミスティック同時実行例外をスローします。

保存して、データ モデルを閉じます。

### <a name="handling-concurrency-exceptions-in-the-dal"></a>DAL で同時実行例外の処理

開いている*SchoolRepository.cs*し、以下の追加`using`のステートメント、`System.Data`名前空間。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

新しい次の追加`SaveChanges`オプティミスティック同時実行例外を処理するメソッド。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

このメソッドが呼び出されたときに、同時実行エラーが発生した場合、メモリ内のエンティティのプロパティの値は、データベースの現在の値に置き換えられます。 Web ページが処理できるように、同時実行例外が再度スローされます。

`DeleteDepartment`と`UpdateDepartment`メソッド、既存の呼び出しを置き換える`context.SaveChanges()`への呼び出しに`SaveChanges()`新しいメソッドを呼び出すためにします。

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>プレゼンテーション層で、同時実行例外の処理

開いている*Departments.aspx*を追加し、`OnDeleted="DepartmentsObjectDataSource_Deleted"`属性を`DepartmentsObjectDataSource`コントロール。 コントロールの開始タグは、次の例のようになります。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

`DepartmentsGridView`コントロールをすべてのテーブル列の指定、`DataKeyNames`属性が次の例で示すようにします。 メモをこれが作成されます非常に大きなビュー状態フィールド理由の 1 つは追跡フィールドを使用する理由は通常、同時実行の競合を追跡することをお勧めです。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

開いている*Departments.aspx.cs*し、以下の追加`using`のステートメント、`System.Data`名前空間。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

データ ソース コントロールの呼び出しは次の新しいメソッドを追加`Updated`と`Deleted`同時実行例外を処理するためのイベント ハンドラー。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

このコードは、例外の種類を確認し、コードを動的に作成、同時実行例外がある場合、`CustomValidator`順番でメッセージを表示するコントロール、`ValidationSummary`コントロール。

新しいメソッドを呼び出して、`Updated`前に追加するイベント ハンドラー。 さらに、新しい作成`Deleted`イベント ハンドラーが同じメソッドを呼び出します (その他の何もしません)。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>部門 ページでオプティミスティック同時実行のテスト

実行、 *Departments.aspx*ページ。

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

をクリックして**編集**行の値を変更し、**予算**列です。 (ために、のみこのチュートリアルで作成したレコードを編集することに注意してください。 既存の`School`データベース レコードに無効なデータが含まれています。 経済性部門のレコードは、安全なを試す。)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

新しいブラウザー ウィンドウを開き、ページをもう一度 (2 番目のブラウザーのウィンドウに最初のブラウザー ウィンドウの [アドレス] ボックスから URL をコピーする) を実行します。

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

をクリックして**編集**、同じ行の以前に編集して変更、**予算**の別の値。

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

2 番目のブラウザー ウィンドウで **更新**です。 **予算**量が正常にこの新しい値に変更されました。

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

最初のブラウザー ウィンドウで **更新**です。 更新プログラムは失敗します。 **予算**2 番目のブラウザー ウィンドウで設定した値を使用して、金額が表示され、エラー メッセージを参照してください。

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>追跡プロパティを使用してオプティミスティック同時実行の処理

追跡プロパティを持つエンティティのオプティミスティック同時実行制御を処理するには、次のタスクを行います。

- ストアド プロシージャをモデルに追加データを管理する`OfficeAssignment`エンティティです。 (追跡のプロパティおよびストアド プロシージャは、組み合わせて使用することがありません。 はだけ分類されています一緒に次の図の)
- CRUD メソッドを追加、DAL との BLL `OfficeAssignment` DAL でオプティミスティック同時実行例外を処理するコードを含むエンティティです。
- Office 割り当て web ページを作成します。
- 新しい web ページで、オプティミスティック同時実行制御をテストします。

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>ストアド プロシージャ、データ モデルを OfficeAssignment を追加します。

開く、 *SchoolModel.edmx*モデル デザイナーでファイルをデザイン画面を右クリックし、をクリックして**データベースからモデルを更新**です。 **追加**のタブ、**データベース オブジェクトの選択** ダイアログ ボックスで、展開**Stored Procedures** 、3 つを選択し、`OfficeAssignment`ストアド プロシージャ (を参照してください、次のスクリーン ショット)、をクリックして**完了**です。 (これらのストアド プロシージャが既にデータベースにダウンロードまたはスクリプトを使用してそれを作成するときにします。)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

右クリックし、`OfficeAssignment`エンティティと選択**ストアド プロシージャ マッピング**です。

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

設定、**挿入**、**更新**、および**削除**ストアド プロシージャを対応するを使用する関数。 `OrigTimestamp`のパラメーター、`Update`機能、設定、**プロパティ**に`Timestamp`を選択し、**元の値を使用して**オプション。

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Entity Framework を呼び出すと、`UpdateOfficeAssignment`ストアド プロシージャの元の値を渡すことは、`Timestamp`内の列、`OrigTimestamp`パラメーター。 ストアド プロシージャで、このパラメーターを使用してその`Where`句。

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

ストアド プロシージャもの新しい値を選択、`Timestamp`更新後の列、Entity Framework を維持できるように、`OfficeAssignment`との同期、対応するデータベースの行をメモリになっているエンティティ。

(注にオフィス割り当てを削除するためのストアド プロシージャがない、`OrigTimestamp`パラメーター。 このため、Entity Framework を検証できませんエンティティが削除する前にも維持されます。)

保存して、データ モデルを閉じます。

### <a name="adding-officeassignment-methods-to-the-dal"></a>Dal OfficeAssignment メソッドの追加

開いている*ISchoolRepository.cs* CRUD の次のメソッドを追加し、`OfficeAssignment`エンティティ セット。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

次の新しいメソッドを追加*SchoolRepository.cs*です。 `UpdateOfficeAssignment`メソッド、ローカルの呼び出しを`SaveChanges`メソッドの代わりに`context.SaveChanges`です。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

テスト プロジェクトで開きます*MockSchoolRepository.cs*し、以下の追加`OfficeAssignment`コレクションと CRUD メソッドです。 (モック リポジトリは、リポジトリ インターフェイスを実装する必要があります。 またはソリューションがコンパイルされません)。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>BLL に OfficeAssignment メソッドの追加

メインのプロジェクトで開きます*SchoolBL.cs* CRUD の次のメソッドを追加し、`OfficeAssignment`エンティティを設定するには。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>OfficeAssignments Web ページを作成します。

使用する新しい web ページを作成、 *Site.Master*マスター ページと名前を付けます*OfficeAssignments.aspx*です。 次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

注意、`DataKeyNames`属性のマークアップを指定します、`Timestamp`プロパティだけでなく、レコードのキー (`InstructorID`)。 プロパティを指定する、`DataKeyNames`属性により制御状態 (状態を表示すると同様です) に保存するコントロール ポストバックの処理中に元の値が使用できるようにします。

保存しなかった場合、`Timestamp`値、Entity Framework ができないため、 `Where` sql 句`Update`コマンド。 その結果を更新する何も見つかりませんなります。 その結果、Entity Framework をスローした場合、オプティミスティック同時実行例外たびに、`OfficeAssignment`エンティティを更新します。

開いている*OfficeAssignments.aspx.cs*し、以下の追加`using`データ アクセス層のステートメント。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

次の追加`Page_Init`Dynamic Data 機能を有効にするメソッド。 また、次のハンドラーを追加、`ObjectDataSource`コントロールの`Updated`同時実行エラーを確認するためにイベント。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>OfficeAssignments ページでオプティミスティック同時実行のテスト

実行、 *OfficeAssignments.aspx*ページ。

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

をクリックして**編集**行内の値を変更し、**場所**列です。

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

新しいブラウザー ウィンドウを開き、ページをもう一度 (2 番目のブラウザーのウィンドウに、最初のブラウザー ウィンドウから URL をコピーする) を実行します。

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

をクリックして**編集**、同じ行の以前に編集して変更、**場所**の別の値。

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

2 番目のブラウザー ウィンドウで **更新**です。

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

最初のブラウザー ウィンドウに切り替えるし、をクリックして**更新**です。

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

エラー メッセージが表示され、**場所**に 2 つ目のブラウザーのウィンドウで変更した値を表示する値が更新されました。

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>EntityDataSource コントロールでの同時実行の処理

`EntityDataSource`コントロールには、データ モデルの同時実行設定を認識する組み込みのロジックが含まれています。 ハンドルの更新としてそれに応じて操作を削除します。 ただしと同様に、すべての例外を処理するあります`OptimisticConcurrencyException`例外、わかりやすいエラー メッセージを提供するために自分でします。

次に、構成は、 *Courses.aspx*ページ (が使用される、`EntityDataSource`コントロール) を更新を許可し、削除操作と同時実行の競合が発生した場合、エラー メッセージを表示します。 `Course`エンティティで行ったのと同じ方法を使用するように列を追跡する同時実行はありません、`Department`エンティティ: キー以外のすべてのプロパティの値を追跡します。

開く、 *SchoolModel.edmx*ファイル。 非キー プロパティの`Course`エンティティ (`Title`、 `Credits`、および`DepartmentID`)、設定、**同時実行モード**プロパティを`Fixed`です。 保存して、データ モデルを終了します。

開く、 *Courses.aspx*ページし、次の変更を加えます。

- `CoursesEntityDataSource`コントロールを追加`EnableUpdate="true"`と`EnableDelete="true"`属性。 制御するための開始タグには、次の例がようになります。

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- `CoursesGridView`制御、変更、`DataKeyNames`属性に値`"CourseID,Title,Credits,DepartmentID"`です。 追加し、`CommandField`要素を`Columns`要素を示す**編集**と**削除**ボタン (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`)。 `GridView`コントロール次の例のようになります。

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

ページを実行し、[部門] ページで以前に実行したように、競合状況を作成します。 2 つのブラウザー ウィンドウで、ページの実行で、**編集**と同じで、各ウィンドウの行し、1 つずつでさまざまな変更を行います。 をクリックして**更新**をクリックして 1 つのウィンドウ**更新**他のウィンドウでします。 クリックすると**更新**2 回目、未処理の同時実行例外に起因するエラー ページが表示されます。

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

処理方法に類似した方法でこのエラーを処理する、`ObjectDataSource`コントロール。 開く、 *Courses.aspx* ] ページで、し、[、`CoursesEntityDataSource`コントロール用のハンドラーを指定、`Deleted`と`Updated`イベント。 コントロールの開始タグには、次の例がようになります。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

前に、`CoursesGridView`コントロールを次の追加`ValidationSummary`コントロール。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

*Courses.aspx.cs*、追加、`using`のステートメント、`System.Data`名前空間を選択し、同時実行例外をチェックするメソッドを追加のハンドラーを追加、`EntityDataSource`コントロールの`Updated`と`Deleted`ハンドラー。 コードは、次のようになります。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

このコードと何の唯一の違い、`ObjectDataSource`コントロールは、ここでは、同時実行例外で、`Exception`その例外のではなく、イベント引数オブジェクトのプロパティ`InnerException`プロパティです。

ページを実行し、同時実行の競合をもう一度作成します。 この時間は、エラー メッセージが表示します。

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

同時実行の競合処理の入門編はこれで終わりです。 次のチュートリアルでは、Entity Framework を使用する web アプリケーションのパフォーマンスを向上する方法に関するガイダンスを提供します。

> [!div class="step-by-step"]
> [前へ](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [次へ](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
