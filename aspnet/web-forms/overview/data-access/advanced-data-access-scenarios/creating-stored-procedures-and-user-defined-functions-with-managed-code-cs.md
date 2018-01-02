---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: "ストアド プロシージャおよびユーザー定義関数を作成するマネージ コード (c#) |Microsoft ドキュメント"
author: rick-anderson
description: "Microsoft SQL Server 2005 は、開発者はマネージ コードからのデータベース オブジェクトの作成を許可する .NET 共通言語ランタイムと統合できます。 このチュートリアルには."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 653c8303691de28b7619c30e773473ffb37f2a61
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>作成するストアド プロシージャおよびユーザー定義関数をマネージ コード (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip)または[PDF のダウンロード](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 は、開発者はマネージ コードからのデータベース オブジェクトの作成を許可する .NET 共通言語ランタイムと統合できます。 このチュートリアルでは、マネージ ストアド プロシージャを作成する方法を示し、Visual Basic または c# のコードでユーザー定義関数を管理します。 Visual Studio のエディションがこのようなマネージ データベース オブジェクトをデバッグすることを許可する方法をも参照してください。


## <a name="introduction"></a>はじめに

Microsoft の SQL Server 2005 のようなデータベースを使用して、 [Transact-Structured クエリ言語 (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL)の挿入、変更、およびデータを取得します。 ほとんどのデータベース システムには、一連の再利用可能な 1 つの単位として実行できる、SQL ステートメントをグループ化構成体が含まれます。 ストアド プロシージャは、1 つの例です。 他にも*ユーザー定義関数*(Udf) は、手順 9. でさらに詳しく検討するコンストラクトです。

基本的に、データのセットを操作するための SQL は設計されています。 `SELECT`、 `UPDATE`、および`DELETE`ステートメントは、本質的に、対応するテーブルのすべてのレコードに適用され、によってのみ制限されます、`WHERE`句。 まだとスカラーのデータを操作するため、一度に 1 つのレコードを操作するために設計された多くの言語機能があります。 [`CURSOR`s](http://www.sqlteam.com/item.asp?ItemID=553)を通じて 1 つずつでループする一連のレコードの対応できるようにします。 文字列操作関数と同様に`LEFT`、 `CHARINDEX`、および`PATINDEX`スカラーのデータを操作します。 SQL は、制御フロー ステートメントのようにも含まれています。`IF`と`WHILE`です。

Microsoft SQL Server 2005 より前のストアド プロシージャおよび Udf でしたのみとして定義する T-SQL ステートメントのコレクション。 SQL Server 2005、ただし、されたように設計との統合を提供する、[共通言語ランタイム (CLR)](https://msdn.microsoft.com/en-us/netframework/aa497266.aspx)、これは、すべての .NET アセンブリで使用されるランタイム。 その結果、ストアド プロシージャおよび Udf は、SQL Server 2005 データベースで作成できますマネージ コードを使用します。 つまり、c# クラスのメソッドとして、ストアド プロシージャまたは UDF を作成できます。 これにより、これらのストアド プロシージャおよび Udf を .NET Framework では、独自のカスタム クラスからの機能を利用できます。

このチュートリアルを見ていきますマネージを作成する方法には、プロシージャおよびユーザー定義関数と、Northwind データベースに統合する方法が格納されています。 開始 s を let!

> [!NOTE]
> マネージ データベース オブジェクトは、対応する SQL をいくつかの利点を提供します。 豊富な言語機能と使いやすさと既存のコードとロジックを再利用する機能は、主な利点です。 マネージ データベース オブジェクトは、手続きロジックが数多くを伴わないデータのセットを使用する場合に、効率が低下する可能性があります。 T-SQL とマネージ コードの使用の詳細についてはメリットに、チェック アウト、[の利点がありますのマネージ コードを使用してデータベース オブジェクトの作成に](https://msdn.microsoft.com/en-us/library/k2e1fb36(VS.80).aspx)です。


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>手順 1: の Northwind データベースの移動`App_Data`

すべてのチュートリアルのこれまでとしての個の web アプリケーションの Microsoft SQL Server 2005 Express Edition のデータベース ファイルを使用した`App_Data`フォルダーです。 データベースを配置する`App_Data`簡略化を配布して、これらのチュートリアルを実行しているすべてのファイルが 1 つのディレクトリ内にあると、チュートリアルをテストする追加の構成手順は必要ありません。

このチュートリアルでは、ただし、let s 移動のうち、Northwind データベース`App_Data`し明示的にそれを SQL Server 2005 Express Edition のデータベース インスタンスを登録します。 データベースには、このチュートリアルの手順を実行おいるときに、`App_Data`フォルダーで、手順の数が行ったはるかに簡単、SQL Server 2005 Express Edition のデータベース インスタンスをデータベースに明示的に登録します。

このチュートリアルでは、ダウンロード ファイルが含まれる、2 つのデータベース -`NORTHWND.MDF`と`NORTHWND_log.LDF`という名前のフォルダーに置かれている -`DataFiles`です。 実行する場合のチュートリアルでは、独自の実装と共に、Visual Studio を終了し、移動、`NORTHWND.MDF`と`NORTHWND_log.LDF`s の web サイトからファイル`App_Data`web サイトの外部でのフォルダーのフォルダーです。 データベース ファイルは別のフォルダーに移動されている SQL Server 2005 Express Edition のデータベース インスタンスと、Northwind データベースを登録する必要があります。 これは、SQL Server Management Studio から実行できます。 非 Express エディションの SQL Server 2005 コンピューターにインストールされている必要がある場合、可能性がありますが既にある Management Studio をインストールします。 コンピューターに SQL Server 2005 Express Edition があるのみをダウンロードしてインストールする場合[Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)です。

SQL Server Management Studio を起動します。 図 1 では、Management Studio を調べれば、どのようなサーバーへの接続に開始します。 サーバー名の localhost \sqlexpress を入力、認証ドロップダウン リストで Windows 認証を選択し、接続 をクリックします。


![適切なデータベース インスタンスに接続します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**図 1**: 適切なデータベース インスタンスに接続


接続が確立している、オブジェクト エクスプ ローラー ウィンドウは、データベース、セキュリティ情報、管理オプション、およびなどを含む SQL Server 2005 Express Edition データベースのインスタンスに関する情報を一覧表示します。

Northwind データベースをアタッチする必要があります、`DataFiles`フォルダー (または任意の場所に移動したこと)、SQL Server 2005 Express Edition のデータベース インスタンスにします。 データベース フォルダーを右クリックし、コンテキスト メニューから、アタッチ オプションを選択します。 これは、データベースのアタッチ ダイアログ ボックスが表示されます。 [追加] ボタンをクリックし、適切なドリルダウン`NORTHWND.MDF`ファイルを開き、[ok] をクリックします。 この時点で、画面は図 2 のようになります。


[![適切なデータベース インスタンスに接続します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**図 2**: 適切なデータベース インスタンスに接続 ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))


> [!NOTE]
> Management Studio での SQL Server 2005 Express Edition インスタンスに接続するときに、データベースのアタッチ ダイアログ ボックスはできません、マイ ドキュメントなどのユーザー プロファイル ディレクトリにドリル ダウンします。 したがって、配置することを確認してください、`NORTHWND.MDF`と`NORTHWND_log.LDF`非ユーザーのプロファイル ディレクトリ内のファイルです。


データベースのアタッチに [ok] ボタンをクリックします。 データベースのアタッチ ダイアログ ボックスは終了し、オブジェクト エクスプ ローラーに単にアタッチされたデータベースが一覧表示する必要がありますようになりました。 データベースのような名前には、Northwind 可能性が高く`9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`です。 データベースを右クリックして、名前の変更 を選択して northwind データベースをデータベースの名前を変更します。


![Northwind データベースをデータベースの名前変更します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**図 3**: northwind データベースをデータベースの名前変更


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>手順 2: Visual Studio での新しいソリューションと SQL Server プロジェクトの作成

SQL Server 2005 でマネージ ストアド プロシージャまたは Udf を作成するには、クラスでの c# コードとしてストアド プロシージャおよび UDF ロジックを書き込みます。 このクラスをアセンブリにコンパイルする必要がありますが、コードを記述すると、(、`.dll`ファイル)、SQL Server データベースにアセンブリを登録およびに対応するメソッドを指す、データベースでストアド プロシージャまたは UDF のオブジェクトを作成アセンブリ。 次の手順すべて手動実行できます。 任意のテキスト エディターでコードを作成、c# コンパイラを使用してコマンドラインからコンパイルすることができます ([`csc.exe`](https://msdn.microsoft.com/en-us/library/ms379563(vs.80).aspx))、それを使用してデータベースを登録、 [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/en-us/library/ms189524.aspx)コマンドまたは管理対象からStudio、ストアド プロシージャまたは同様の方法での UDF のオブジェクトを追加します。 さいわい、Professional およびチームのシステムのバージョン Visual Studio にはでは、これらのタスクを自動化する SQL Server のプロジェクトの種類が含まれます。 このチュートリアルでを進めながら、SQL Server のプロジェクトの種類を使用してマネージ ストアド プロシージャおよび UDF を作成します。

> [!NOTE]
> Visual Web Developer または Visual Studio の Standard edition を使用している場合は、代わりに、手動による方法を使用する必要があります。 手順 13 では、次の手順を手動で実行する詳細な手順を提供します。 次の手順には、使用している Visual Studio のバージョンに関係なく適用しなければならない重要なの SQL Server の構成手順が含まれるので、手順 13 を読み取る前に手順 2. を 12 通してみてです。


Visual Studio を開いてを開始します。 ファイル メニューから新しいプロジェクト ダイアログを表示する新しいプロジェクトを選択します (図 4 を参照してください)。 データベース プロジェクトの種類にドリルダウンし、右側に表示されているテンプレートから新しい SQL Server プロジェクトを作成します。 このプロジェクトに名前を選択しました`ManagedDatabaseConstructs`という名前のソリューションの配置と`Tutorial75`です。


[![新しい SQL Server プロジェクトを作成します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**図 4**: 新しい SQL Server プロジェクトを作成 ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))


ソリューションと SQL Server プロジェクトを作成する新しいプロジェクト ダイアログ ボックスで ok ボタンをクリックします。

SQL Server のプロジェクトは、特定のデータベースに関連付けられています。 その結果、新しい SQL Server プロジェクトを作成した後すぐが要求されたこの情報を指定します。 図 5 は、手順 1. に戻ります、SQL Server 2005 Express Edition のデータベース インスタンスに登録されている Northwind データベースを指すように入力された新しいデータベース参照の追加 ダイアログ ボックスを示します。


![Northwind データベースに SQL Server プロジェクトを関連付ける](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**図 5**: Northwind データベースに SQL Server プロジェクトを関連付ける


マネージ ストアド プロシージャおよび Udf はこのプロジェクト内で作成をデバッグするのには、SQL/CLR デバッグ接続のサポートを有効にする必要があります。 (図 5 と)、新しいデータベースと SQL Server プロジェクトを関連付けるときに、Visual Studio 要求する場合、接続で SQL/CLR のデバッグを有効にすること (図 6 を参照してください)。 [はい] をクリックします。


![SQL/CLR デバッグを有効にします。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**図 6**: SQL/CLR デバッグを有効にします。


この時点で新しい SQL Server プロジェクトをソリューションに追加されているがします。 という名前のフォルダーが含まれている`Test Scripts`という名前のファイルと`Test.sql`プロジェクトで作成されたマネージ データベース オブジェクトのデバッグに使用されます。 手順 12 でのデバッグで見ていきます。

このプロジェクトに、新しいマネージ ストアド プロシージャおよび Udf を追加おできますようになりましたが、実行できるようにする前に s 最初に、既存の web アプリケーション ソリューションです。 [ファイル] メニューから、[追加] オプションを選択し、既存の Web サイトを選択します。 適切な web サイト フォルダーを参照し、[ok] をクリックします。 図 7 では、これを 2 つのプロジェクトを含めるソリューションが更新されます。 web サイトと`ManagedDatabaseConstructs`SQL Server プロジェクト。


![ソリューション エクスプ ローラーに 2 つのプロジェクトが含まれています](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**図 7**: ソリューション エクスプ ローラーに 2 つのプロジェクトが含まれています


`NORTHWNDConnectionString`値`Web.config`で現在参照、`NORTHWND.MDF`ファイルで、`App_Data`フォルダーです。 このデータベースを削除したため`App_Data`ことを明示的に登録し、SQL Server 2005 Express Edition のデータベース インスタンスで適宜更新する必要があります、`NORTHWNDConnectionString`値。 開く、`Web.config`ファイルに web サイトの変更、`NORTHWNDConnectionString`値、接続文字列を読み取るようにする:`Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`です。 この変更後に、 `<connectionStrings>` 」の「 `Web.config` 、次のようになります。


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> 説明したように、[前のチュートリアル](debugging-stored-procedures-cs.md)クライアント アプリケーションから SQL Server オブジェクトをデバッグするときに、ASP.NET web サイトなど接続プールを無効にする必要があります。 上に示す接続文字列は、接続プールを無効になります ( `Pooling=false` )。 ASP.NET web サイトからデバッグをマネージ ストアド プロシージャおよび Udf の予定がない場合は、接続プールを有効にします。


## <a name="step-3-creating-a-managed-stored-procedure"></a>手順 3: マネージを作成するストアド プロシージャ

マネージ ストアド プロシージャを Northwind データベースに追加するには、最初に SQL Server プロジェクト内のメソッドとして、ストアド プロシージャを作成する必要があります。 ソリューション エクスプ ローラーを右クリックし、`ManagedDatabaseConstructs`プロジェクト名と、新しい項目を追加します。 これにより、プロジェクトに追加できるマネージ データベース オブジェクトの種類の一覧を表示する新しい項目の追加 ダイアログ ボックスが表示されます。 図 8 に示す、これが含まれますストアド プロシージャおよびユーザー定義関数、その他。

単にすべての提供が中止されました製品を返すストアド プロシージャを追加することによって開始 s を使用できます。 新しいストアド プロシージャのファイルの名前を付けます`GetDiscontinuedProducts.cs`です。


[![GetDiscontinuedProducts.cs をという名前の新しいストアド プロシージャを追加します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**図 8**: 新しいストアド プロシージャという名前の追加`GetDiscontinuedProducts.cs`([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))


これにより、次のコンテンツで、新しい c# クラス ファイルが作成されます。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

注としてストアド プロシージャが実装されている、`static`メソッド内で、`partial`という名前のクラス ファイル`StoredProcedures`です。 さらに、`GetDiscontinuedProducts`でメソッドを装飾、 `SqlProcedure attribute`、これは、ストアド プロシージャとしてメソッドをマークします。

次のコードを作成、`SqlCommand`オブジェクトと設定、`CommandText`を`SELECT`から列をすべて返すクエリを`Products`製品用のテーブルが`Discontinued`1 equals をフィールドです。 コマンドを実行し、結果をクライアント アプリケーションに送信します。 このコードを追加、`GetDiscontinuedProducts`メソッドです。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

すべてのマネージ データベース オブジェクトへのアクセスがある、 [ `SqlContext`オブジェクト](https://msdn.microsoft.com/en-us/library/ms131108.aspx)呼び出し元のコンテキストを表すです。 `SqlContext`にアクセスできるように、 [ `SqlPipe`オブジェクト](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlpipe.aspx)を介してその[`Pipe`プロパティ](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx)です。 これは、`SqlPipe`オブジェクトは、SQL Server データベースと呼び出し元のアプリケーション間の情報の終端に使用します。 その名前からわかるように、 [ `ExecuteAndSend`メソッド](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx)、渡されたで実行`SqlCommand`オブジェクトと、結果、クライアント アプリケーションに返送します。

> [!NOTE]
> マネージ データベース オブジェクトは、ストアド プロシージャおよび Udf セットベースのロジックではなく、手続き型のロジックを使用するに最適です。 手続き型のロジックでは、行ごとにデータのセットの操作またはスカラーのデータを扱う必要があります。 `GetDiscontinuedProducts`先ほど作成した、ただし、メソッドに手続き型のロジックは関係しません。 そのため、これが理想的には実装する T-SQL ストアド プロシージャとして。 マネージ ストアド プロシージャを作成および展開するために必要な手順を示すために管理対象のストアド プロシージャとして実装されます。


## <a name="step-4-deploying-the-managed-stored-procedure"></a>手順 4: ストアド プロシージャを管理対象の展開

このコードは完全なを使って、Northwind データベースを展開する準備ができました。 SQL Server プロジェクトを配置するとアセンブリにコードをコンパイルで、データベース アセンブリを登録し、データベース、アセンブリ内の適切なメソッドにリンクすることで、対応するオブジェクトを作成します。 展開オプションで実行されるタスクの正確なセットは手順 13 に記述されたより正確にします。 右クリックし、`ManagedDatabaseConstructs`プロジェクトのソリューション エクスプ ローラーの名前と、展開オプションを選択します。 ただし、展開は失敗し、次のエラー: '外部' 付近に構文が正しくありません。 この機能を有効に高い値に、現在のデータベースの互換性レベルを設定する必要があります。 参照してください、ストアド プロシージャについては、`sp_dbcmptlevel`です。

このエラー メッセージは、Northwind データベースにアセンブリを登録しようとするときに発生します。 に SQL Server 2005 データベースを、アセンブリを登録するために、データベースの互換性レベルを 90 に設定する必要があります。 既定では、新しい SQL Server 2005 データベースは 90 の互換性レベルがあります。 ただし、Microsoft SQL Server 2000 を使用して作成されたデータベースには、80 の既定の互換性レベルがあります。 Northwind データベースは、Microsoft SQL Server 2000 データベースでは最初に後の互換性レベル 80 に設定されている現在、したがって増やす必要がある 90 マネージ データベース オブジェクトを登録するために

データベースの互換性レベルを更新するには、Management Studio で新しいクエリ ウィンドウを開き、入力します。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

上記のクエリを実行するには、ツールバーで [Execute] アイコンをクリックします。


[![Northwind データベースの互換性レベルを更新します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**図 9**: Northwind データベースの互換性レベルを更新 ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))


互換性レベルを更新した後は、SQL Server プロジェクトを再展開します。 今度は、展開は、エラーなしで完了する必要があります。

SQL Server Management Studio に戻り、オブジェクト エクスプ ローラーで、Northwind データベースを右クリックし、および更新 を選択します。 次に、プログラミング フォルダーにドリル ダウンし、アセンブリ フォルダーを展開します。 図 10 では、Northwind データベースが含まれていますによって生成されたアセンブリには、`ManagedDatabaseConstructs`プロジェクト。


![ManagedDatabaseConstructs アセンブリは、Northwind データベースに登録されました](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**図 10**:`ManagedDatabaseConstructs`アセンブリは、Northwind データベースに登録されました


また Stored Procedures フォルダーを展開します。 という名前のストアド プロシージャが表示されます`GetDiscontinuedProducts`です。 このストアド プロシージャは、展開プロセスとへのポインターによって作成された、`GetDiscontinuedProducts`メソッドで、`ManagedDatabaseConstructs`アセンブリ。 ときに、`GetDiscontinuedProducts`ストアド プロシージャを実行するとが実行される、`GetDiscontinuedProducts`メソッドです。 マネージ ストアド プロシージャのため、Management Studio では編集できません (そのため、ストアド プロシージャ名の横にあるロック アイコン)。


![GetDiscontinuedProducts ストアド プロシージャは、フォルダーに一覧表示、ストアド プロシージャ](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**図 11**:`GetDiscontinuedProducts`ストアド プロシージャは、フォルダーに一覧表示、ストアド プロシージャ


1 つハードルをマネージ ストアド プロシージャを呼び出すことができます前に解決するにはまだ: マネージ コードの実行を防ぐために、データベースを構成します。 これを新しいクエリ ウィンドウを開きを実行することを確認、`GetDiscontinuedProducts`ストアド プロシージャです。 次のエラー メッセージが表示されます: .NET Framework でのユーザー コードの実行が無効になっています。 Clr enabled 構成オプションを有効にします。

Northwind データベースの構成情報を確認、入力して、コマンドを実行する`exec sp_configure`してクエリ ウィンドウにします。 設定に有効になっている clr が 0 に設定されて現在ことを示します。


[![有効になっている clr の設定は、現在の設定を 0 に](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**図 12**: 有効になっている clr の設定は、現在の設定を 0 に ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))


図 12 にそれぞれの構成設定が示した 4 つの値を持つことに注意してください: 最小値と最大値および構成と実行の値。 Clr を有効になっている設定の構成値を更新するには、次のコマンドを実行します。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

再実行する場合、`exec sp_configure`上記のステートメントが 1 の場合に有効になっている clr の設定の構成値を更新することが、実行値が 0 に設定されていることが表示されます。 この構成の変更を有効にする必要がありますを実行する、 [ `RECONFIGURE`コマンド](https://msdn.microsoft.com/en-us/library/ms176069.aspx)は、実行値は、現在の構成値に設定します。 入力するだけ`RECONFIGURE`クエリ ウィンドウで、ツールバーの Execute のアイコンをクリックします。 実行する場合`exec sp_configure`これで、有効になっている clr の設定の構成では、1 の値を確認して、値を実行する必要があります。

Clr を有効になっている構成が完了する準備が整いましたマネージ実行`GetDiscontinuedProducts`ストアド プロシージャです。 クエリ ウィンドウで入力し、コマンド`exec``GetDiscontinuedProducts`です。 対応するマネージ コードで、ストアド プロシージャを呼び出すと、`GetDiscontinuedProducts`メソッドを実行します。 このコードの問題、`SELECT`提供が中止されたして呼び出し元のアプリケーションは、このインスタンスで SQL Server Management Studio は、このデータを返すすべての製品を返すクエリです。 Management Studio では、これらの結果を受信し、結果ウィンドウに表示します。


[![ストアド プロシージャが返すすべて GetDiscontinuedProducts 製品を生産中止](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**図 13**:`GetDiscontinuedProducts`ストアド プロシージャを返します提供が中止された製品すべて ([フルサイズのイメージを表示するには、をクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))。


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>手順 5:、入力パラメーターを受け取るプロシージャが格納されている管理対象を作成します。

多くのクエリおよびこれらのチュートリアル全体で作成したストアド プロシージャの使用が*パラメーター*です。 たとえば、[を作成する新しいストアド プロシージャを型指定されたデータセットの Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)という名前のストアド プロシージャを作成したチュートリアル`GetProductsByCategoryID`という名前の入力パラメーターを承諾する`@CategoryID`です。 ストアド プロシージャから返されるすべての製品が`CategoryID`フィールドが指定された値に一致した`@CategoryID`パラメーター。

入力パラメーターを受け取るマネージ ストアド プロシージャを作成するには、メソッド定義でこれらのパラメーターを指定だけです。 これを示すためには、let s 追加する別のマネージ ストアド プロシージャ、`ManagedDatabaseConstructs`という名前のプロジェクト`GetProductsWithPriceLessThan`です。 このマネージ ストアド プロシージャは、料金を指定する入力パラメーターを受け入れるし、すべての製品を返すが`UnitPrice`フィールドは、パラメーターの値よりも小さいです。

プロジェクトに新しいストアド プロシージャを追加するを右クリックし、`ManagedDatabaseConstructs`プロジェクト名と、新しいストアド プロシージャを追加します。 そのファイルに `GetProductsWithPriceLessThan.cs` という名前を付けます。 という名前のメソッドで新しい c# クラス ファイルを作成これが手順 3. で説明したとおり、`GetProductsWithPriceLessThan`内に配置された、`partial`クラス`StoredProcedures`です。

更新プログラム、`GetProductsWithPriceLessThan`メソッド定義を受け入れるように、 [ `SqlMoney` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlmoney.aspx)という名前の入力パラメーター`price`を実行し、クエリの結果を返すコードを記述および。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

`GetProductsWithPriceLessThan`定義とのコードによく似たのメソッドの定義とコード、`GetDiscontinuedProducts`手順 3 で作成したメソッド。 唯一の違いは、`GetProductsWithPriceLessThan`メソッドは入力パラメーターとして受け取ります (`price`) では、 `SqlCommand` s クエリにはパラメーターが含まれています (`@MaxPrice`) にパラメーターを追加し、 `SqlCommand` s`Parameters`コレクションと値を割り当て、`price`変数。

このコードを追加した後は、SQL Server プロジェクトを再展開します。 次に、SQL Server Management Studio に戻り、ストアド プロシージャ フォルダーを更新します。 新しいエントリを参照する必要があります`GetProductsWithPriceLessThan`です。 クエリ ウィンドウで、入力し、コマンド`exec GetProductsWithPriceLessThan 25`、図 14 に示すように、25 ドル未満のすべての製品を一覧になります。


[![製品 $25 が表示されます。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**図 14**: $25 で製品が表示されます ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>手順 6: データ アクセス層からのマネージ ストアド プロシージャの呼び出し

この時点で追加しました、`GetDiscontinuedProducts`と`GetProductsWithPriceLessThan`するストアド プロシージャを管理、`ManagedDatabaseConstructs`プロジェクトし、Northwind SQL Server データベースに登録しています。 SQL Server Management Studio からこれらの管理対象のストアド プロシージャを呼び出すことも (図 13 と 14 s を参照してください)。 これらを使用するアプリケーション、asp.net には、ストアド プロシージャが管理されている、ただし、データ アクセスと、アーキテクチャのビジネス ロジック層に追加する必要があります。 この手順では 2 つの新しいメソッドを追加、`ProductsTableAdapter`で、`NorthwindWithSprocs`型指定されたデータセットで最初に作成された、[を作成する新しいストアド プロシージャを型指定されたデータセットの Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)チュートリアルです。 手順 7 では、BLL に、対応するメソッドを追加します。

開く、`NorthwindWithSprocs`型指定されたデータセットに新しいメソッドを追加することで開始して Visual Studio で、`ProductsTableAdapter`という`GetDiscontinuedProducts`です。 TableAdapter に新しいメソッドを追加するには、デザイナーの TableAdapter の名前を右クリックし、コンテキスト メニューから、クエリの追加オプションを選択します。

> [!NOTE]
> Northwind データベースを移動したため、 `App_Data` SQL Server 2005 Express Edition のデータベース インスタンスにフォルダーがこの変更を反映するように、Web.config 内の対応する接続文字列を更新することが不可欠です。 手順 2. で説明したの更新、`NORTHWNDConnectionString`値`Web.config`です。 この更新を忘れた場合と、クエリを追加できませんでしたエラー メッセージが表示されます。 接続が見つかりません`NORTHWNDConnectionString`オブジェクトの`Web.config`TableAdapter に新しいメソッドを追加しようとしています ダイアログ ボックス。 このエラーを解決するには、[ok] をクリックしに移動し、`Web.config`し、更新、`NORTHWNDConnectionString`手順 2 で説明したように値します。 TableAdapter にメソッドを再度追加してみます。 今度は問題なく処理が必要です。


新しいメソッドを追加するには、過去のチュートリアルに何度も使用しています、TableAdapter クエリの構成ウィザードが起動します。 最初の手順では、TableAdapter がデータベースにアクセスする方法を指定するよう求められます。 または、新規または既存のストアド プロシージャを使用して、アドホック SQL ステートメントを使用します。 既に作成して登録されているため、`GetDiscontinuedProducts`で、データベースの管理対象のストアド プロシージャを選択、既存のストアド プロシージャのオプションと [次へ] をクリックします。


[![既存のストアド プロシージャ オプションの使用を選択します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**図 15**: を使用して既存のストアド プロシージャ オプションを選択 ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))


次の画面は、メソッドを呼び出すストアド プロシージャの意見を求めます。 選択、`GetDiscontinuedProducts`ドロップダウン リストからストアド プロシージャを管理し、[次へ] をクリックします。


[![Select、GetDiscontinuedProducts マネージ ストアド プロシージャ](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**図 16**: 選択、`GetDiscontinuedProducts`マネージ ストアド プロシージャ ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))


ストアド プロシージャを行、単一の値、または何も返すかどうかを指定するメッセージが表示されます。 `GetDiscontinuedProducts`セットを返します、提供が中止された製品の行の最初のオプション (表形式のデータ) を選択し、[次へ] をクリックします。


[![表形式のデータ オプションを選択します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**図 17**: 表形式のデータ オプションを選択 ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))


ウィザードの最終画面では、データ アクセス パターンを使用して、結果として得られるメソッドの名前を指定することができます。 チェック ボックスをオンになってと名の両方のメソッドのままに`FillByDiscontinued`と`GetDiscontinuedProducts`です。 ウィザードを完了するには、[完了] をクリックします。


[![名前のメソッド FillByDiscontinued と GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**図 18**: メソッドの名前を付けます`FillByDiscontinued`と`GetDiscontinuedProducts`([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))


という名前のメソッドを作成するには、この手順を繰り返します`FillByPriceLessThan`と`GetProductsWithPriceLessThan`で、`ProductsTableAdapter`の`GetProductsWithPriceLessThan`ストアド プロシージャを管理します。

図 19 にメソッドを追加した後、データセット デザイナーのスクリーン ショットを示しています、`ProductsTableAdapter`の`GetDiscontinuedProducts`と`GetProductsWithPriceLessThan`ストアド プロシージャを管理します。


[![ProductsTableAdapter には、この手順で追加した新しいメソッドが含まれています。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**図 19**:`ProductsTableAdapter`この手順で追加された新しいメソッドが含まれています ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>手順 7: ビジネス ロジック層への対応するメソッドの追加

手順 4. および 5. で追加されたマネージ ストアド プロシージャを呼び出すためのメソッドを含めるデータ アクセス層を更新した、ビジネス ロジック層に対応するメソッドを追加お必要があります。 次の 2 つのメソッドを追加、`ProductsBLLWithSprocs`クラス。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

両方のメソッドだけで、対応する DAL メソッド呼び出しを返す、`ProductsDataTable`インスタンス。 `DataObjectMethodAttribute`と、ObjectDataSource のデータ ソース構成ウィザードの選択 タブで、ドロップダウン リストに含まれるこれらのメソッドの各メソッドの上のマークアップ。

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>手順 8: マネージ呼び出しからのストアド プロシージャ、プレゼンテーション層

呼び出しをサポートするようにビジネス ロジックとデータ アクセス レイヤーで補完された、`GetDiscontinuedProducts`と`GetProductsWithPriceLessThan`マネージ ストアド プロシージャは、これらも表示できる ASP.NET ページを使用してプロシージャ結果を格納します。

開く、 `ManagedFunctionsAndSprocs.aspx`  ページで、`AdvancedDAL`フォルダーと、ツールボックスからデザイナーにドラッグする GridView。 集合 GridView s`ID`プロパティを`DiscontinuedProducts`と、という名前の新しい ObjectDataSource へのバインドのスマート タグから`DiscontinuedProductsDataSource`です。 構成からそのデータをプルする ObjectDataSource、`ProductsBLLWithSprocs`クラスの`GetDiscontinuedProducts`メソッドです。


[![構成、ObjectDataSource ProductsBLLWithSprocs クラスを使用するには](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**図 20**: 構成を使用する ObjectDataSource、`ProductsBLLWithSprocs`クラス ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))


[![GetDiscontinuedProducts 方法を選択 タブで、ドロップダウン リストから選択します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**図 21**: 選択、`GetDiscontinuedProducts`を選択 タブで、ドロップダウン リストからメソッド ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))


このグリッドを表示製品情報だけを使用するため、UPDATE、INSERT でのドロップダウン リストの設定を (なし) タブを削除してし、完了 をクリックします。

ウィザードを完了すると、Visual Studio が自動的に追加 BoundField または CheckBoxField 内の各データ フィールド、`ProductsDataTable`です。 これらのフィールドを除くすべてを削除するには、しばらく時間かかる`ProductName`と`Discontinued`箇所、GridView、ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

ブラウザーからこのページを表示するのにはしばらくを実行します。 ページを開いたとき、ObjectDataSource 呼び出し、`ProductsBLLWithSprocs`クラスの`GetDiscontinuedProducts`メソッドです。 手順 7. で説明したとおり、このメソッド呼び出し DAL s`ProductsDataTable`クラス s`GetDiscontinuedProducts`によって実行されるメソッド、`GetDiscontinuedProducts`ストアド プロシージャです。 このストアド プロシージャは、マネージ ストアド プロシージャと提供が中止された製品を返す、手順 3. で作成したコードを実行します。

マネージ ストアド プロシージャによって返される結果にパッケージ化、 `ProductsDataTable` DAL によってし、プレゼンテーション層 GridView にバインドされているし、表示の場所に戻ります BLL に返されます。 期待どおりに、グリッドには、提供が中止されましたこれらの製品が一覧表示します。


[![廃止された製品の一覧が表示されます。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**図 22**:「提供が中止された製品の一覧が表示されます ([フルサイズのイメージを表示するには、をクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))


さらに確保するためには、テキスト ボックスと別の GridView をページに追加します。 この GridView を呼び出すことによって、テキスト ボックスに入力された金額未満の製品の表示がある、`ProductsBLLWithSprocs`クラスの`GetProductsWithPriceLessThan`メソッドです。

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>手順 9: を作成して、T-SQL で Udf を呼び出す

ユーザー定義関数、または Udf はデータベースのオブジェクトを厳密に模倣プログラミング言語の関数のセマンティクスです。 C# の場合、関数のように Udf は、入力パラメーター数が変数を含めるし、特定の型の値を返します。 UDF は、文字列、整数、およびなど、または表形式のデータでのいずれかのスカラーのデータを返すことができます。 両方の種類のユーザー定義関数、スカラー データ型を返す UDF で始まるをクイック見て s を使用できます。

次の UDF では、特定の製品の在庫の推定値を計算します。 これは 3 つの入力パラメーターの取得によって、 `UnitPrice`、 `UnitsInStock`、および`Discontinued`- 特定の製品の値し、型の値を返します`money`です。 乗算することによって、在庫の推定値を計算、`UnitPrice`によって、`UnitsInStock`です。 この値は、提供が中止された項目の半分になります。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

この UDF は、データベースに追加されるにあります Management Studio を使用、プログラミング フォルダー、し、関数、および、スカラー値関数を展開します。 使用できます、`SELECT`クエリ次のようにします。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

追加した、 `udf_ComputeInventoryValue` Northwind データベースに UDF図 23 は、上記の出力を示しています。 `SELECT` Management Studio を通じて表示されるときにクエリを実行します。 また、UDF が、スカラー値関数、オブジェクト エクスプ ローラーでフォルダーの下に表示されていることに注意してください。


[![各製品の在庫の値が表示されています。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**図 23**: インベントリの値が表示されている各製品 s ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))


Udf では、表形式のデータを返すこともできます。 たとえば、特定のカテゴリに属している製品を返す UDF を生み出すことができます。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

`udf_GetProductsByCategoryID` UDF を受け入れる、`@CategoryID`入力パラメーターと、指定の結果を返します。`SELECT`クエリ。 この UDF の参照を作成した後、 `FROM` (または`JOIN`) の句、`SELECT`クエリ。 次の例を返します、 `ProductID`、 `ProductName`、および`CategoryID`飲み物の値。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

追加した、 `udf_GetProductsByCategoryID` Northwind データベースに UDF図 24 は、上記の出力を示しています。 `SELECT` Management Studio を通じて表示されるときにクエリを実行します。 表形式のデータを返す Udf は s テーブル値関数のオブジェクト エクスプ ローラー フォルダーにあります。


[![ProductID、ProductName、および [categoryid] は、各飲み物の一覧表示されます。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**図 24**: `ProductID`、 `ProductName`、および`CategoryID`Each 飲み物の一覧表示されます ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))


> [!NOTE]
> 作成し、Udf を使用する詳細については、チェック アウト[紹介し、ユーザー定義関数](http://www.sqlteam.com/item.asp?ItemID=1955)です。 チェック アウトも[長所と Drawbacks of User-Defined 関数](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)です。


## <a name="step-10-creating-a-managed-udf"></a>手順 10: マネージ UDF を作成します。

`udf_ComputeInventoryValue`と`udf_GetProductsByCategoryID`前の例で作成される Udf は、T-SQL でデータベース オブジェクトです。 SQL Server 2005 には、管理対象の Udf に追加することができますもサポートしています、`ManagedDatabaseConstructs`プロジェクトのマネージ ストアド プロシージャの手順 3 から 5 と同じようにします。 このステップの s を実装できるように、`udf_ComputeInventoryValue`マネージ コードでの UDF です。

マネージ UDF を追加する、`ManagedDatabaseConstructs`プロジェクトをソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加を選択します。 [新しい項目の追加] ダイアログ ボックスからユーザー定義テンプレートを選択し、名前の新しい UDF ファイル`udf_ComputeInventoryValue_Managed.cs`です。


[![新しいマネージ UDF を ManagedDatabaseConstructs プロジェクトに追加します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**図 25**: 追加する新しい管理 UDF、`ManagedDatabaseConstructs`プロジェクト ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))


ユーザー定義関数のテンプレートを作成、`partial`という名前のクラス`UserDefinedFunctions`クラス ファイル名と同じ名前を持つメソッドに (`udf_ComputeInventoryValue_Managed`、このインスタンスで)。 使用してこのメソッドを装飾、 [ `SqlFunction`属性](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx)、マネージ UDF としてメソッドをフラグを設定します。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

`udf_ComputeInventoryValue`メソッドは現在を返します、 [ `SqlString`オブジェクト](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlstring.aspx)任意の入力パラメーターは受け付けられません。 3 つの入力パラメーターを受け取るように、メソッドの定義を更新する必要があります`UnitPrice`、 `UnitsInStock`、および`Discontinued`- しを返します、`SqlMoney`オブジェクト。 インベントリの値を計算するためのロジックは、T-SQL と同じ`udf_ComputeInventoryValue`UDF です。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

UDF のメソッドの入力パラメーターが、対応する SQL 型のメモ:`SqlMoney`の`UnitPrice`フィールド、 [ `SqlInt16` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlint16.aspx)の`UnitsInStock`、および[ `SqlBoolean` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlboolean.aspx)`Discontinued`します。 これらのデータ型で定義された型の反映、`Products`テーブル:`UnitPrice`型の列は、 `money`、`UnitsInStock`型の列`smallint`、および`Discontinued`型の列`bit`です。

作成することで、コードの開始、`SqlMoney`という名前のインスタンス`inventoryValue`0 の値を割り当てられています。 `Products`データベースでは、テーブル`NULL`の値が、`UnitsInPrice`と`UnitsInStock`列です。 そのため、必要があります最初のチェックにこれらの値が含まれているかどうかを`NULL`を行って、s、`SqlMoney`オブジェクト s [ `IsNull`プロパティ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlmoney.isnull.aspx)です。 両方`UnitPrice`と`UnitsInStock`が含まれていない`NULL`値を計算し、 `inventoryValue` 2 つの製品であることにします。 その後、if`Discontinued`が true の場合、値を半分おです。

> [!NOTE]
> `SqlMoney`オブジェクトのみが許可 2`SqlMoney`インスタンスに一緒に乗算されます。 できません、`SqlMoney`インスタンスにリテラルの浮動小数点数が乗算されます。 したがって、半分にする`inventoryValue`新しいで乗算お`SqlMoney`値 0.5 のあるインスタンスです。


## <a name="step-11-deploying-the-managed-udf"></a>手順 11: 管理対象の UDF を展開します。

これで、マネージ UDF を作成したら、Northwind データベースに配置する準備ができました。 手順 4 で説明したとおり、SQL Server プロジェクト内のマネージ オブジェクトは、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、コンテキスト メニューから [展開] を選択して展開されます。

プロジェクトを配置した後は、SQL Server Management Studio に戻り、スカラー値関数のフォルダーを更新しています。 2 つのエントリが表示されます。

- `dbo.udf_ComputeInventoryValue`-手順 9. で T-SQL UDF を作成し、
- `dbo.udf ComputeInventoryValue_Managed`-だけに展開された手順 10 で管理対象の UDF を作成します。

このマネージ UDF をテストするには、Management Studio 内から次のクエリを実行します。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

このコマンドは、マネージ`udf ComputeInventoryValue_Managed`、T-SQL ではなく UDF `udf_ComputeInventoryValue` UDF が、この出力は、同じです。 UDF の出力のスクリーン ショットを参照する図 23 を参照します。

## <a name="step-12-debugging-the-managed-database-objects"></a>手順 12: マネージ データベース オブジェクトのデバッグ

[ストアド プロシージャのデバッグ](debugging-stored-procedures-cs.md)デバッグ Visual Studio での SQL Server のオプションは、3 つに説明したチュートリアル: ダイレクト データベース デバッグ、アプリケーションのデバッグ、および SQL Server プロジェクトからデバッグできます。 クライアント アプリケーションと SQL Server プロジェクトから直接、データベースのオブジェクトは、ダイレクト データベース デバッグ経由でデバッグすることはできませんが、デバッグすることができますを管理します。 デバッグを行うためには、ただし、SQL Server 2005 データベース必要があります SQL/CLR のデバッグを許可します。 初めて作成されるときに注意してください、`ManagedDatabaseConstructs`プロジェクトが Visual Studio をご希望 SQL/CLR (手順 2. で図 6 を参照) のデバッグを有効にする必要かどうか。 この設定は、サーバー エクスプ ローラー ウィンドウからデータベースを右クリックして変更できます。


![データベースが SQL/CLR デバッグを許可します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**図 26**: で、データベースが SQL/CLR デバッグを許可します。


デバッグするかをかを想像、`GetProductsWithPriceLessThan`ストアド プロシージャを管理します。 コード内でブレークポイントを設定することは、まず、`GetProductsWithPriceLessThan`メソッドです。


[![GetProductsWithPriceLessThan メソッドにブレークポイントを設定します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**図 27**: にブレークポイントを設定、`GetProductsWithPriceLessThan`メソッド ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))


まず、SQL Server のプロジェクトからのマネージ データベース オブジェクトのデバッグについて見て s を使用できます。 2 つのプロジェクトがソリューションに含まれているため、 `ManagedDatabaseConstructs` SQL Server プロジェクトを Visual Studio を起動するように指示する必要があります、SQL Server プロジェクトからデバッグするために、web サイトと共に、 `ManagedDatabaseConstructs` SQL Server のプロジェクトのデバッグを開始するとします。 右クリックし、`ManagedDatabaseConstructs`ソリューション エクスプ ローラーでプロジェクトし、コンテキスト メニューからスタートアップ プロジェクト オプションとして、セットを選択します。

ときに、`ManagedDatabaseConstructs`内の SQL ステートメントを実行する、デバッガーからプロジェクトを起動、`Test.sql`にあるファイル、`Test Scripts`フォルダーです。 例については、テスト、`GetProductsWithPriceLessThan`マネージ ストアド プロシージャは、既存の置換`Test.sql`ファイルのコンテンツは、次のステートメントを呼び出すと、`GetProductsWithPriceLessThan`を渡してストアド プロシージャを管理、 `@CategoryID` 14.95 の値。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

入力されるには、上記のスクリプトとしている`Test.sql`デバッグ メニューに移動し、デバッグ開始 を選択するか、f5 キーを押すのデバッグを開始、または、ツールバーの緑色の再生 アイコン。 これは、ソリューション内でプロジェクトをビルド、Northwind データベースへのマネージ データベース オブジェクトを展開し、し、実行、`Test.sql`スクリプト。 この時点では、ブレークポイントにヒットして、ステップ実行により、お、`GetProductsWithPriceLessThan`メソッドは、入力パラメーターの値を確認しなどです。


[![GetProductsWithPriceLessThan メソッド内のブレークポイントがヒットしました](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**図 28**: で、ブレークポイント、`GetProductsWithPriceLessThan`メソッドにヒットした ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))


SQL データベース オブジェクトをクライアント アプリケーションをデバッグするためには、アプリケーションのデバッグをサポートするために、データベースを構成することが不可欠です。 サーバー エクスプ ローラーでデータベースを右クリックし、アプリケーションのデバッグ オプションはオンになっていることを確認してください。 さらに、SQL デバッガーに統合して、接続プールを無効にするのには、ASP.NET アプリケーションを構成する必要があります。 次の手順のステップ 2 で詳しく説明された、[ストアド プロシージャのデバッグ](debugging-stored-procedures-cs.md)チュートリアルです。

ASP.NET アプリケーションとデータベースを構成した場合は、ASP.NET web サイト、スタートアップ プロジェクトとして設定し、デバッグを開始します。 ブレークポイントがある管理対象のオブジェクトの 1 つを呼び出すページにアクセスした場合、アプリケーションは中断され、コントロールに引き継がれる、デバッガーでステップ実行できますコード図 28 で示すようにします。

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>手順 13: 手動でのコンパイルと展開マネージ データベース オブジェクト

SQL Server のプロジェクトしやすいように作成、コンパイル、およびマネージ データベース オブジェクトを展開します。 残念ながら、SQL Server のプロジェクトは Visual Studio の Professional およびチームのシステムのエディションで使用できます。 Visual Web Developer または Standard エディションの Visual Studio を使用してマネージ データベース オブジェクトを使用する場合は、手動で作成して展開する必要があります。 これには、4 つの手順が含まれます。

1. マネージ データベース オブジェクトのソース コードを含むファイルを作成します。
2. オブジェクトをアセンブリにコンパイルします。
3. SQL Server 2005 データベースにアセンブリを登録し、
4. アセンブリ内の適切なメソッドを指す SQL Server でデータベース オブジェクトを作成します。

これらのタスクを示しています、s を新規作成管理をこれらの製品を返すストアド プロシージャが`UnitPrice`が指定された値より大きい。 という名前のコンピューターに新しいファイルを作成`GetProductsWithPriceGreaterThan.cs`ファイルに次のコードを入力し、(Visual Studio、メモ帳または任意のテキスト エディターを行えますこれを実現)。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

このコードは、ほとんどの場合と同じ、`GetProductsWithPriceLessThan`手順 5. で作成したメソッド。 唯一の違いは、メソッド名、`WHERE`句、およびクエリで使用されるパラメーター名。 戻り、 `GetProductsWithPriceLessThan` 、メソッド、`WHERE`句を読み取る:`WHERE UnitPrice < @MaxPrice`です。 ここで、`GetProductsWithPriceGreaterThan`を使用:`WHERE UnitPrice > @MinPrice`です。

これで、このクラスをアセンブリにコンパイルする必要があります。 保存したディレクトリに移動し、コマンドラインから、`GetProductsWithPriceGreaterThan.cs`ファイルし、c# コンパイラを使用して (`csc.exe`)、アセンブリにクラス ファイルをコンパイルします。


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

場合を含むフォルダー`csc.exe`で s システムではなく`PATH`、完全パスを参照する必要がある`%WINDOWS%\Microsoft.NET\Framework\version\`、次のようにします。


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![GetProductsWithPriceGreaterThan.cs をアセンブリにコンパイルします。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**図 29**: コンパイル`GetProductsWithPriceGreaterThan.cs`、アセンブリに ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))


`/t`フラグは、c# クラス ファイルが (実行可能ファイルではなく)、DLL にコンパイルすることを指定します。 `/out`フラグは、生成されたアセンブリの名前を指定します。

> [!NOTE]
> コンパイルではなく、`GetProductsWithPriceGreaterThan.cs`または使用してコマンドラインからのクラス ファイル[Visual c# Express Edition](https://msdn.microsoft.com/vstudio/express/visualcsharp/)または Visual Studio Standard Edition で別のクラス ライブラリ プロジェクトを作成します。 S ren 一 Lauritsen がこのような Visual c# Express Edition を使用したプロジェクトのコードを指定してください、`GetProductsWithPriceGreaterThan`ストアド プロシージャは、2 つの管理ストアド プロシージャおよび UDF は、3、5、および 10 の手順で作成します。 S ren s プロジェクトには、対応するデータベース オブジェクトを追加するために必要な T-SQL コマンドも含まれています。


アセンブリにコンパイルのコードで、SQL Server 2005 データベース内でアセンブリを登録する準備ができました。 これは、コマンドを使用して、T-SQL で実行できる`CREATE ASSEMBLY`、または SQL Server Management Studio を使用します。 Management Studio の使用注目を使用できます。

Management Studio から、Northwind データベース プログラミング フォルダーを展開します。 アセンブリは、そのサブ フォルダーのいずれか。 データベースを新しいアセンブリを手動で追加するには、アセンブリ フォルダーを右クリックし、コンテキスト メニューから新しいアセンブリを選択します。 この表示では、新しいアセンブリ ダイアログ ボックス (図 30) します。 [参照] ボタンをクリックして、`ManuallyCreatedDBObjects.dll`アセンブリおだけコンパイルされ、データベースにアセンブリを追加するには、[ok] をクリックします。 表示しないように、`ManuallyCreatedDBObjects.dll`オブジェクト エクスプ ローラーでアセンブリ。


[![ManuallyCreatedDBObjects.dll アセンブリをデータベースに追加します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**図 30**: 追加、`ManuallyCreatedDBObjects.dll`データベースにアセンブリ ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))


![オブジェクト エクスプ ローラーで、ManuallyCreatedDBObjects.dll が一覧表示します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**図 31**:`ManuallyCreatedDBObjects.dll`オブジェクト エクスプ ローラーで一覧表示


Northwind データベースにアセンブリを追加しました中にまだに関連付ける必要があるストアド プロシージャ、`GetProductsWithPriceGreaterThan`アセンブリ内のメソッドです。 これを実現するには、新しいクエリ ウィンドウを開き、次のスクリプトを実行します。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

これは、Northwind データベースの名前付きで、新しいストアド プロシージャが作成されます`GetProductsWithPriceGreaterThan`マネージ メソッドに関連付けます`GetProductsWithPriceGreaterThan`(クラスである`StoredProcedures`が、アセンブリ内にある`ManuallyCreatedDBObjects`)。

上記のスクリプトを実行すた後には、オブジェクト エクスプ ローラーでストアド プロシージャ フォルダーを更新します。 ストアド プロシージャの新しいエントリが表示されます`GetProductsWithPriceGreaterThan`の隣に鍵のアイコンを持ちます。 このストアド プロシージャをテストするには、入力し、クエリ ウィンドウで、次のスクリプトを実行します。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

上記のコマンドが製品の情報を表示する図 32 が示すような`UnitPrice`$24.95 より大きい。


[![オブジェクト エクスプ ローラーで、ManuallyCreatedDBObjects.dll が一覧表示します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**図 32**:`ManuallyCreatedDBObjects.dll`オブジェクト エクスプ ローラーに表示されます ([フルサイズのイメージを表示するをクリックして](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))


## <a name="summary"></a>概要

Microsoft SQL Server 2005 では、統合と、共通言語ランタイム (CLR)、マネージ コードを使用して作成するデータベース オブジェクトを許可するを提供します。 以前は、T-SQL を使用してこれらのデータベース オブジェクトを作成する可能性がありますのみが、今すぐ .NET プログラミング言語と同様に c# を使用してこれらのオブジェクトを作成できます。 作成したこのチュートリアルでは 2 つのマネージ ストアド プロシージャとマネージ ユーザー定義関数です。

Visual Studio の SQL Server のプロジェクトの種類には、作成、コンパイル、およびマネージ データベース オブジェクトの配置が容易になります。 さらに、高度なデバッグ サポートを提供します。 ただし、SQL Server のプロジェクトの種類では、Visual Studio の Professional およびチームのシステムのエディションで利用できるのみです。 それらの Visual Web Developer または Standard エディションの Visual Studio、作成、コンパイル、および展開の手順を使用する必要があります手動で実行する手順 13 で示したようにします。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ユーザー定義関数の利点と欠点](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [マネージ コードでの SQL Server 2005 のオブジェクトの作成](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [SQL Server 2005 のマネージ コードを使用してトリガーを作成します。](http://www.15seconds.com/issue/041006.htm)
- [方法: を作成および CLR SQL を実行するサーバーのストアド プロシージャ](https://msdn.microsoft.com/en-us/library/5czye81z(VS.80).aspx)
- [作成および CLR の SQL Server のユーザー定義関数を実行する方法](https://msdn.microsoft.com/en-us/library/w2kae45k(VS.80).aspx)
- [方法: 編集、 `Test.sql` SQL オブジェクトを実行するスクリプト](https://msdn.microsoft.com/en-us/library/ms233682(VS.80).aspx)
- [紹介し、ユーザー定義関数](http://www.sqlteam.com/item.asp?ItemID=1955)
- [マネージ コードと SQL Server 2005 (ビデオ)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [TRANSACT-SQL リファレンス](https://msdn.microsoft.com/en-us/library/aa299742(SQL.80).aspx)
- [チュートリアル: マネージ コードでのストアド プロシージャの作成](https://msdn.microsoft.com/en-us/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が S ren 一 Lauritsen しました。 この記事では、S ren も確認するだけでなくマネージ データベース オブジェクトを手動でコンパイルするためには、この記事のダウンロードに含まれている Visual c# Express Edition プロジェクトを作成します。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](debugging-stored-procedures-cs.md)
[次へ](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
