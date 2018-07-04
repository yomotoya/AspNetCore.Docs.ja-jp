---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: ASP.NET 4 Web アプリケーションで Entity Framework 4.0 での同時実行の処理 |Microsoft Docs
author: tdykstra
description: このチュートリアル シリーズでは、Entity Framework 4.0 のチュートリアル シリーズの概要を作成した Contoso University web アプリケーションに基づいています。 ここには.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: c22f06e68b15d3fbf13e9f2af0e19631abe076ec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392738"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>ASP.NET 4 Web アプリケーションで Entity Framework 4.0 での同時実行の処理
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズは、Contoso University web アプリケーションによって作成される、 [、Entity Framework 4.0 の概要](https://asp.net/entity-framework/tutorials#Getting%20Started)チュートリアル シリーズです。 前のチュートリアルを完了していない場合は、このチュートリアルの開始点としてできます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)に、作成します。 できます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完全なチュートリアル シリーズで作成します。 チュートリアルについて質問等がございましたらを投稿できます、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)します。


前のチュートリアルを並べ替える方法およびデータを使用してフィルターを学習しました、`ObjectDataSource`コントロールと Entity Framework。 このチュートリアルでは、Entity Framework を使用する ASP.NET web アプリケーションでの同時実行を処理するためのオプションを使用します。 インストラクターのオフィスの割り当てを更新するのには専用の新しい web ページを作成します。 そのページで、先ほど作成した部門 ページで、同時実行の問題を処理します。

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>同時実行の競合

同時実行の競合は、1 人のユーザーがレコードを編集し、最初のユーザーの変更がデータベースに書き込まれる前に、別のユーザーが同じレコードを編集するときに発生します。 Entity Framework をこのような競合を検出するように設定しない場合では、他のユーザーの変更は上書き最後にデータベースを更新します。 多くのアプリケーションでこのようなリスクが許容されると、可能な同時実行の競合を処理するためにアプリケーションを構成する必要はありません。 (いくつかのユーザー、またはいくつかの更新がある場合、または場合いない本当に重要な場合は、いくつかの変更が上書きされると、同時実行プログラミングのコストがメリットを上回る可能性があります)。同時実行の競合について心配する必要はありませんが場合は、このチュートリアルでは; を省略できます。残りの 2 つのチュートリアル シリーズでは、この 1 で作成したものについてに依存しません。

### <a name="pessimistic-concurrency-locking"></a>ペシミスティック同時実行制御 (ロック)

同時実行で偶発的にデータが失われる事態をアプリケーションで回避する必要があれば、その方法としてデータベース ロックがあります。 これは呼び出されます*ペシミスティック同時実行制御*します。 たとえば、データベースから行を読む前に、読み取り専用か更新アクセスでロックを要求します。 更新アクセスで行をロックすると、他のユーザーはその行を読み取り専用または更新アクセスでロックできなくなります。変更中のデータのコピーが与えられるためです。 読み取り専用で行をロックすると、他のユーザーはその行を読み取り専用でロックできますが、更新アクセスではロックできません。

ロックの管理には、いくつかのデメリットがあります。 プログラムが複雑になります。 必要な重大なデータベース管理のリソースとアプリケーションのユーザーの数とパフォーマンスの問題が生じる増加 (つまり、適切にスケールされない)。 そのような理由から、一部のデータベース管理システムはペシミスティック同時実行制御に対応していません。 Entity Framework は、組み込みのサポートしていませんし、チュートリアルのこの方法は説明しませんが実装するためにします。

### <a name="optimistic-concurrency"></a>オプティミスティック同時実行制御

ペシミスティック同時実行制御の代わりに、*オプティミスティック同時実行制御*します。 オプティミスティック同時実行制御では、同時実行の競合の発生を許し、発生したら適切に対処します。 たとえば、John が実行されます、 *Department.aspx*ページで、数回のクリック、**編集**史学部、用のリンクし、削減、**予算**$ $1,000,000.00 から量125,000.00 します。 (John の競合する部門の管理し、自分の部門にお金を解放する必要がある)。

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

John が前に**Update**、Jane が、同じページの実行をクリックする、**編集**史学部、し、変更リンク、 **Start Date** 2011 年 1 月 10 日からフィールドを 1/1/1999 します。 (Jane の史学部の管理し、複数勤続年数を指定する必要がある)。

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John が**Update** Jane がクリックし、最初に、 **Update**します。 ジェーンのブラウザーの現在のリスト、**予算**金額が $1,000,000.00、としては、金額が $125,000.00 に John が変更されたため、正しくないです。

このシナリオで実行できる操作の一部を以下に示します。

- ユーザーが変更したプロパティを追跡記録し、それに該当する列だけをデータベースで更新できます。 例のシナリオでは、2 人のユーザーが異なるプロパティを更新したため、データは失われません。 次回の誰かが閲覧の履歴の部門、1/1/1999 が表示されますおよび 125,000.00 ドルです。 

    これは、Entity Framework の既定の動作であり、データが失われる可能性がある競合の数を大幅に短縮することができます。 ただし、この動作は、エンティティの同じプロパティに競合する変更が加えられた場合、データ損失を回避しません。 さらに、この動作は必ずしも可能であればエンティティ型をストアド プロシージャをマップするときに、エンティティへの変更は、データベースに加えられたときにすべてのエンティティのプロパティの更新されます。
- Jane の変更の John の変更を上書きすることができます。 Jane がクリックした後**Update**、**予算**金額が $1,000,000.00 に戻ります。 これは *Client Wins* (クライアント側に合わせる) シナリオまたは *Last in Wins* (最終書き込み者優先) シナリオと呼ばれています。 (クライアントの値よりも優先データ ストアの新機能です。)
- Jane の変更は、データベースに更新されないようにできます。 通常、エラー メッセージが表示は、データの現在の状態を表示させます、彼女できるようにする必要がある場合は、その変更を再入力できるようにと。 入力を保存し、それを再入力しなくても再適用する機会を提供してプロセスを自動化することがさらにします。 これは *Store Wins* (ストア側に合わせる) シナリオと呼ばれています。 (クライアントが送信した値よりデータストアの値が優先されます。)

### <a name="detecting-concurrency-conflicts"></a>同時実行競合の検出

Entity Framework では、処理することによって競合を解決できます`OptimisticConcurrencyException`Entity Framework がスローする例外。 このような例外がスローされるタイミングを認識する目的で、Entity Framework は競合を検出できなければなりません。 そのため、データベースとデータ モデルを適宜構成する必要があります。 競合検出を有効にするためのオプションには次のようなものがあります。

- データベースに行が変更されたときの判断に使用できるテーブルの列を含めます。 その列を含めるに Entity Framework を構成することができますし、 `Where` sql 句`Update`または`Delete`コマンド。

    目的は、`Timestamp`内の列、`OfficeAssignment`テーブル。

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    データ型、`Timestamp`列ともいいます`Timestamp`します。 ただし、列には、実際には日付または時刻の値にはが含まれていません。 代わりに、値は、行が更新されるたびにインクリメントが連続番号です。 `Update`または`Delete`コマンド、`Where`句に含まれる元`Timestamp`値。 別のユーザーの値によって更新される行が変更された場合`Timestamp`は元の値と異なるため、`Where`句に更新する行が返されません。 現在の行が更新されたしないことを Entity Framework が見つかると`Update`または`Delete`(つまり、影響を受けた行の数が 0 である場合) をコマンドを同時実行の競合として解釈します。
- Entity Framework 内のテーブルにすべての列の元の値を含めるを構成、`Where`の句`Update`と`Delete`コマンド。

    最初のオプションで、行が最初に読み取らため、行のすべてのものが変更された場合のように、`Where`句しない行を返すを更新する同時実行の競合として解釈し、Entity Framework。 このメソッドを使用して効率的に作業を`Timestamp`フィールドしますが、効率的なことができます。 多くの列を含むデータベース テーブルでは、する可能性が非常に大きな`Where`句、web アプリケーションで要求できます大量の状態を維持するとします。 大量の状態を保持することと、サーバー リソース (たとえば、セッション状態) が必要ですか、web ページ自体 (たとえば、ビュー状態) に含める必要があるためにアプリケーションのパフォーマンスが影響することができます。

このチュートリアルでは、エラー追跡プロパティがないエンティティのオプティミスティック同時実行競合の処理を追加します (、`Department`エンティティ) および追跡プロパティがエンティティの (、`OfficeAssignment`エンティティ)。

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>追跡プロパティを使用しないで、オプティミスティック同時実行の処理

オプティミスティック同時実行制御を実装するために、`Department`エンティティで、追跡がありません (`Timestamp`) プロパティは、次のタスクを完了します。

- 同時実行の追跡を有効にするデータ モデルを変更`Department`エンティティ。
- `SchoolRepository`クラスでの同時実行例外の処理、`SaveChanges`メソッド。
- *Departments.aspx*ページで、実行しようとした変更が成功したことを警告ユーザーにメッセージを表示して同時実行例外を処理します。 ユーザーは、現在の値を参照してくださいし、まだ必要な場合は、変更を再試行してください。

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>同時実行データ モデルでの追跡を有効にします。

Visual Studio では、このシリーズで前のチュートリアルで作業していた Contoso University web アプリケーションを開きます。

開いている*SchoolModel.edmx*、データ モデル デザイナーで右クリックし、`Name`プロパティ、`Department`エンティティをクリック**プロパティ**します。 **プロパティ**ウィンドウで、変更、`ConcurrencyMode`プロパティを`Fixed`します。

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

その他の非主キーのスカラー プロパティの同じ操作を行います (`Budget`、 `StartDate`、および`Administrator`)。(できないナビゲーション プロパティ。)生成するたびに、Entity Framework を指定します、`Update`または`Delete`を更新する SQL コマンド、 `Department` 、データベース内のエンティティ、これらの列 (元の値) で含める必要がある、`Where`句。 行が見つからない場合に場合、`Update`または`Delete`コマンドが実行される、Entity Framework では、オプティミスティック同時実行例外をスローします。

保存して、データ モデルを閉じます。

### <a name="handling-concurrency-exceptions-in-the-dal"></a>//DAL で同時実行例外の処理

開いている*SchoolRepository.cs*し、以下の追加`using`のステートメント、`System.Data`名前空間。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

次の新しい追加`SaveChanges`オプティミスティック同時実行例外を処理します。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

このメソッドが呼び出されたときに、同時実行エラーが発生した場合、メモリ内のエンティティのプロパティの値は、データベースの現在の値に置き換えられます。 Web ページが処理できるように、同時実行例外が再度スローされます。

`DeleteDepartment`と`UpdateDepartment`メソッド、既存の呼び出しを置き換える`context.SaveChanges()`への呼び出しで`SaveChanges()`新しいメソッドを呼び出すためにします。

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>プレゼンテーション層での同時実行例外の処理

開いている*Departments.aspx*を追加し、`OnDeleted="DepartmentsObjectDataSource_Deleted"`属性を`DepartmentsObjectDataSource`コントロール。 コントロールの開始タグは、次の例のようになります。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

`DepartmentsGridView`コントロールをすべての表の列の指定、`DataKeyNames`属性は、次の例に示すようにします。 メモ、これが作成されます非常に大きいビュー状態フィールド、理由の 1 つである追跡フィールドを使用する理由は一般に同時実行の競合を追跡することをお勧めします。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

開いている*Departments.aspx.cs*し、以下の追加`using`のステートメント、`System.Data`名前空間。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

データ ソース コントロールの呼び出しは次の新しいメソッドを追加`Updated`と`Deleted`同時実行例外を処理するためのイベント ハンドラー。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

このコードが例外の種類をチェックし、コードを動的に作成、同時実行例外の場合を`CustomValidator`順番にメッセージを表示するコントロールを`ValidationSummary`コントロール。

新しいメソッドを呼び出し、`Updated`先ほど追加したイベント ハンドラー。 さらに、作成、新しい`Deleted`イベント ハンドラーを同じメソッドを呼び出します (ただし、他の何も行いません)。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>部門のページでオプティミスティック同時実行のテスト

実行、 *Departments.aspx*ページ。

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

をクリックして**編集**行内の値を変更し、**予算**列。 (ために、のみ、このチュートリアル用に作成したレコードを編集することに注意してください。 既存の`School`データベース レコードには、無効なデータが含まれています。 経済性部門のレコードは、安全なみたりする場合。)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

新しいブラウザー ウィンドウを開き、ページをもう一度 (2 番目のブラウザー ウィンドウに、最初のブラウザー ウィンドウのアドレス ボックスから URL をコピーする) を実行します。

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

クリックして**編集**と同じで、以前に編集する行し、変更、**予算**を別の値。

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

2 番目のブラウザー ウィンドウでクリックして**Update**します。 **予算**量がこの新しい値に正常に変更されました。

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

最初のブラウザー ウィンドウでクリックして**Update**します。 更新プログラムは失敗します。 **予算**2 番目のブラウザー ウィンドウで設定した値を使用して、金額が表示され、エラー メッセージが表示されます。

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>追跡プロパティを使用してオプティミスティック同時実行の処理

追跡プロパティを持つエンティティのオプティミスティック同時実行制御を処理するには、次のタスクを行います。

- ストアド プロシージャを管理するデータ モデルに追加`OfficeAssignment`エンティティ。 (追跡のプロパティとストアド プロシージャは、一緒に使用する必要はありません。 がいるだけでグループ化されたここで図)。
- DAL との BLL に CRUD メソッドを追加`OfficeAssignment`DAL でオプティミスティック同時実行例外を処理するコードを含むエンティティ。
- オフィスの割り当ての web ページを作成します。
- 新しい web ページでは、オプティミスティック同時実行制御をテストします。

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>ストアド プロシージャ、データ モデルを OfficeAssignment を追加します。

開く、 *SchoolModel.edmx*モデル デザイナーでファイルをデザイン サーフェイスを右クリックし、をクリックして**データベースからモデルを更新**します。 **追加**のタブ、**データベース オブジェクトの選択** ダイアログ ボックスで、展開**ストアド プロシージャ**、3 つを選択します`OfficeAssignment`ストアド プロシージャ (を参照してください、。次のスクリーン ショット)、順にクリックします**完了**します。 (これらのストアド プロシージャが既にデータベースにダウンロードまたはスクリプトを使用して作成したときにします。)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

右クリックし、`OfficeAssignment`エンティティと選択**ストアド プロシージャ マッピング**します。

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

設定、**挿入**、**更新**、および**削除**ストアド プロシージャの対応を使用する関数。 `OrigTimestamp`のパラメーター、`Update`関数は、設定、**プロパティ**に`Timestamp`を選択し、**元の値を使用**オプション。

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Entity Framework を呼び出すと、`UpdateOfficeAssignment`ストアド プロシージャの元の値を渡すことは、`Timestamp`内の列、`OrigTimestamp`パラメーター。 ストアド プロシージャでは、このパラメーターを使用してその`Where`句。

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

ストアド プロシージャもの新しい値を選択、`Timestamp`列を更新後に、Entity Framework が保持できるように、`OfficeAssignment`対応するデータベースの行との同期がメモリ内のエンティティ。

(オフィスの割り当てを削除するためのストアド プロシージャはありませんが、`OrigTimestamp`パラメーター。 このため、Entity Framework を検証できませんエンティティが削除する前にも維持されます。)

保存して、データ モデルを閉じます。

### <a name="adding-officeassignment-methods-to-the-dal"></a>DAL に OfficeAssignment メソッドを追加

開いている*ISchoolRepository.cs*の次の CRUD メソッドを追加し、`OfficeAssignment`エンティティ セット。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

次の新しいメソッドを追加*SchoolRepository.cs*します。 `UpdateOfficeAssignment`メソッド、ローカルを呼び出す`SaveChanges`メソッドの代わりに`context.SaveChanges`します。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

テスト プロジェクトで開きます*MockSchoolRepository.cs*し、以下の追加`OfficeAssignment`コレクションと CRUD メソッド。 (モック リポジトリは、リポジトリ インターフェイスを実装する必要があります。 またはソリューションがコンパイルされません)。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>BLL に OfficeAssignment メソッドを追加

メインのプロジェクトで開きます*SchoolBL.cs*の次の CRUD メソッドを追加し、`OfficeAssignment`エンティティを設定するには。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>OfficeAssignments Web ページの作成

使用する新しい web ページを作成、 *Site.Master*マスター ページと名前を付けます*OfficeAssignments.aspx*します。 次のマークアップを追加、`Content`という名前のコントロール`Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

インシデントを`DataKeyNames`属性は、マークアップを指定します、`Timestamp`プロパティだけでなく、レコードのキー (`InstructorID`)。 プロパティを指定する、`DataKeyNames`属性によって、コントロールにコントロールの状態 (これは状態を表示するような) に保存するポストバック処理中に元の値が使用できるようにします。

保存しなかった場合、`Timestamp`値、Entity Framework ができないため、 `Where` sql 句`Update`コマンド。 その結果を更新する何も見つかりませんなります。 Entity Framework は毎回オプティミスティック同時実行例外をスローする結果として、`OfficeAssignment`エンティティを更新します。

開いている*OfficeAssignments.aspx.cs*し、次を追加`using`データ アクセス層のステートメント。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

次の追加`Page_Init`メソッドで、Dynamic Data 機能を有効にします。 次のハンドラーを追加しても、`ObjectDataSource`コントロールの`Updated`イベントの同時実行エラーを確認するには。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>OfficeAssignments ページでオプティミスティック同時実行のテスト

実行、 *OfficeAssignments.aspx*ページ。

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

をクリックして**編集**行内の値を変更し、**場所**列。

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

新しいブラウザー ウィンドウを開き、ページをもう一度 (2 番目のブラウザー ウィンドウに、最初のブラウザー ウィンドウから URL をコピーする) を実行します。

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

クリックして**編集**と同じで、以前に編集する行し、変更、**場所**を別の値。

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

2 番目のブラウザー ウィンドウでクリックして**Update**します。

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

最初のブラウザー ウィンドウに切り替え、をクリックして**Update**します。

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

エラー メッセージが表示、**場所**に 2 番目のブラウザー ウィンドウで変更する値を表示する値が更新されました。

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>EntityDataSource コントロールでの同時実行の処理

`EntityDataSource`コントロールには、データ モデルでの同時実行設定を認識する組み込みのロジックが含まれていて、ハンドルを更新および削除操作はそれに応じて。 ただし、すべての例外と行う必要があります`OptimisticConcurrencyException`例外わかりやすいエラー メッセージを提供するために自分でします。

次に、構成は、 *Courses.aspx*ページ (使用する、`EntityDataSource`コントロール) を更新できるようにして、削除操作と同時実行の競合が発生した場合、エラー メッセージを表示します。 `Course`エンティティで行った方法と同じものを使用するために、列を追跡する並列処理はありません、`Department`エンティティ: すべてのキー以外のプロパティの値を追跡します。

開く、 *SchoolModel.edmx*ファイル。 キー以外のプロパティの`Course`エンティティ (`Title`、 `Credits`、および`DepartmentID`)、設定、**同時実行モード**プロパティを`Fixed`します。 保存して、データ モデルを終了します。

開く、 *Courses.aspx*ページし、次の変更を加えます。

- `CoursesEntityDataSource`コントロールを追加`EnableUpdate="true"`と`EnableDelete="true"`属性。 そのコントロールの開始タグには、次の例では、今すぐようになります。

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- `CoursesGridView`制御、変更、`DataKeyNames`属性に値`"CourseID,Title,Credits,DepartmentID"`します。 追加し、`CommandField`要素を`Columns`要素を示す**編集**と**削除**ボタン (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`)。 `GridView`コントロール次の例のようになります。

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

ページを実行し、前に、[部門] ページで行ったように、競合状況を作成します。 2 つのブラウザー ウィンドウで、ページの実行をクリックします**編集**と同じで、各ウィンドウの行し、それぞれに別の変更を加えます。 をクリックして**更新**クリックして 1 つのウィンドウで**更新**他のウィンドウで。 クリックすると**Update** 2 回目、同時実行のハンドルされない例外に起因するエラー ページが表示されます。

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

このエラーの処理方法に類似した方法で処理する、`ObjectDataSource`コントロール。 開く、 *Courses.aspx*  ページで、および、`CoursesEntityDataSource`コントロール、ハンドラーを指定、`Deleted`と`Updated`イベント。 コントロールの開始タグには、次の例では、今すぐようになります。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

前に、`CoursesGridView`コントロールを次の追加`ValidationSummary`コントロール。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

*Courses.aspx.cs*、追加、`using`のステートメント、`System.Data`名前空間は、同時実行例外をチェックするメソッドを追加し、ハンドラーを追加、`EntityDataSource`コントロールの`Updated`と`Deleted`ハンドラー。 コードは、次のようになります。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

このコード実行したとの唯一の違い、`ObjectDataSource`コントロールがここでは、同時実行例外があるは`Exception`その例外のではなく、イベント引数オブジェクトのプロパティ`InnerException`プロパティ。

ページを実行し、同時実行の競合をもう一度作成します。 この時間は、エラー メッセージが表示します。

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

同時実行の競合処理の入門編はこれで終わりです。 次のチュートリアルでは、Entity Framework を使用する web アプリケーションのパフォーマンスを向上する方法のガイダンスが提供されます。

> [!div class="step-by-step"]
> [前へ](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [次へ](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
