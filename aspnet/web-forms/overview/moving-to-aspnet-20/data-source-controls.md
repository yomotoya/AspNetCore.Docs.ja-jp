---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: データ ソース コントロール |Microsoft Docs
author: microsoft
description: DataGrid コントロールで ASP.NET 1.x が Web アプリケーションでのデータ アクセスに大きな強化をマークします。 ただし、した可能性がありますと、わかりやすいでした.
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: 15a09e8ac7da6d23216a92863ae7ce6db7ecd57a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809355"
---
<a name="data-source-controls"></a>データ ソース コントロール
====================
によって[Microsoft](https://github.com/microsoft)

> DataGrid コントロールで ASP.NET 1.x が Web アプリケーションでのデータ アクセスに大きな強化をマークします。 ただし、した可能性がありますと、わかりやすいありませんでした。 そこから多くの便利な機能を取得するコードの非常に長い時間が必要です。 1.x ですべてのデータ アクセスの試みで、このモデルです。


DataGrid コントロールで ASP.NET 1.x が Web アプリケーションでのデータ アクセスに大きな強化をマークします。 ただし、した可能性がありますと、わかりやすいありませんでした。 そこから多くの便利な機能を取得するコードの非常に長い時間が必要です。 1.x ですべてのデータ アクセスの試みで、このモデルです。

ASP.NET 2.0 では、これをデータ ソース コントロールでの一部です。 ASP.NET 2.0 のデータ ソース コントロールでは、開発者向けのデータの取得、データの表示、およびデータを編集するため、宣言型モデルを提供します。 データ ソース コントロールの目的は、これらのデータのソースに関係なくデータ バインドされたコントロールへのデータの一貫性のある表現を提供することです。 ASP.NET 2.0 のデータ ソース コントロールの中核はデータ ソース コントロールの抽象クラスです。 データ ソース コントロール クラスは、IDataSource インターフェイスと IListSource インターフェイスを (新しい DataSourceId プロパティを使用してデータ バインド コントロールのデータ ソースとデータ ソース コントロールを割り当てることにより、後者の基本実装を提供します。後で説明) をリストとしてそのデータを公開します。 データ ソース コントロールからのデータの各リストは、DataSourceView オブジェクトとして公開されます。 DataSourceView のインスタンスへのアクセスは、IDataSource インターフェイスによって提供されます。 たとえば、ICollection を DataSourceViews を列挙することができますが、特定のデータ ソース コントロールに関連付けられているし、GetView メソッドでは、名前で特定の DataSourceView インスタンスにアクセスできます。 GetViewNames メソッドを返します。

データ ソース コントロールには、ユーザー インターフェイスが用意されていません。 必要な場合はページの状態にアクセス権があるように、サーバー コントロール宣言構文をサポートできるようにとして実装されます。 データ ソース コントロールでは、すべての HTML マークアップはクライアントに表示されません。

> [!NOTE]
> 後でわかりますがありますもキャッシュ データ ソース コントロールを使用して取得した利点。


## <a name="storing-connection-strings"></a>接続文字列を格納します。

をデータ ソース コントロールを構成する方法を見ていく前に、接続文字列に関する ASP.NET 2.0 の新機能について説明します必要があります。 ASP.NET 2.0 では、実行時に動的に読み込むことができる接続文字列を簡単に格納できるように構成ファイル内の新しいセクションについて説明します。 &lt;ConnectionStrings&gt;セクションでは、簡単に接続文字列を格納します。

次のスニペットでは、新しい接続文字列を追加します。

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> 同様、 &lt;appSettings&gt;  セクションで、 &lt;connectionStrings&gt;外のセクションが表示されます、 &lt;system.web&gt;構成ファイルのセクション。


この接続文字列を使用するには、サーバー コントロールの ConnectionString 属性を設定するときに、次の構文を使用することができます。

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt;セクションは、機密情報が公開されないようにも暗号化できます。 その機能については、後のモジュールで説明します。

## <a name="caching-data-sources"></a>キャッシュのデータ ソース

各データ ソース コントロールがキャッシュを構成する 4 つのプロパティを提供しますEnableCaching、CacheDuration、CacheExpirationPolicy、および CacheKeyDependency します。

## <a name="enablecaching"></a>EnableCaching

EnableCaching は、データ ソース コントロールのキャッシュに動作しているかどうかを決定するブール型プロパティが有効になっています。

## <a name="cacheduration-property"></a>CacheDuration プロパティ

CacheDuration プロパティは、キャッシュの有効期間を秒数を設定します。 このプロパティを設定**0**により、キャッシュを明示的に無効にするまで有効なままにします。

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy プロパティ

CacheExpirationPolicy プロパティの設定はいずれかに**絶対**または**スライド**します。 データがキャッシュされる時間の最大量が CacheDuration プロパティで指定された秒数である絶対手段に設定します。 スライド式に設定すると、各操作の実行時に、有効期限がリセットされます。

## <a name="cachekeydependency-property"></a>CacheKeyDependency プロパティ

CacheKeyDependency プロパティの文字列値を指定すると、その文字列に基づいて、新しいキャッシュ依存関係を ASP.NET が設定されます。 これによりを明示的に単に変更または、CacheKeyDependency を削除してキャッシュを無効にすることができます。

**重要な**: 権限借用を有効にしへのアクセス、データ ソースやデータの内容がクライアント id に基づく、EnableCaching を False に設定してキャッシュを無効にすることをお勧めします。 このシナリオでキャッシュが有効になっている最初のデータを要求したユーザーは別のユーザー要求を発行する場合は、承認、データ ソースには適用されません。 データは、単純にキャッシュから提供されます。

## <a name="the-sqldatasource-control"></a>SqlDataSource コントロール

詳細を表示する、従業員の編集、従業員を追加する、従業員を削除して、インターフェイスと対話します。 SQL Server データベース、System.Data.OleDb プロバイダー、System.Data.Odbc プロバイダー、または Oracle へのアクセスに System.Data.OracleClient プロバイダーへのアクセスに System.Data.SqlClient プロバイダーを使用できます。 そのため、SqlDataSource は確かにのみ使用されません、SQL Server データベース内のデータにアクセスするため。

SqlDataSource を使用するには単に ConnectionString プロパティの値を指定および指定する SQL コマンドかストアド プロシージャ。 SqlDataSource コントロールは、基になる ADO.NET のアーキテクチャの操作の処理されます。 接続を開き、データ ソースをクエリまたはストアド プロシージャを実行、データ、の接続を閉じます。

> [!NOTE]
> データ ソース コントロール クラスには、接続は自動的に閉じる、ために、データベース接続のリークによって生成された顧客の通話の数を減らすにする必要があります。


次のコード スニペットは、上記のように構成ファイルに格納されている接続文字列を使用して、SqlDataSource コントロールに、DropDownList コントロールをバインドします。

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

上記のよう、SqlDataSource の DataSourceMode プロパティには、データ ソースのモードを指定します。 上記の例では、DataSourceMode は DataReader に設定されます。 その場合は、SqlDataSource は IDataReader を順方向専用、読み取り専用カーソルを使用してオブジェクトを返します。 返されるオブジェクトの指定した型は、使用されているプロバイダーによって制御されます。 この場合、使用している System.Data.SqlClient プロバイダーで指定されている、 &lt;connectionStrings&gt; web.config ファイルのセクション。 そのため、返されるオブジェクトは、SqlDataReader の種類のなります。 データセットの DataSourceMode 値を指定すると、サーバー上のデータセットにデータを格納できます。 このモードでは、並べ替え、ページングなどなどの機能を追加できます。データ バインディングいた場合は、GridView コントロールに SqlDataSource、私は選択したデータセット モード。 ただし、DropDownList の場合、DataReader のモードでは、適切な選択には。

> [!NOTE]
> SqlDataSource、または、AccessDataSource をキャッシュする場合は、データセットに DataSourceMode プロパティを設定する必要があります。 DataReader の DataSourceMode でキャッシュを有効にした場合、例外が発生します。


## <a name="sqldatasource-properties"></a>SqlDataSource プロパティ

SqlDataSource コントロールのプロパティの一部を次に示します。

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

パラメーターのいずれかが null の場合、select コマンドが取り消されたかどうかを指定するブール値。 既定では true です。

### <a name="conflictdetection"></a>ConflictDetection

複数のユーザー更新することがデータ ソースと同時の状況では、ConflictDetection プロパティは、SqlDataSource コントロールの動作を決定します。 このプロパティは、ConflictOptions 列挙体の値のいずれかに評価されます。 これらの値が**CompareAllValues**と**OverwriteChanges**します。 場合 OverwriteChanges、最後に、データ ソースにデータを書き込む人物に設定するには、前の変更が上書きされます。 ただし、ConflictDetection プロパティが CompareAllValues に設定されている場合、SelectCommand によって返される列の作成パラメーターと、パラメーターは、各に SqlDataSource を許可するこれらの列の元の値を保持するためにも作成されます。SelectCommand が実行されるため、値が変更されたかどうかを決定します。

### <a name="deletecommand"></a>DeleteCommand

設定またはデータベースから行を削除するときに使用する SQL 文字列を取得します。 SQL クエリまたはストアド プロシージャ名を指定できます。

### <a name="deletecommandtype"></a>DeleteCommandType

設定または SQL クエリ (テキスト) またはストアド プロシージャ (StoredProcedure) か、削除のコマンドの種類を取得します。

### <a name="deleteparameters"></a>によって

SqlDataSource コントロールに関連付けられた SqlDataSourceView オブジェクトの DeleteCommand によって使用されるパラメーターを返します。

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

このプロパティは、CompareAllValues に ConflictDetection プロパティが設定されている場合に元の値パラメーターの形式を指定に使用されます。 既定値は{0}元のパラメーターの値が元のパラメーターと同じ名前になることを意味します。 つまり、フィールド名が EmployeeID である場合は、元の値のパラメーターは@EmployeeIDします。

### <a name="selectcommand"></a>SelectCommand

設定またはデータベースからデータを取得するために使用する SQL 文字列を取得します。 SQL クエリまたはストアド プロシージャ名を指定できます。

### <a name="selectcommandtype"></a>SelectCommandType

設定または SQL クエリ (テキスト) またはストアド プロシージャ (StoredProcedure) か、select コマンドは、の型を取得します。

### <a name="selectparameters"></a>SelectParameters

SqlDataSource コントロールに関連付けられた SqlDataSourceView オブジェクトの SelectCommand によって使用されるパラメーターを返します。

### <a name="sortparametername"></a>SortParameterName

取得またはデータ ソース コントロールでデータの並べ替えを取得するときに使用されるストアド プロシージャのパラメーターの名前を設定します。 ストアド プロシージャに SelectCommandType が設定されている場合にのみ有効です。

### <a name="sqlcachedependency"></a>SqlCacheDependency

セミコロン区切りのデータベースと SQL Server キャッシュの依存関係で使用されるテーブルを指定する文字列。 (SQL キャッシュ依存関係は後のモジュールで説明されます)。

### <a name="updatecommand"></a>UpdateCommand

設定またはデータベース内のデータを更新するときに使用される SQL 文字列を取得します。 SQL クエリまたはストアド プロシージャ名を指定できます。

### <a name="updatecommandtype"></a>UpdateCommandType

設定または SQL クエリ (テキスト) またはストアド プロシージャ (StoredProcedure) か、更新プログラムのコマンドの種類を取得します。

### <a name="updateparameters"></a>UpdateParameters

SqlDataSource コントロールに関連付けられた SqlDataSourceView オブジェクトの UpdateCommand によって使用されるパラメーターを返します。

## <a name="the-accessdatasource-control"></a>AccessDataSource コントロール

AccessDataSource コントロールでは、SqlDataSource クラスから派生し、Microsoft Access データベースにデータをバインドするために使用します。 AccessDataSource コントロール用の ConnectionString プロパティは、読み取り専用プロパティです。 ConnectionString プロパティを使用するのではなく、データ ファイルのプロパティは、次に示すように、Access データベースをポイントに使用されます。

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource System.Data.OleDb を基本 SqlDataSource の ProviderName は常に設定し、Microsoft.Jet.OLEDB.4.0 OLE DB プロバイダーを使用してデータベースに接続します。 AccessDataSource コントロールを使用して、パスワードで保護された Access データベースに接続することはできません。 パスワードで保護されたデータベースに接続した場合、SqlDataSource コントロールを使用する必要があります。

> [!NOTE]
> アプリで、Web サイト内に格納されている access データベースを配置する必要があります\_データ ディレクトリ。 ASP.NET では、参照するには、このディレクトリ内のファイルは許可されません。 プロセス アカウントをアプリに読み取りと書き込みのアクセス許可を付与する必要があります\_Access データベースを使用する場合、データ ディレクトリ。


## <a name="the-xmldatasource-control"></a>XmlDataSource コントロール

XmlDataSource は、データ バインド コントロールに XML データをデータ バインドに使用されます。 データ ファイルのプロパティを使用して XML ファイルにバインドすることができますか、データ プロパティを使用して XML 文字列にバインドすることができます。 XmlDataSource では、バインド可能なフィールドとしての XML 属性を公開します。 属性としては表されない値をバインドする必要がある場合は、XSL 変換を使用する必要があります。 XML データのフィルター処理する XPath 式を使用することもできます。

次の XML ファイルを考慮してください。

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

XmlDataSource がの XPath プロパティを使用することに注意してください。*人付き合い/* でフィルター処理するためにだけ、 &lt;Person&gt;ノード。 DropDownList、DataTextField プロパティを使用して LastName 属性にデータをバインドします。

XmlDataSource コントロールは主に読み取り専用の XML データへのデータをバインドするために使用、XML データ ファイルを編集すること勧めします。 このような場合、自動挿入、更新、および XML ファイル内の情報の削除が動作しない自動的に他のデータ ソース コントロールと同様に注意してください。 代わりに、XmlDataSource コントロールの次のメソッドを使用してデータを手動で編集するコードを記述する必要があります。

### <a name="getxmldocument"></a>GetXmlDocument

XmlDataSource によって取得された XML コードを含む XmlDocument オブジェクトを取得します。

### <a name="save"></a>保存

メモリ内の XmlDocument をデータ ソースに保存します。

Save メソッドは、次の 2 つの条件が満たされたときに機能のみを認識する必要があります。

1. XmlDataSource がメモリ内の XML データにバインドするデータ プロパティの代わりに XML ファイルにバインドするデータ ファイルのプロパティを使用してください。
2. 変換または TransformFile プロパティを使用して変換が指定されていません。

Save メソッドが同時に複数のユーザーによって呼び出されたときに予期しない結果を生成できることにも注意してください。

## <a name="the-objectdatasource-control"></a>ObjectDataSource コントロール

ここまで説明するデータ ソース コントロールとは、データ ソース コントロールがデータ ストアに直接通信する 2 層アプリケーションの優れた選択肢です。 ただし、多くの実際のアプリケーションは多層アプリケーション データ ソース コントロールがさらに、データ層と通信するビジネス オブジェクトと通信する必要があります。 このような場合は、ObjectDataSource では請求書が適切に設定します。 ObjectDataSource は、ソース オブジェクトと組み合わせて動作します。 ObjectDataSource コントロールは、ソース オブジェクト、指定されたメソッドを呼び出して 1 つの要求のスコープ内ですべてのオブジェクト インスタンスの dispose のインスタンスを作成する場合は、オブジェクト (Visual Basic では Shared) の静的メソッドではなくインスタンス メソッドがあります。 そのため、オブジェクトは、ステートレスである必要があります。 オブジェクトでは、取得および 1 つの要求の範囲内のすべての必要なリソースを解放する必要があります。 ObjectDataSource コントロールの ObjectCreating イベントを処理することによって、ソース オブジェクトを作成する方法を制御できます。 ソース オブジェクトのインスタンスを作成し、そのインスタンスに ObjectDataSourceEventArgs クラスの ObjectInstance プロパティを設定できます。 ObjectDataSource コントロール、独自のインスタンスを作成する代わりに ObjectCreating イベントで作成されたインスタンスが使用されます。

ObjectDataSource コントロールのソース オブジェクトは、データを取得および変更を呼び出せるパブリック静的メソッド (Visual Basic では Shared) を公開する場合、ObjectDataSource コントロールはこれらのメソッドを直接呼び出します。 ObjectDataSource コントロールは、メソッドの呼び出しを行うために、ソース オブジェクトのインスタンスを作成する必要があります、オブジェクトは、パブリック パラメーターを受け取らないコンス トラクターを含める必要があります。 ObjectDataSource コントロールは、ソース オブジェクトの新しいインスタンスを作成するときにこのコンス トラクターを呼び出します。

ソース オブジェクトに、パラメーターなしのパブリック コンス トラクターが含まれていない場合は、ObjectCreating イベントで ObjectDataSource コントロールによって使用されるソース オブジェクトのインスタンスを作成することができます。

## <a name="specifying-object-methods"></a>オブジェクトのメソッドを指定します。

ObjectDataSource コントロールのソース オブジェクトでは、任意の数の選択、挿入、更新に使用されるメソッドを含むしたり、データを削除することができます。 これらのメソッドは、ObjectDataSource コントロールのいずれか、SelectMethod、InsertMethod、UpdateMethod、または DeleteMethod プロパティを使用して識別される、メソッドの名前に基づいて、ObjectDataSource コントロールによって呼び出されます。 ソース オブジェクトは、データ ソース オブジェクトの合計数のカウントを返す SelectCountMethod のプロパティを使用して、ObjectDataSource コントロールで識別されるの省略可能な SelectCount メソッドも指定できます。 ObjectDataSource コントロールはページングするときに使用するため、データ ソースのレコードの合計数を取得する Select メソッドが呼び出された後、SelectCount メソッドを呼び出します。

## <a name="lab-using-data-source-controls"></a>データ ソース コントロールを使用してラボ

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>手順 1 - SqlDataSource コントロールでのデータの表示

次の手順では、Northwind データベースに接続する、SqlDataSource コントロールを使用します。 これにより、SQL Server 2000 インスタンスに、Northwind データベースへのアクセスがあることを前提としています。

1. 新しい ASP.NET Web サイトを作成します。
2. 新しい web.config ファイルを追加します。

    1. ソリューション エクスプ ローラーでプロジェクトを右クリックし、[新しい項目の追加] をクリックします。
    2. テンプレートの一覧から Web 構成ファイルを選択し、[追加] をクリックします。
3. 編集、 &lt;connectionStrings&gt;セクションの次のようにします。 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. コード ビューに切り替え、ConnectionString 属性と SelectCommand 属性を追加、 &lt;asp: SqlDataSource&gt;次のように制御します。 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. [デザイン] ビューから新しい GridView コントロールを追加します。
6. GridView タスク メニューの データ ソースの選択ドロップダウン リストから SqlDataSource1 を選択します。
7. Default.aspx を右クリックし、メニューから、ブラウザーでビューを選択します。 保存を求められたら、[はい] をクリックします。
8. GridView では、Products テーブルからデータを表示します。

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>手順 2 - SqlDataSource コントロールにデータを編集

次の手順および方法を示しますデータをバインドする DropDownList コントロール宣言構文を使用して、DropDownList コントロールに表示されるデータを編集することができます。

1. デザイン ビューでは、Default.aspx から GridView コントロールを削除します。 

    **重要な**: ページで、SqlDataSource コントロールのままにします。
2. DropDownList コントロールを Default.aspx に追加します。
3. ソース ビューに切り替えます。
4. DataSourceId、DataTextField、および DataValueField 属性を追加、 &lt;asp: DropDownList&gt;次のように制御します。 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Default.aspx を保存し、ブラウザーで表示します。 DropDownList に Northwind データベースから製品が含まれているに注意してください。
6. ブラウザーを閉じます。
7. Default.aspx のソース ビューでは、DropDownList コントロールの下に新しい TextBox コントロールを追加します。 TxtProductName にテキスト ボックスの ID プロパティを変更します。
8. テキスト ボックス コントロールの下には、新しいボタン コントロールを追加します。 BtnUpdate を Text プロパティをボタンの ID プロパティを変更する**製品の更新名**します。
9. Default.aspx のソース ビューで、UpdateCommand プロパティと 2 つの新しい UpdateParameters SqlDataSource タグによう追加します。 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > このコードで追加したパラメーター (ProductName および ProductID) 2 つの更新があることに注意してください。 これらのパラメーターは、txtProductName TextBox の Text プロパティと ddlProducts DropDownList の SelectedValue プロパティにマップされます。
10. デザイン ビューに切り替えるし、イベント ハンドラーを追加するボタン コントロールをダブルクリックします。
11. 次のコードを追加、btnUpdate に\_コードをクリックします。 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Default.aspx を右クリックし、ブラウザーで表示を選択します。 すべての変更を保存するように求められたら、[はい] をクリックします。
13. ASP.NET 2.0 の実行時にコンパイル部分クラスを使用します。 コード変更の反映を確認するためにアプリケーションを構築する必要はありません。
14. 次のドロップダウン リストから製品を選択します。
15. ボックスに、選択した製品の新しい名前を入力および更新 ボタンをクリックします。
16. 製品名は、データベースで更新されます。

## <a name="exercise-3-using-the-objectdatasource-control"></a>演習 3 ObjectDataSource コントロールを使用します。

この演習では、ObjectDataSource コントロールとソース オブジェクトを使用して Northwind データベースと対話する方法について説明します。

1. ソリューション エクスプ ローラーでプロジェクトを右クリックし、[新しい項目の追加] をクリックします。
2. テンプレートの一覧で Web フォームを選択します。 Object.aspx に名前を変更し、[追加] をクリックします。
3. ソリューション エクスプ ローラーでプロジェクトを右クリックし、[新しい項目の追加] をクリックします。
4. テンプレートの一覧で、クラスを選択します。 NorthwindData.cs にクラスの名前を変更し、[追加] をクリックします。
5. クラスをアプリに追加するように求められたら [はい] をクリックします。\_コード フォルダー。
6. NorthwindData.cs ファイルには、次のコードを追加します。 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Object.aspx のソース ビューには、次のコードを追加します。 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. すべてのファイルを保存し、object.aspx を参照します。
9. 詳細を表示する、従業員の編集、従業員を追加する、従業員を削除して、インターフェイスと対話します。
