---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: "データ ソース コントロール |Microsoft ドキュメント"
author: microsoft
description: "DataGrid コントロール ASP.NET 1.x としてマークされている Web アプリケーションでのデータ アクセスでは、優れた向上します。 ただし、した可能性がありますと、わかりやすいでした."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: f40189796d3e25e9c337768cf04fdbfa293cdc2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="data-source-controls"></a>データ ソース コントロール
====================
によって[Microsoft](https://github.com/microsoft)

> DataGrid コントロール ASP.NET 1.x としてマークされている Web アプリケーションでのデータ アクセスでは、優れた向上します。 ただし、した可能性がありますと、わかりやすいされませんでした。 そこから多くの便利な機能を取得するコードの非常に長い時間が必要です。 1.x ですべてのデータ アクセスの試みで、このモデルです。


DataGrid コントロール ASP.NET 1.x としてマークされている Web アプリケーションでのデータ アクセスでは、優れた向上します。 ただし、した可能性がありますと、わかりやすいされませんでした。 そこから多くの便利な機能を取得するコードの非常に長い時間が必要です。 1.x ですべてのデータ アクセスの試みで、このモデルです。

ASP.NET 2.0 でこの一部に対処データ ソース コントロールします。 ASP.NET 2.0 のデータ ソース コントロールは、データを取得して、データを表示するデータの編集の宣言型モデルを開発者に提供します。 データ ソース コントロールの目的は、それらのデータのソースに関係なくデータ バインドされたコントロールへのデータの一貫性のある表現を指定します。 ASP.NET 2.0 のデータ ソース コントロールの中心に、データ ソース コントロールの抽象クラスです。 データ ソース コントロールのクラスは IDataSource インターフェイスおよび後者のすることができます (新しい DataSourceId プロパティを使用してデータ バインド コントロールのデータ ソースとして、データ ソース コントロールを割り当てる、IListSource インターフェイスの基本実装を提供します。後で説明しています) し、一覧とその中のデータを公開します。 データ ソース コントロールからのデータの各リストは、DataSourceView オブジェクトとして公開されます。 DataSourceView インスタンスへのアクセスは IDataSource インターフェイスによって提供されます。 たとえば、GetViewNames メソッドは、特定のデータ ソース コントロールに関連付けられている、DataSourceViews を列挙することができますを ICollection と GetView メソッドでは、名前で特定の DataSourceView インスタンスにアクセスすることができますを返します。

データ ソース コントロールでは、ユーザー インターフェイスを持っているありません。 これらは、宣言構文をサポートできるようにサーバーが制御し、アクセスできるように、ページの状態に必要な場合に実装されます。 データ ソース コントロールでは、クライアントにすべての HTML マークアップが表示されません。

> [!NOTE]
> わかる後がありますはキャッシュも利点がデータ ソース コントロールを使用して取得します。


## <a name="storing-connection-strings"></a>接続文字列を格納します。

前にデータ ソース コントロールを構成する方法を見て、接続文字列をに関する ASP.NET 2.0 の新機能について説明する必要があります。 ASP.NET 2.0 には、実行時に動的に読み込むことができる接続文字列を簡単に格納できるように構成ファイル内の新しいセクションが導入されています。 &lt;ConnectionStrings&gt;セクションで簡単に接続文字列を格納します。

次のスニペットは、新しい接続文字列を追加します。

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> 同様、 &lt;appSettings&gt;  セクションで、 &lt;connectionStrings&gt;以外のセクションが表示されます、 &lt;system.web&gt;構成ファイル内のセクションです。


この接続文字列を使用するのには、サーバー コントロールの ConnectionString 属性を設定するときに、次の構文を使用できます。

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt;セクションでは、機密情報が公開されていないようにも暗号化できます。 その機能は、後のモジュールで取り上げます。

## <a name="caching-data-sources"></a>データ ソースのキャッシュ

各データ ソース コントロールは、4 つのプロパティです。 キャッシュを構成します。EnableCaching、CacheDuration、CacheExpirationPolicy、および CacheKeyDependency です。

## <a name="enablecaching"></a>EnableCaching

EnableCaching は、データ ソース コントロールが有効になってキャッシュするかどうかどうかを指定するブール型プロパティです。

## <a name="cacheduration-property"></a>CacheDuration プロパティ

CacheDuration プロパティは、キャッシュの有効期間を秒数を設定します。 このプロパティを設定**0**により、キャッシュは、明示的に無効にするまで有効なままです。

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy プロパティ

CacheExpirationPolicy プロパティの設定はどちらかに**絶対**または**スライディング**です。 絶対あることを意味、データがキャッシュされる最長時間 CacheDuration プロパティによって指定された秒数を設定します。 スライド式に設定して、各操作が実行されるときに、有効期限がリセットされます。

## <a name="cachekeydependency-property"></a>CacheKeyDependency プロパティ

CacheKeyDependency プロパティの文字列値を指定すると、その文字列に基づいて、新しいキャッシュ依存関係を ASP.NET が設定されます。 これにより、変更、または、CacheKeyDependency を削除するだけで明示的にキャッシュを無効にすることができます。

**重要な**: 権限借用を有効にしへのアクセス、データ ソースやデータのコンテンツがクライアント id に基づく、EnableCaching を False に設定してキャッシュを無効にすることをお勧めします。 このシナリオでキャッシュが有効になっている最初のデータを要求したユーザーは別のユーザー要求を発行する場合は、データ ソースへの承認は適用されません。 データは、キャッシュから提供するだけです。

## <a name="the-sqldatasource-control"></a>SqlDataSource コントロール

SqlDataSource コントロールには、ADO.NET をサポートする任意のリレーショナル データベースに格納されているデータにアクセスする開発者ができるようにします。 SQL Server データベース、System.Data.OleDb プロバイダー、System.Data.Odbc プロバイダーまたは Oracle にアクセスするのに System.Data.OracleClient プロバイダーにアクセスするのに、System.Data.SqlClient プロバイダーを使用できます。 したがって、SqlDataSource が確実にのみ使用されません、SQL Server データベース内のデータにアクセスするため。

SqlDataSource を使用するためにするだけで ConnectionString プロパティの値を指定し、SQL コマンドを指定またはストアド プロシージャ。 ADO.NET のアーキテクチャを基になる操作の SqlDataSource コントロールによって行われます。 接続を開き、データ ソースをクエリまたはストアド プロシージャを実行、データを返し、するための接続を閉じます。

> [!NOTE]
> データ ソース コントロールのクラスは、自動的に接続を自動的に閉じます、ので、データベース接続のリークによって生成された顧客の呼び出しの数を減らすにする必要があります。


次のコード スニペットは、上記のように構成ファイルに格納されている接続文字列を使用して、SqlDataSource コントロールに DropDownList コントロールをバインドします。

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

上記のよう、SqlDataSource の DataSourceMode プロパティは、データ ソースのモードを指定します。 上記の例では、DataSourceMode が DataReader に設定されます。 その場合は、SqlDataSource は IDataReader を順方向専用、および読み取り専用カーソルを使用してオブジェクトを返します。 返されるオブジェクトの指定した型は、使用されているプロバイダーによって制御されます。 この場合、使用している System.Data.SqlClient プロバイダーで指定されている、 &lt;connectionStrings&gt; web.config ファイルのセクションです。 このため、返されるオブジェクトは、SqlDataReader 型のなります。 データセットの DataSourceMode 値を指定すると、サーバー上のデータセットにデータを格納できます。 このモードでは、並べ替え、ページングなどなどの機能を追加することができます。データ バインディングはされていた場合を GridView コントロール SqlDataSource、I は選択したデータセット モード。 、、の DropDownList の場合、DataReader モードが適切な選択です。

> [!NOTE]
> SqlDataSource、または、AccessDataSource をキャッシュする場合は、データセットに DataSourceMode プロパティを設定する必要があります。 DataReader の DataSourceMode でキャッシュを有効にした場合、例外が発生します。


## <a name="sqldatasource-properties"></a>SqlDataSource プロパティ

SqlDataSource コントロールのプロパティの一部を次に示します。

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

パラメーターのいずれかが null の場合、select コマンドが取り消されたかどうかを指定するブール値。 既定では true です。

### <a name="conflictdetection"></a>ConflictDetection

ここで複数のユーザー更新することがデータ ソース、同時の状況では、ConflictDetection プロパティは、SqlDataSource コントロールの動作を決定します。 このプロパティは、ConflictOptions 列挙の値のいずれかに評価されます。 それらの値が**CompareAllValues**と**OverwriteChanges**です。 場合は OverwriteChanges、最終更新者がデータ ソースへのデータの書き込みに設定するには、前の変更が上書きされます。 ただし、ConflictDetection プロパティが CompareAllValues に設定されている場合、SelectCommand によって返される列に対するパラメーターを作成し、パラメーターは、これらの各列に SqlDataSource を許可する元の値を保持するためにも作成されます。SelectCommand が実行されるため、値が変更されたかどうかを決定します。

### <a name="deletecommand"></a>DeleteCommand

設定またはデータベースから行を削除するときに使用される SQL 文字列を取得します。 SQL クエリまたはストアド プロシージャの名前を指定できます。

### <a name="deletecommandtype"></a>DeleteCommandType

設定または SQL クエリ (テキスト) またはストアド プロシージャ (ストアド プロシージャ) するか、削除コマンドの種類を取得します。

### <a name="deleteparameters"></a>Deleteparameters の各

SqlDataSource コントロールに関連付けられた SqlDataSourceView オブジェクトの DeleteCommand によって使用されているパラメーターを返します。

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

このプロパティを使用してを CompareAllValues に ConflictDetection プロパティが設定されている場合元の値パラメーターの形式を指定します。 既定では元のパラメーターの値が元のパラメーターと同じ名前になることを意味する {0} です。 つまり、フィールド名が EmployeeID、元の値パラメーターである場合@EmployeeIDです。

### <a name="selectcommand"></a>SelectCommand

設定またはデータベースからデータを取得するために使用する SQL 文字列を取得します。 SQL クエリまたはストアド プロシージャの名前を指定できます。

### <a name="selectcommandtype"></a>SelectCommandType

設定または SQL クエリ (テキスト) またはストアド プロシージャ (ストアド プロシージャ) するか、select コマンドの種類を取得します。

### <a name="selectparameters"></a>SelectParameters

SqlDataSource コントロールに関連付けられた SqlDataSourceView オブジェクトの SelectCommand によって使用されているパラメーターを返します。

### <a name="sortparametername"></a>SortParameterName

取得またはデータ ソース コントロールでデータの並べ替えを取得するときに使用されるストアド プロシージャのパラメーターの名前を設定します。 ストアド プロシージャに SelectCommandType が設定されている場合にのみ有効です。

### <a name="sqlcachedependency"></a>SqlCacheDependency

セミコロンで区切られた、データベースと SQL Server のキャッシュの依存関係で使用されるテーブルを指定する文字列。 (SQL キャッシュ依存関係は以降のモジュールで説明されます)。

### <a name="updatecommand"></a>updateCommand

設定またはデータベース内のデータを更新するときに使用される SQL 文字列を取得します。 SQL クエリまたはストアド プロシージャの名前を指定できます。

### <a name="updatecommandtype"></a>UpdateCommandType

設定または SQL クエリ (テキスト) またはストアド プロシージャ (ストアド プロシージャ) するか、更新コマンドの種類を取得します。

### <a name="updateparameters"></a>UpdateParameters

SqlDataSource コントロールに関連付けられた SqlDataSourceView オブジェクトの UpdateCommand によって使用されているパラメーターを返します。

## <a name="the-accessdatasource-control"></a>AccessDataSource コントロール

AccessDataSource コントロールは、SqlDataSource クラスから派生し、Microsoft Access データベースへのデータ バインドに使用されます。 AccessDataSource コントロールの ConnectionString プロパティは、読み取り専用プロパティです。 ConnectionString プロパティを使用するのではなく DataFile プロパティは、Access データベースを指す次のように使用されます。

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource は System.Data.OleDb を基本 SqlDataSource の ProviderName は常に設定し、Microsoft.Jet.OLEDB.4.0 OLE DB プロバイダーを使用してデータベースに接続します。 AccessDataSource コントロールを使用して、パスワードで保護された Access データベースに接続することはできません。 パスワードで保護されたデータベースに接続した場合は、SqlDataSource コントロールを使用する必要があります。

> [!NOTE]
> アプリで、Web サイト内に格納された access データベースを配置する必要があります\_データ ディレクトリ。 ASP.NET では、参照するには、このディレクトリにファイルが許可されていません。 プロセスのアカウントをアプリに読み取りおよび書き込みのアクセス許可を付与する必要があります\_Access データベースを使用する場合、データ ディレクトリ。


## <a name="the-xmldatasource-control"></a>XmlDataSource コントロール

指定された xmldatasource 上のデータ バインド コントロールを XML データをデータ バインドを使用します。 DataFile プロパティを使用して XML ファイルにバインドすることができますか、データ プロパティを使用する XML 文字列にバインドすることができます。 指定された xmldatasource 上では、バインド可能なフィールドとしての XML 属性を公開します。 属性としては表されない値にバインドする必要がある場合は、XSL 変換を使用する必要があります。 XML データをフィルター処理する XPath 式を使用することもできます。

次の XML ファイルを考慮してください。

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

指定された xmldatasource 上での XPath プロパティを使用することを確認*/方*にフィルターを適用するためにだけ、&lt;人&gt;ノード。 DropDownList、DataTextField プロパティを使用して LastName 属性にデータをバインドします。

XmlDataSource コントロールは読み取り専用の XML データへのデータ バインドに使用される、主は、XML データ ファイルを編集することです。 このような場合、自動挿入、更新、および XML ファイル内の情報の削除が動作しない自動的には他のデータ ソース コントロールに注意してください。 代わりに、XmlDataSource コントロールの次のメソッドを使用してデータを手動で編集するコードを記述する必要があります。

### <a name="getxmldocument"></a>GetXmlDocument

指定された xmldatasource 上で取得した XML コードを含む XmlDocument オブジェクトを取得します。

### <a name="save"></a>保存

データ ソースに戻るには、インメモリ XmlDocument を保存します。

次の 2 つの条件が満たされたときに Save メソッドをのみ使用できることに注意は。

1. 指定された xmldatasource 上は、DataFile プロパティを使用して、メモリ内の XML データにバインドするデータのプロパティではなく XML ファイルにバインドするは。
2. 変換または TransformFile プロパティ経由での変換が指定されていません。

Save メソッドことに同時に複数のユーザーによって呼び出されたときに予期しない結果が得られることにも注意してください。

## <a name="the-objectdatasource-control"></a>ObjectDataSource コントロール

この時点までを説明するデータ ソース コントロールは、データ ソース コントロールがデータ ストアに直接通信する 2 層アプリケーションの最適な選択肢です。 ただし、多くの実際のアプリケーションは多階層アプリケーション データ ソース コントロールがさらに、データ レイヤーと通信するビジネス オブジェクトと通信する必要があります。 これらの状況では、ObjectDataSource、課金内容を適切に設定します。 ソース オブジェクトと組み合わせて、ObjectDataSource が動作します。 ObjectDataSource コントロールは、オブジェクトがインスタンス メソッド (Visual Basic では Shared) の静的メソッドではなく、ソース オブジェクト、指定したメソッドの呼び出しと 1 つの要求のスコープ内ですべてのオブジェクト インスタンスの dispose のインスタンスを作成します。 そのため、オブジェクトは、ステートレスである必要があります。 オブジェクトは取得し、1 つの要求の範囲内のすべての必要なリソースを解放する必要があります。 ObjectDataSource コントロールの ObjectCreating イベントを処理することによって、ソース オブジェクトを作成する方法を制御できます。 ソース オブジェクトのインスタンスを作成し、そのインスタンスには、ObjectDataSourceEventArgs クラスの ObjectInstance プロパティを設定できます。 ObjectDataSource コントロールでは、独自のインスタンスを作成する代わりに ObjectCreating イベントで作成されるインスタンスを使用します。

ObjectDataSource コントロールのソース オブジェクトを取得し、データを変更するのに呼び出せるパブリック静的メソッド (Visual Basic では Shared) を公開する場合、ObjectDataSource コントロールはこれらのメソッドを直接呼び出します。 ObjectDataSource コントロールは、メソッドの呼び出しを行うために、ソース オブジェクトのインスタンスを作成する必要があります、オブジェクトはパラメーターをとらないパブリック コンス トラクターを含める必要があります。 ObjectDataSource コントロールは、ソース オブジェクトの新しいインスタンスを作成するときにこのコンス トラクターを呼び出します。

ソース オブジェクトにパラメーターなしのパブリック コンス トラクターが含まれていない場合は、ObjectDataSource コントロール ObjectCreating イベントで使用されるソース オブジェクトのインスタンスを作成することができます。

## <a name="specifying-object-methods"></a>オブジェクトのメソッドを指定します。

ObjectDataSource コントロールのソース オブジェクトでは、選択、挿入、更新、使用するメソッドをいくつでも含めるしたり、データを削除することができます。 これらのメソッドは、ObjectDataSource コントロールのプロパティを SelectMethod、InsertMethod、UpdateMethod、または DeleteMethod を使用して識別されると、メソッドの名前に基づいて、ObjectDataSource コントロールから呼び出されます。 ソース オブジェクトには、省略可能な SelectCount をメソッドは、データ ソース オブジェクトの合計数のカウントを返す SelectCountMethod プロパティを使用して、ObjectDataSource コントロールによって識別を含めることもできます。 ページングするときに使用するデータ ソースのレコードの合計数を取得する Select メソッドが呼び出された後、ObjectDataSource コントロールは SelectCount メソッドを呼び出します。

## <a name="lab-using-data-source-controls"></a>データ ソース コントロールを使用してラボ

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>手順 1 - SqlDataSource コントロールにデータを表示します。

次の演習では、SqlDataSource コントロールを使用して、Northwind データベースに接続します。 これには、SQL Server 2000 インスタンスに、Northwind データベースへのアクセスがあることが前提とします。

1. 新しい ASP.NET Web サイトを作成します。
2. 新しい web.config ファイルを追加します。

    1. ソリューション エクスプ ローラーでプロジェクトを右クリックし、[新しい項目の追加] をクリックします。
    2. テンプレートの一覧から Web 構成ファイルを選択し、[追加] をクリックします。
3. 編集、 &lt;connectionStrings&gt;セクションの次のようにします。 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. コード ビューに切り替え、ConnectionString 属性および SelectCommand 属性を追加、 &lt;asp: SqlDataSource&gt;次のように制御します。 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. [デザイン] ビューから新しい GridView コントロールを追加します。
6. データ ソースのドロップダウン リストから、GridView タスク メニューで、SqlDataSource1 を選択します。
7. Default.aspx を右クリックし、メニューから ブラウザー ビューで選択します。 保存するように求められたら、[はい] をクリックします。
8. GridView は、Products テーブルからデータを表示します。

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>手順 2 - SqlDataSource コントロールにデータを編集

次の手順および方法を示しますデータをバインドする DropDownList 宣言の構文を使用して制御 DropDownList コントロールで表示されるデータを編集することができます。

1. [デザイン] ビューでは、Default.aspx の GridView コントロールを削除します。 

    **重要な**: ページ上の SqlDataSource コントロールのままにします。
2. Default.aspx に DropDownList コントロールを追加します。
3. ソース ビューに切り替えます。
4. DataSourceId、DataTextField、および DataValueField 属性を追加、 &lt;asp: DropDownList&gt;次のように制御します。 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Default.aspx を保存し、ブラウザーで表示します。 次のドロップダウン リストに、Northwind データベースから製品が含まれているに注意してください。
6. ブラウザーを閉じます。
7. Default.aspx のソース ビューでの DropDownList のコントロールの下の新しいテキスト ボックス コントロールを追加します。 TxtProductName にテキスト ボックスの ID プロパティを変更します。
8. TextBox コントロールの下には、新しいボタン コントロールを追加します。 BtnUpdate およびテキストのプロパティをボタンの ID プロパティを変更**製品の更新名**です。
9. Default.aspx のソース ビューで UpdateCommand プロパティと 2 つの新しい UpdateParameters SqlDataSource タグを付けるよう追加します。 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > このコードで追加したパラメーター (ProductName、ProductID) 2 つの更新は注意してください。 これらのパラメーターは、txtProductName テキスト ボックスの Text プロパティおよび ddlProducts DropDownList の SelectedValue プロパティにマップされます。
10. デザイン ビューに切り替えるし、イベント ハンドラーを追加するボタン コントロールをダブルクリックします。
11. 次のコードを追加、btnUpdate に\_コード をクリックします。 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Default.aspx を右クリックし、ブラウザーで表示を選択します。 すべての変更を保存するように求められたら、[はい] をクリックします。
13. ASP.NET 2.0 の実行時にコンパイル部分クラスを使用します。 有効にするためのコード変更を確認するためにアプリケーションを構築する必要はありません。
14. 次のドロップダウン リストから、製品を選択します。
15. ボックスで、選択した製品の新しい名前を入力し、更新ボタンをクリックします。
16. 製品名は、データベースで更新されます。

## <a name="exercise-3-using-the-objectdatasource-control"></a>手順 3、ObjectDataSource コントロールの使用

この演習では、ObjectDataSource コントロールと、ソース オブジェクトを使用して、Northwind データベースを操作する方法を示します。

1. ソリューション エクスプ ローラーでプロジェクトを右クリックし、[新しい項目の追加] をクリックします。
2. テンプレートの一覧で、Web フォームを選択します。 Object.aspx に名前を変更し、[追加] をクリックします。
3. ソリューション エクスプ ローラーでプロジェクトを右クリックし、[新しい項目の追加] をクリックします。
4. テンプレートの一覧でクラスを選択します。 NorthwindData.cs にクラスの名前を変更し、[追加] をクリックします。
5. クラスをアプリに追加するように求められたら、[はい] をクリックして\_コード フォルダーです。
6. NorthwindData.cs ファイルに次のコードを追加します。 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Object.aspx のソース ビューには、次のコードを追加します。 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. すべてのファイルを保存し、object.aspx を参照します。
9. 詳細の表示、編集従業員、従業員を追加および削除従業員によってインターフェイスと対話します。
