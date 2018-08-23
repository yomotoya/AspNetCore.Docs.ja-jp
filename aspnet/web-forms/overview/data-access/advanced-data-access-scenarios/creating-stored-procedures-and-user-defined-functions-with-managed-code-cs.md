---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: ストアド プロシージャおよびユーザー定義関数を作成するマネージ コード (c#) |Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 は、開発者がマネージ コードからデータベース オブジェクトを作成する .NET 共通言語ランタイムと統合します。 このチュートリアルには.
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: fb4a867d5868e8000fcd10130401a9e169b6f49f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838975"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>ストアド プロシージャとマネージ コード (c#) でユーザー定義関数を作成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip)または[PDF のダウンロード](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 は、開発者がマネージ コードからデータベース オブジェクトを作成する .NET 共通言語ランタイムと統合します。 このチュートリアルでは、マネージ ストアド プロシージャを作成する方法を示していて、ユーザー定義関数は、Visual Basic または c# のコードを管理します。 これらのエディションの Visual Studio を使用すると、このようなマネージ データベース オブジェクトをデバッグする方法をも参照してください。


## <a name="introduction"></a>はじめに

Microsoft の SQL Server 2005 のようなデータベースを使用して、 [Transact-Structured クエリ言語 (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL)の挿入、変更、およびデータを取得します。 ほとんどのデータベース システムには、一連の再利用可能な 1 つの単位として実行できる、SQL ステートメントをグループ化するための構成要素が含まれます。 ストアド プロシージャは、1 つの例です。 もう 1 つは*ユーザー定義関数*(Udf) は、手順 9 でさらに詳しく検討する構造体。

基本的には、データのセットを操作するための SQL は設計されています。 `SELECT`、 `UPDATE`、および`DELETE`ステートメントは本質的に対応するテーブル内のすべてのレコードに適用され、によってのみ制限されますが、`WHERE`句。 まだとスカラーのデータを操作するため、一度に 1 つのレコードを操作するために設計された多くの言語機能があります。 [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553)一度に 1 つをループするレコードのセットを許可します。 文字列操作関数のような`LEFT`、 `CHARINDEX`、および`PATINDEX`スカラーのデータを操作します。 SQL は、のような制御フロー ステートメントも含まれています。`IF`と`WHILE`します。

Microsoft SQL Server 2005 では、前にストアド プロシージャおよび Udf でしたのみとして定義する T-SQL ステートメントのコレクション。 ただし、SQL Server 2005 と統合できる設計されました、[共通言語ランタイム (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx)、これは、すべての .NET アセンブリで使用されるランタイム。 その結果、ストアド プロシージャおよび SQL Server 2005 データベースでの Udf を作成できますマネージ コードを使用します。 つまり、c# クラスのメソッドとしてストアド プロシージャまたは UDF を作成できます。 これにより、これらのストアド プロシージャ、Udf では、.NET Framework と、独自のカスタム クラスから機能を利用できます。

説明はこのチュートリアルでは管理対象を作成する方法には、手順およびユーザー定義関数と、Northwind データベースに統合する方法が格納されています。 Let s を始めましょう。

> [!NOTE]
> マネージ データベース オブジェクトは、対応する SQL 経由でいくつかの利点を提供します。 豊富な言語機能と使いやすさと既存のコードとロジックを再利用する機能は、主な利点です。 マネージ データベース オブジェクトは多くの手続き型のロジックを含まないデータのセットを使用する場合に、効率が低下する可能性があります。 T-SQL とマネージ コードの使用の詳細についてはメリットに、チェック アウト、[の利点がありますを使用してマネージ コードのデータベース オブジェクトを作成する](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx)します。


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>手順 1: の Northwind データベースの移動`App_Data`

すべてチュートリアルのこれまでとして web アプリケーションの s で Microsoft SQL Server 2005 Express Edition のデータベース ファイルを使用が`App_Data`フォルダー。 データベースを配置する`App_Data`を配布して、1 つのディレクトリ内に配置された追加の構成手順、チュートリアルをテストする必要はありませんすべてのファイルと、これらのチュートリアルを実行している簡略化します。

このチュートリアルでは、ただし、let s 移動の Northwind データベース`App_Data`し、SQL Server 2005 Express Edition のデータベース インスタンスに明示的に登録します。 中に、データベースで、このチュートリアルの手順を実行できます、`App_Data`フォルダー、さまざまな手順が行ったはるかに簡単です、データベースを SQL Server 2005 Express Edition のデータベース インスタンスを明示的に登録します。

このチュートリアルでは、ダウンロードが 2 つのデータベース ファイル -`NORTHWND.MDF`と`NORTHWND_log.LDF`という名前のフォルダーに置かれている -`DataFiles`します。 Visual Studio を閉じるし、移動のチュートリアルでは、独自の実装と共にフォローしている場合、`NORTHWND.MDF`と`NORTHWND_log.LDF`s の web サイトからファイル`App_Data`web サイトの外部でのフォルダーのフォルダー。 別のフォルダーに移動されているデータベース ファイルと Northwind データベースを SQL Server 2005 Express Edition のデータベース インスタンスを登録する必要があります。 これは、SQL Server Management Studio から実行できます。 非-Express Edition の SQL Server 2005 コンピューターにインストールされている必要がある場合、可能性がありますが既にある Management Studio をインストールします。 場合のみ SQL Server 2005 Express Edition をコンピューターにある ダウンロードしてインストールする少し[Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)します。

SQL Server Management Studio を起動します。 図 1 に示すように接続するには、どのようなサーバーで Management Studio を開始します。 サーバー名を localhost \sqlexpress を入力、認証ドロップダウン リストで、[Windows 認証を選択および接続] をクリックします。


![適切なデータベース インスタンスに接続します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**図 1**: 適切なデータベース インスタンスに接続します。


Ve を接続すると、オブジェクト エクスプ ローラー ウィンドウは、データベース、セキュリティ情報、管理オプション、およびなどを含む SQL Server 2005 Express Edition データベースのインスタンスに関する情報を一覧表示します。

Northwind データベースをアタッチする必要があります、`DataFiles`フォルダー (または任意の場所に移動したこと)、SQL Server 2005 Express Edition のデータベース インスタンスにします。 データベース フォルダーを右クリックし、コンテキスト メニューから、アタッチ オプションを選択します。 これは、データベースのアタッチ ダイアログ ボックスが表示されます。 [追加] ボタンをクリックして、適切なドリルダウン`NORTHWND.MDF`ファイルを開き、[ok] をクリックします。 この時点で、画面は図 2 のようなはずです。


[![適切なデータベース インスタンスに接続します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**図 2**: 適切なデータベース インスタンスに接続 ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))。


> [!NOTE]
> Management Studio を使用して SQL Server 2005 Express Edition インスタンスに接続するときに、データベースのアタッチ ダイアログ ボックスはできません、マイ ドキュメントなどのユーザー プロファイル ディレクトリにドリルダウンできます。 そのため、確認を配置、`NORTHWND.MDF`と`NORTHWND_log.LDF`以外のユーザー プロファイル ディレクトリ内のファイル。


データベースのアタッチに [ok] ボタンをクリックします。 データベースのアタッチ ダイアログ ボックスが閉じられ、オブジェクト エクスプ ローラーは、単に接続されたデータベースを表示する必要がありますようになりました。 Northwind データベースのような名前が`9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`します。 データベースで右クリックし、名前の変更を選択して、Northwind にデータベースを変更します。


![Northwind データベースをデータベースの名前変更します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**図 3**: northwind データベースをデータベースの名前変更


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>手順 2: Visual Studio での新しいソリューションと SQL Server プロジェクトの作成

SQL Server 2005 でマネージ ストアド プロシージャまたは Udf を作成するには、ストアド プロシージャと UDF のロジックをクラスでの c# コードとしてに記述されます。 このクラスをアセンブリにコンパイルする必要がありますが、コードを記述すると、(、`.dll`ファイル)、SQL Server データベースにアセンブリを登録およびに対応するメソッドを指す、データベースでストアド プロシージャまたは UDF のオブジェクトを作成アセンブリ。 次の手順はすべて手動で実行します。 任意のテキスト エディターでコードを作成、c# コンパイラを使用してコマンドラインからコンパイルすることができます ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx))、それを使用してデータベースを登録、 [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx)コマンドまたは管理Studio、ストアド プロシージャまたは同様の方法での UDF のオブジェクトを追加します。 さいわい、Visual Studio の Professional およびチーム システムのバージョンには、これらのタスクを自動化する SQL Server のプロジェクトの種類が含まれます。 このチュートリアルでは説明します、SQL Server のプロジェクトの種類を使用してマネージ ストアド プロシージャと UDF を作成します。

> [!NOTE]
> Visual Web Developer edition または Visual Studio の Standard edition を使用している場合は、代わりに、手動のアプローチを使用する必要があります。 手順 13 では、次の手順を手動で実行するための詳細な手順を提供します。 次の手順を使用する Visual Studio のバージョンでも適用する必要がありますの重要な SQL Server の構成手順が含まれるので、手順 13 を読む前に 12 までの手順 2 を読み取ることをお勧めします。


Visual Studio を開いてを開始します。 ファイル メニューから新しいプロジェクト ダイアログを表示する新しいプロジェクトを選択ボックス (図 4 参照)。 データベース プロジェクトの種類にドリルダウンし、右側に表示されているテンプレートから新しい SQL Server プロジェクトを作成します。 このプロジェクトの名前を選択しました`ManagedDatabaseConstructs`という名前のソリューション内に配置、および`Tutorial75`します。


[![新しい SQL Server プロジェクトを作成します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**図 4**: 新しい SQL Server プロジェクトの作成 ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))。


ソリューションと SQL Server プロジェクトを作成する新しいプロジェクト ダイアログ ボックスで ok ボタンをクリックします。

SQL Server のプロジェクトは、特定のデータベースに関連付けられます。 その結果、新しい SQL Server プロジェクトを作成した後すぐが要求されたこの情報を指定します。 図 5 は、手順 1. で SQL Server 2005 Express Edition のデータベース インスタンスで登録した Northwind データベースを指すように入力された新しいデータベースの参照ダイアログ ボックスを示します。


![SQL Server のプロジェクトに Northwind データベースを関連付ける](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**図 5**: Northwind データベースに SQL Server のプロジェクトを関連付ける


マネージ ストアド プロシージャおよび Udf は、このプロジェクト内で作成デバッグ、SQL または CLR デバッグの接続のサポートを有効にする必要があります。 (図 5 と)、新しいデータベースと SQL Server のプロジェクトを関連付けるときに、Visual Studio 求められたとき、接続で SQL/CLR のデバッグを有効にするかどうか (図 6 参照)。 [はい] をクリックします。


![SQL/CLR デバッグを有効にします。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**図 6**: SQL/CLR デバッグを有効にします。


この時点で新しい SQL Server のプロジェクトがソリューションに追加されています。 という名前のフォルダーが含まれている`Test Scripts`という名前のファイルと`Test.sql`プロジェクトで作成したマネージ データベース オブジェクトのデバッグに使用されます。 手順 12 でのデバッグを紹介します。

このプロジェクトに、新しいマネージ ストアド プロシージャおよび Udf を追加しましたできますようになりましたが、実行できるようにする前にまずが含まれます、既存の web アプリケーションにはソリューション。 [ファイル] メニューから追加のオプションを選択し、既存の Web サイトを選択します。 適切な web サイトのフォルダーを参照し、[ok] をクリックします。 図 7 に示す、これを 2 つのプロジェクトを含めるソリューションが更新されます。 web サイト、および`ManagedDatabaseConstructs`SQL Server プロジェクト。


![ソリューション エクスプ ローラーが 2 つのプロジェクトが含まれています](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**図 7**: ソリューション エクスプ ローラーが 2 つのプロジェクトが含まれています


`NORTHWNDConnectionString`値`Web.config`で現在参照されて、`NORTHWND.MDF`ファイル、`App_Data`フォルダー。 このデータベースから削除したため`App_Data`を明示的に登録し、これに応じて拡大縮小を更新する SQL Server 2005 Express Edition のデータベース インスタンスで、`NORTHWNDConnectionString`値。 開く、`Web.config`ファイルで、web サイトと変更、`NORTHWNDConnectionString`値、接続文字列を読み取るようにする:`Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`します。 この変更後、`<connectionStrings>`セクション`Web.config`次のようになります。


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> 説明したように、[前のチュートリアル](debugging-stored-procedures-cs.md)クライアント アプリケーションから SQL Server オブジェクトをデバッグするときなど、ASP.NET web サイトでは、接続プールを無効にする必要があります。 上記の接続文字列は、接続プールを無効にします ( `Pooling=false` )。 ASP.NET web サイトから管理対象のストアド プロシージャ、Udf のデバッグを行わない場合は、接続プールを有効にします。


## <a name="step-3-creating-a-managed-stored-procedure"></a>手順 3: ストアド プロシージャを作成、管理対象

マネージ ストアド プロシージャを Northwind データベースに追加するには、最初の SQL Server プロジェクト内のメソッドとしてストアド プロシージャを作成する必要があります。 ソリューション エクスプ ローラーを右クリックし、`ManagedDatabaseConstructs`プロジェクト名と新しい項目の追加を選択します。 これにより、プロジェクトに追加できるマネージ データベース オブジェクトの種類の一覧、新しい項目の追加 ダイアログ ボックスが表示されます。 図 8 に示す、他のユーザーの間でストアド プロシージャとユーザー定義の関数を含みます。

S を単にすべての提供が中止されました製品を返すストアド プロシージャを追加することで開始できるようにします。 新しいストアド プロシージャのファイルに名前`GetDiscontinuedProducts.cs`します。


[![GetDiscontinuedProducts.cs という名前の新しいストアド プロシージャを追加します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**図 8**: 新しいストアド プロシージャという名前の追加`GetDiscontinuedProducts.cs`([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))。


これにより、次の内容で新しい c# クラス ファイルが作成されます。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

注としてストアド プロシージャが実装されている、`static`内のメソッド、`partial`という名前のクラス ファイル`StoredProcedures`します。 さらに、`GetDiscontinuedProducts`でメソッドを装飾、 `SqlProcedure attribute`、ストアド プロシージャとしてのメソッドがマークされます。

次のコードを作成、`SqlCommand`オブジェクトと設定その`CommandText`に、`SELECT`から列をすべて返すクエリを`Products`製品のテーブルです`Discontinued`1 equals をフィールドします。 コマンドを実行し、結果をクライアント アプリケーションに送信します。 このコードを追加、`GetDiscontinuedProducts`メソッド。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

すべてのマネージ データベース オブジェクトへのアクセスがある、 [ `SqlContext`オブジェクト](https://msdn.microsoft.com/library/ms131108.aspx)呼び出し元のコンテキストを表します。 `SqlContext`にアクセスできるように、 [ `SqlPipe`オブジェクト](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx)経由でその[`Pipe`プロパティ](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx)します。 これは、`SqlPipe`オブジェクトは、SQL Server データベースと呼び出し元のアプリケーション間の情報の終端に使用します。 その名のとおり、 [ `ExecuteAndSend`メソッド](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx)、渡されたでを実行します`SqlCommand`オブジェクトと、結果がクライアント アプリケーションを送信します。

> [!NOTE]
> マネージ データベース オブジェクトは、ストアド プロシージャ、Udf、セットベースのロジックではなく、手続き型のロジックを使用するに最適です。 手続き型のロジックでは、行単位ごとにデータのセットの操作またはスカラーのデータを扱う必要があります。 `GetDiscontinuedProducts`先ほど作成した、ただし、メソッドに手続き型のロジックは関係しません。 そのため、これは理想的にとして実装する T-SQL ストアド プロシージャ。 マネージ ストアド プロシージャを作成および展開するために必要な手順を示すためにマネージ ストアド プロシージャとして実装されます。


## <a name="step-4-deploying-the-managed-stored-procedure"></a>手順 4: デプロイ マネージ ストアド プロシージャ

このコードは完全で、Northwind データベースにデプロイする準備ができました。 SQL Server のプロジェクト配置は、コード アセンブリにコンパイル、データベースと、アセンブリを登録およびアセンブリに適切なメソッドにリンクすること、データベース内の対応するオブジェクトを作成します。 [配置] オプションで実行されるタスクの正確なセットは手順 13 に記述されたより正確にします。 右クリックし、`ManagedDatabaseConstructs`プロジェクト名がソリューション エクスプ ローラーで、[配置] オプションを選択します。 ただし、展開が次のエラーで失敗します。 '外部' 付近に構文が正しくないです。 この機能を有効にする大きな値に、現在のデータベースの互換性レベルを設定する必要があります。 参照してくださいヘルプ ストアド プロシージャを`sp_dbcmptlevel`します。

このエラー メッセージは、Northwind データベースにアセンブリを登録する際に発生します。 SQL Server 2005 データベースにアセンブリを登録するために、データベースの互換性レベルを 90 に設定する必要があります。 既定では、新しい SQL Server 2005 データベースは 90 の互換性レベルがあります。 ただし、Microsoft SQL Server 2000 を使用して作成されたデータベースでは、80 の既定の互換性レベルがあります。 Northwind データベースが Microsoft SQL Server 2000 データベースでは最初に、その互換性レベルが 80 に設定されていて、したがってを 90 にマネージ データベース オブジェクトを登録するために増やす必要があります。

データベースの互換性レベルを更新するには、Management Studio で新しいクエリ ウィンドウを開きし、入力します。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

上記のクエリを実行するには、ツールバーの実行アイコンをクリックします。


[![Northwind データベースの互換性レベルを更新します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**図 9**: Northwind データベースの互換性レベルを更新 ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))。


互換性レベルを更新した後は、SQL Server のプロジェクトを再デプロイします。 この時間、展開は、エラーなしで完了する必要があります。

SQL Server Management Studio に戻り、[オブジェクト エクスプ ローラーで Northwind データベースを右クリックしておよび更新] を選択します。 次に、プログラミング フォルダーにドリル ダウンし、アセンブリ フォルダーを展開します。 図 10 に示す、Northwind データベースが含まれていますによって生成されたアセンブリには、`ManagedDatabaseConstructs`プロジェクト。


![ManagedDatabaseConstructs アセンブリは、Northwind データベースに登録されました](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**図 10**:`ManagedDatabaseConstructs`アセンブリは、Northwind データベースに登録されました


また Stored Procedures フォルダーを展開します。 という名前のストアド プロシージャが表示されます`GetDiscontinuedProducts`します。 このストアド プロシージャが、展開プロセスとへのポインターによって作成された、`GetDiscontinuedProducts`メソッドで、`ManagedDatabaseConstructs`アセンブリ。 ときに、`GetDiscontinuedProducts`ストアド プロシージャを実行すると、さらに、実行、`GetDiscontinuedProducts`メソッド。 これは、マネージ ストアド プロシージャであるために、Management Studio を使用は編集できません (そのため、ストアド プロシージャ名の横にあるロック アイコン)。


![GetDiscontinuedProducts ストアド プロシージャは、ストアド プロシージャ フォルダーに一覧表示します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**図 11**:`GetDiscontinuedProducts`ストアド プロシージャが、ストアド プロシージャ フォルダーに記載されています


マネージ ストアド プロシージャを呼び出し前になりませんハードルがまだ: マネージ コードの実行を防ぐために、データベースを構成します。 この新しいクエリ ウィンドウを開きを実行することを確認、`GetDiscontinuedProducts`ストアド プロシージャ。 次のエラー メッセージが表示されます。 .NET Framework でのユーザー コードの実行が無効になっています。 Clr enabled 構成オプションを有効にします。

Northwind データベースの構成情報を確認し、入力して、コマンドを実行する`exec sp_configure`クエリ ウィンドウにします。 これは、設定に有効になっている clr が現在 0 に設定することを示しています。


[![Clr を有効になっている設定が設定されている 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**図 12**: clr を有効になっている設定が設定されている 0 ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))。


図 12 では、各構成設定が示した 4 つの値を持つことに注意してください: 最小値と最大値および構成と実行の値。 Clr を有効になっている設定の構成値を更新するには、次のコマンドを実行します。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

再実行する場合、`exec sp_configure`は上記のステートメントが 1 に clr を有効になっている設定の構成値を更新することが、実行値が 0 に設定を参照してください。 この構成の変更を有効にする必要がありますを実行する、 [ `RECONFIGURE`コマンド](https://msdn.microsoft.com/library/ms176069.aspx)は、実行値、現在の構成値に設定します。 入力するだけ`RECONFIGURE`クエリ ウィンドウで、ツールバーの実行アイコンをクリックします。 実行する場合`exec sp_configure`clr を有効になっている設定の構成では、1 の値が表示して値を実行する必要があります。

完全な clr を有効になっている構成では、マネージを実行する準備が`GetDiscontinuedProducts`ストアド プロシージャ。 クエリ ウィンドウで入力し、コマンド`exec``GetDiscontinuedProducts`します。 対応するマネージ コードをストアド プロシージャを呼び出すことにより、`GetDiscontinuedProducts`メソッドを実行します。 このコードの問題、`SELECT`は廃止されましたされて呼び出し元のアプリケーションは、このインスタンスで SQL Server Management Studio は、このデータを返すすべての製品を返すクエリです。 Management Studio では、これらの結果を受信し、結果ウィンドウに表示します。


[![ストアド プロシージャが返すすべて GetDiscontinuedProducts 製品を廃止します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**図 13**:`GetDiscontinuedProducts`ストアド プロシージャを返しますすべて廃止の製品 ([フルサイズの画像を表示する をクリックします。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))。


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>手順 5: マネージ ストアド プロシージャの作成を入力パラメーターを受け取る

クエリとこのチュートリアルで作成したストアド プロシージャが使用*パラメーター*します。 たとえば、[型指定されたデータセット s Tableadapter の新しいのストアド プロシージャの作成](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)という名前のストアド プロシージャを作成したチュートリアル`GetProductsByCategoryID`という入力パラメーターを承諾する`@CategoryID`。 ストアド プロシージャには、すべての製品が返されますが`CategoryID`フィールドには、指定された値が一致した`@CategoryID`パラメーター。

入力パラメーターを受け取るマネージ ストアド プロシージャを作成するには、メソッドの定義でそれらのパラメーターを指定だけです。 これを示すためには、let s に追加する別のマネージ ストアド プロシージャ、`ManagedDatabaseConstructs`という名前のプロジェクト`GetProductsWithPriceLessThan`します。 このマネージ ストアド プロシージャは、価格を指定する入力パラメーターを受け入れるしはすべての製品を返しますが`UnitPrice`フィールドは、パラメーターの値より小さい。

プロジェクトに新しいストアド プロシージャを追加するを右クリックし、`ManagedDatabaseConstructs`プロジェクト名と、新しいストアド プロシージャを追加することもできます。 そのファイルに `GetProductsWithPriceLessThan.cs` という名前を付けます。 という名前のメソッドで新しい c# クラス ファイルを作成これが手順 3. で説明したように`GetProductsWithPriceLessThan`内に配置される、`partial`クラス`StoredProcedures`します。

更新プログラム、`GetProductsWithPriceLessThan`メソッドの定義を受け入れるように、 [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx)という名前の入力パラメーター`price`と書き込みを実行し、クエリの結果を返すコード。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

`GetProductsWithPriceLessThan`メソッドの定義とコードの定義とのコードを密接に似る、`GetDiscontinuedProducts`手順 3 で作成したメソッド。 唯一の違いを`GetProductsWithPriceLessThan`メソッドは入力パラメーターとして受け取ります (`price`)、 `SqlCommand` s クエリにはパラメーターが含まれています (`@MaxPrice`) にパラメーターを追加し、 `SqlCommand` s`Parameters`コレクションが、値を割り当て、`price`変数。

このコードを追加した後は、SQL Server のプロジェクトを再デプロイします。 次に、SQL Server Management Studio に戻るし、Stored Procedures フォルダーを更新します。 新しいエントリを参照する必要があります`GetProductsWithPriceLessThan`します。 クエリ ウィンドウで、入力し、コマンド`exec GetProductsWithPriceLessThan 25`、図 14 に示すように、25 ドル未満のすべての製品を一覧には。


[![$25 で製品が表示されます。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**図 14**: 25 ドルで製品が表示されます ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))。


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>手順 6: データ アクセス層からのマネージ ストアド プロシージャの呼び出し

この時点で追加されました、`GetDiscontinuedProducts`と`GetProductsWithPriceLessThan`マネージ ストアド プロシージャを`ManagedDatabaseConstructs`プロジェクトし、Northwind SQL Server データベースとそれらを登録します。 SQL Server Management Studio からこれらの管理対象のストアド プロシージャを呼び出すことも (図 13 および 14 s を参照してください)。 これらを使用するアプリケーションがストアド プロシージャを管理するため、ASP.NET には、ただし、データ アクセスおよびアーキテクチャのビジネス ロジック層に追加する必要があります。 この手順で新しい 2 つの方法は追加、`ProductsTableAdapter`で、`NorthwindWithSprocs`型指定されたデータセットで最初に作成された、[型指定されたデータセット s Tableadapter の新しいのストアド プロシージャの作成](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)チュートリアル。 手順 7 では、BLL に、対応するメソッドを追加します。

開く、 `NorthwindWithSprocs` Visual Studio で開始する新しいメソッドを追加することで、型指定されたデータセット、`ProductsTableAdapter`という`GetDiscontinuedProducts`します。 TableAdapter には、新しいメソッドを追加するには、デザイナーで TableAdapter の名前を右クリックし、コンテキスト メニューからクエリの追加オプションを選択します。

> [!NOTE]
> Northwind データベースから移動しているため、 `App_Data` SQL Server 2005 Express Edition のデータベース インスタンスにフォルダー、この変更を反映するように Web.config 内の対応する接続文字列を更新することが不可欠です。 手順 2. で説明した更新、`NORTHWNDConnectionString`値`Web.config`します。 この更新プログラムを忘れた場合と、クエリを追加するには、エラー メッセージ失敗が表示されます。 接続が見つかりません`NORTHWNDConnectionString`オブジェクトの`Web.config`ダイアログ ボックス、TableAdapter に新しいメソッドを追加しようとしています。 このエラーを解決するには、[ok] をクリックしに移動し、`Web.config`し、更新、`NORTHWNDConnectionString`手順 2. で説明したように値します。 TableAdapter にメソッドを再度追加してみます。 この時間がエラーなく動作する必要があります。


新しいメソッドを追加すると、何度も過去のチュートリアルで使用すると、TableAdapter クエリ構成ウィザードが起動します。 最初の手順では、TableAdapter によるデータベースのアクセス方法を指定するよう求められます。 または、新規または既存のストアド プロシージャを使用して、アドホック SQL ステートメントを使用します。 既に作成され、登録しましたので、`GetDiscontinuedProducts`管理対象のストアド プロシージャ、データベースを使用して既存のストアド プロシージャ オプションと [次へ] を選択します。


[![既存のストアド プロシージャ オプションの使用を選択します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**図 15**: ストアド プロシージャ オプションを使用して既存の選択 ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))。


次の画面から私たちメソッドを呼び出すストアド プロシージャのメッセージが表示されます。 選択、`GetDiscontinuedProducts`マネージ ストアド プロシージャをドロップダウン リストから、[次へ] をクリックします。


[![選択、GetDiscontinuedProducts マネージ ストアド プロシージャ](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**図 16**: 選択、`GetDiscontinuedProducts`マネージ ストアド プロシージャ ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))。


ストアド プロシージャには、行、1 つの値、または何が返されるかどうかを指定するメッセージが表示されます。 `GetDiscontinuedProducts`セットを返します提供が中止された製品の行の最初のオプション (表形式のデータ) を選択し、[次へ] をクリックします。


[![表形式のデータ オプションを選択します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**図 17**: 表形式のデータ オプションを選択します ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))。


ウィザードの最終画面では、データ アクセス パターンを使用し、結果として得られるメソッドの名前を指定できます。 チェック ボックスがオンと名の両方のメソッドのままに`FillByDiscontinued`と`GetDiscontinuedProducts`します。 ウィザードを完了するには、[完了] をクリックします。


[![名前のメソッド FillByDiscontinued と GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**図 18**: メソッドの名前を付けます`FillByDiscontinued`と`GetDiscontinuedProducts`([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))。


という名前のメソッドを作成するには、この手順を繰り返します`FillByPriceLessThan`と`GetProductsWithPriceLessThan`で、`ProductsTableAdapter`の`GetProductsWithPriceLessThan`ストアド プロシージャを管理します。

図 19 にメソッドを追加した後、データセット デザイナーのスクリーン ショットを示しています、`ProductsTableAdapter`の`GetDiscontinuedProducts`と`GetProductsWithPriceLessThan`ストアド プロシージャを管理します。


[![ProductsTableAdapter には、この手順で追加された新しいメソッドが含まれています。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**図 19**:`ProductsTableAdapter`この手順で追加された新しいメソッドが含まれています ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))。


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>手順 7: ビジネス ロジック層への対応するメソッドの追加

できたので、追加の手順 4. と 5. でマネージ ストアド プロシージャを呼び出すためのメソッドのデータ アクセス層を更新しました、ビジネス ロジック層に対応するメソッドを追加する必要があります。 次の 2 つのメソッドを追加、`ProductsBLLWithSprocs`クラス。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

どちらの方法は、単に対応する DAL メソッドを呼び出しを返す、`ProductsDataTable`インスタンス。 `DataObjectMethodAttribute`各メソッドの上のマークアップにより、ObjectDataSource のデータ ソースの構成ウィザードの選択 タブで、ドロップダウン リストに含まれるこれらのメソッド。

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>手順 8: 呼び出すマネージ ストアド プロシージャのプレゼンテーション層

ビジネス ロジックとデータ アクセス層に呼び出しをサポートするように拡張、`GetDiscontinuedProducts`と`GetProductsWithPriceLessThan`マネージ ストアド プロシージャは、これらも表示できる ASP.NET ページを使用してプロシージャ結果を格納します。

開く、`ManagedFunctionsAndSprocs.aspx`ページで、`AdvancedDAL`フォルダーと、ツールボックスから、GridView をデザイナーにドラッグします。 GridView s 設定`ID`プロパティを`DiscontinuedProducts`し、スマート タグ、という名前の新しい ObjectDataSource にバインドする`DiscontinuedProductsDataSource`します。 構成からそのデータをプルする ObjectDataSource、`ProductsBLLWithSprocs`クラスの`GetDiscontinuedProducts`メソッド。


[![ProductsBLLWithSprocs クラスを使用する ObjectDataSource を構成します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**図 20**: 構成に使用する ObjectDataSource、`ProductsBLLWithSprocs`クラス ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))。


[![GetDiscontinuedProducts 方法を選択します タブで、ドロップダウン リストから選択します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**図 21**: 選択、`GetDiscontinuedProducts`選択 タブで、ドロップダウン リストからメソッド ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))。


このグリッドを表示製品情報だけを使用するためでは、UPDATE、INSERT、ドロップダウン リストを設定し (None) にタブを削除して [完了] をクリックします。

ウィザードを完了すると、Visual Studio が自動的に追加 BoundField または CheckBoxField 内の各データ フィールドの`ProductsDataTable`します。 これらのフィールドを除くのすべてを削除する少し`ProductName`と`Discontinued`位置、GridView をポイントして ObjectDataSource s の宣言型マークアップは、次のようになります。


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

ブラウザーからこのページを表示する時間がかかります。 ページを開いたとき、ObjectDataSource 呼び出し、`ProductsBLLWithSprocs`クラスの`GetDiscontinuedProducts`メソッド。 このメソッドは、DAL に呼び出します手順 7. で説明したように`ProductsDataTable`クラス s`GetDiscontinuedProducts`によって実行されるメソッド、`GetDiscontinuedProducts`ストアド プロシージャ。 このストアド プロシージャは、マネージ ストアド プロシージャと提供が中止された製品を返す、手順 3 で作成したコードを実行します。

マネージ ストアド プロシージャによって返される結果にパッケージ化、 `ProductsDataTable` DAL によってして返送、BLL は、プレゼンテーション層、GridView にバインドされているし、表示の場所に戻ります。 予想どおり、グリッドには廃止されましたが、これらの製品が一覧表示します。


[![廃止された製品が一覧表示します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**図 22**:、廃止された製品の一覧が表示されます ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))。


さらに実際には、ページに、テキスト ボックスと別の GridView を追加します。 この GridView を呼び出すことによって、テキスト ボックスに入力された金額よりも小さい、製品の表示がある、`ProductsBLLWithSprocs`クラスの`GetProductsWithPriceLessThan`メソッド。

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>手順 9: を作成して、T-SQL での Udf を呼び出す

ユーザー定義関数、または Udf の場合、データベース オブジェクト、厳密に模倣されたプログラミング言語の関数のセマンティクスです。 ような c# の関数は、Udf は、入力パラメーター数が変数を含めるし、特定の型の値を返します。 UDF は、文字列、整数、およびなど、または表形式のデータでのいずれかのスカラーのデータを返すことができます。 S をざっと見て Udf、スカラー データ型を返す UDF を以降の両方の種類を使用できます。

次の UDF は、特定の製品の在庫の推定値を計算します。 これは、3 つの入力パラメーターで考慮することで、 `UnitPrice`、 `UnitsInStock`、および`Discontinued`- 特定の製品の値し、型の値を返します`money`します。 乗算することによって、在庫の推定値を計算、`UnitPrice`によって、`UnitsInStock`します。 この値は、提供が中止された項目の半分になります。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

この UDF は、データベースに追加されると、プログラミング フォルダー、し、関数、および、スカラー値関数を展開して Management Studio を検出することができます。 使用できます、`SELECT`クエリようになります。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

追加した、 `udf_ComputeInventoryValue` Northwind データベースに UDF図 23 は、上記の出力を示しています。 `SELECT` Management Studio で表示した場合のクエリを実行します。 また、UDF がオブジェクト エクスプ ローラーでスカラー値関数のフォルダーの下に表示されていることに注意してください。


[![各製品の在庫値を一覧します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**図 23**: インベントリの値が表示されている各製品 s ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))。


Udf は、表形式のデータを返すこともできます。 たとえば、特定のカテゴリに属する製品を返す UDF を作成できます。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

`udf_GetProductsByCategoryID` UDF を受け入れる、`@CategoryID`入力パラメーターと、指定の結果を返します`SELECT`クエリ。 この UDF を参照できます、作成した後、 `FROM` (または`JOIN`) の句、`SELECT`クエリ。 次の例が返されます、 `ProductID`、 `ProductName`、および`CategoryID`ごと、飲み物の値。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

追加した、 `udf_GetProductsByCategoryID` Northwind データベースに UDF図 24 は、上記の出力を示しています。 `SELECT` Management Studio で表示した場合のクエリを実行します。 表形式のデータを返す Udf は、オブジェクト エクスプ ローラーのテーブル値関数フォルダーにあります。


[![ProductID、ProductName、および CategoryID 各飲み物の一覧が表示されます。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**図 24**: `ProductID`、 `ProductName`、および`CategoryID`各飲み物の一覧表示されます ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))。


> [!NOTE]
> 作成と Udf の使用に関する詳細については、チェック アウト[ユーザー定義関数の概要](http://www.sqlteam.com/item.asp?ItemID=1955)します。 チェック アウトも[利点と Drawbacks of User-Defined 関数](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)します。


## <a name="step-10-creating-a-managed-udf"></a>手順 10: マネージ UDF を作成します。

`udf_ComputeInventoryValue`と`udf_GetProductsByCategoryID`上記の例で作成される Udf は SQL データベースのオブジェクト。 SQL Server 2005 には、マネージ Udf に追加することができますもサポートしています、`ManagedDatabaseConstructs`マネージ ストアド プロシージャの手順 3 から 5 と同様のプロジェクトします。 この手順では、s が実装できるように、 `udf_ComputeInventoryValue` UDF では、マネージ コード。

マネージ UDF を追加する、`ManagedDatabaseConstructs`プロジェクト、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、新しい項目の追加を選択します。 新しい項目の追加 ダイアログ ボックスからユーザー定義テンプレートを選択し、新しい UDF ファイルに名前`udf_ComputeInventoryValue_Managed.cs`します。


[![新しいマネージ UDF を ManagedDatabaseConstructs プロジェクトに追加します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**図 25**: に新しいマネージ UDF を追加、`ManagedDatabaseConstructs`プロジェクト ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))。


ユーザー定義関数のテンプレートを作成、`partial`という名前のクラス`UserDefinedFunctions`クラス ファイルの名前と同じ名前を持つメソッドを (`udf_ComputeInventoryValue_Managed`、このインスタンスで)。 使用してこのメソッドを装飾、 [ `SqlFunction`属性](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx)、マネージ UDF としてメソッドのフラグを設定します。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

`udf_ComputeInventoryValue`メソッドが返す現在、 [ `SqlString`オブジェクト](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx)入力パラメーターは受け付けられません。 3 つの入力パラメーターを受け入れるように、メソッド定義を更新する必要があります`UnitPrice`、 `UnitsInStock`、および`Discontinued`- を返します、`SqlMoney`オブジェクト。 インベントリの値を計算するためのロジックは、T-SQL と同じ`udf_ComputeInventoryValue`UDF します。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

UDF メソッドの入力パラメーターは、対応する SQL 型に注意してください:`SqlMoney`の`UnitPrice`フィールド、 [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx)の`UnitsInStock`、および[ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx)`Discontinued`します。 これらのデータ型で定義された型の反映、`Products`テーブル:`UnitPrice`型の列は、 `money`、`UnitsInStock`型の列`smallint`、および`Discontinued`型の列`bit`します。

コードを作成して開始、`SqlMoney`という名前のインスタンス`inventoryValue`0 の値を割り当てることができます。 `Products`テーブルでは、データベースの`NULL`値、`UnitsInPrice`と`UnitsInStock`列。 そのため、必要があります最初のチェックにこれらの値が含まれているかどうかを`NULL`で、これを行って、`SqlMoney`オブジェクト[`IsNull`プロパティ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx)します。 両方`UnitPrice`と`UnitsInStock`以外を含む`NULL`値を計算し、 `inventoryValue` 2 つの製品であることにします。 場合はその後、`Discontinued`が true の場合、値を半分います。

> [!NOTE]
> `SqlMoney`オブジェクトは、2 つのみを`SqlMoney`を乗算するインスタンス。 できません、`SqlMoney`インスタンスにリテラルの浮動小数点数が乗算されます。 そのため、半分にする`inventoryValue`新しいによって乗算します`SqlMoney`0.5 の値を持つインスタンス。


## <a name="step-11-deploying-the-managed-udf"></a>手順 11: マネージ UDF を展開します。

マネージ UDF を作成するは、Northwind データベースにデプロイする準備ができました。 手順 4. で説明したように、SQL Server プロジェクト内のマネージ オブジェクトは、ソリューション エクスプ ローラーでプロジェクト名を右クリックし、コンテキスト メニューから [配置] オプションを選択してデプロイされます。

プロジェクトをデプロイした後は、SQL Server Management Studio に戻り、スカラー値関数のフォルダーを更新しています。 2 つのエントリが表示されます。

- `dbo.udf_ComputeInventoryValue` -手順 9 で T-SQL UDF を作成し、
- `dbo.udf ComputeInventoryValue_Managed` -展開されたばかりの手順 10 で、マネージ UDF を作成します。

このマネージ UDF をテストするには、Management Studio 内から次のクエリを実行します。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

このコマンドは、マネージ`udf ComputeInventoryValue_Managed`、T-SQL ではなく UDF `udf_ComputeInventoryValue` UDF が、出力は同じです。 UDF の出力のスクリーン ショットを表示する図 23 を参照します。

## <a name="step-12-debugging-the-managed-database-objects"></a>手順 12: マネージ データベース オブジェクトのデバッグ

[ストアド プロシージャのデバッグ](debugging-stored-procedures-cs.md)デバッグ Visual Studio での SQL Server のオプションは、3 つを説明したチュートリアル: ダイレクト データベース デバッグ、アプリケーションのデバッグ、および SQL Server のプロジェクトからデバッグします。 クライアント アプリケーションと SQL Server のプロジェクトから直接オブジェクトは、ダイレクト データベース デバッグを使用してデバッグすることはできませんが、デバッグできるは、データベースを管理します。 デバッグを行うためには、ただし、SQL Server 2005 データベース必要があります SQL/CLR のデバッグを許可します。 最初に作成したときにを思い出してください、`ManagedDatabaseConstructs`プロジェクトが Visual Studio をご希望 SQL/CLR デバッグ (手順 2. で図 6 参照) を有効にしたいかどうか。 この設定は、サーバー エクスプ ローラー ウィンドウからデータベースを右クリックして変更できます。


![データベースで SQL/CLR デバッグを許可します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**図 26**: データベースで SQL/CLR デバッグを許可


想像してデバッグする、`GetProductsWithPriceLessThan`ストアド プロシージャを管理します。 内のコードでブレークポイントを設定して始めたい、`GetProductsWithPriceLessThan`メソッド。


[![GetProductsWithPriceLessThan メソッドにブレークポイントを設定します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**図 27**: にブレークポイントを設定、`GetProductsWithPriceLessThan`メソッド ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))。


最初に、SQL Server のプロジェクトからのマネージ データベース オブジェクトのデバッグについて s を使用できます。 -2 つのプロジェクトがソリューションに含まれているため、`ManagedDatabaseConstructs`と共に Visual Studio を起動するように指示する必要があります。 SQL Server プロジェクトからデバッグするには、web サイトの SQL Server のプロジェクト、 `ManagedDatabaseConstructs` SQL Server プロジェクトのデバッグを開始するとします。 右クリックし、`ManagedDatabaseConstructs`ソリューション エクスプ ローラーでプロジェクトし、コンテキスト メニューからスタートアップ プロジェクト オプションとして、セットを選択します。

ときに、`ManagedDatabaseConstructs`で SQL ステートメントを実行しますが、デバッガーからプロジェクトを起動、`Test.sql`ファイルにある、`Test Scripts`フォルダー。 たとえば、テストするため、`GetProductsWithPriceLessThan`マネージ ストアド プロシージャは、既存`Test.sql`ファイルのコンテンツを呼び出す次のステートメントを使用して、`GetProductsWithPriceLessThan`を渡してストアド プロシージャを管理、 `@CategoryID` 14.95 の値。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

上記のスクリプトを入力するいると`Test.sql`デバッグ メニューに移動し、デバッグ開始 を選択するか、f5 キーを押してデバッグを開始、または、ツールバーの緑色の再生 アイコンです。 ソリューション内のプロジェクトをビルド、マネージ データベース オブジェクトを Northwind データベースに配置し、実行し、これは、`Test.sql`スクリプト。 この時点で、ブレークポイントにヒットして、たどることができます、`GetProductsWithPriceLessThan`メソッドを入力パラメーターの値を確認したりします。


[![GetProductsWithPriceLessThan メソッド内のブレークポイントがヒットしました](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**図 28**: で、ブレークポイント、`GetProductsWithPriceLessThan`メソッドがヒットしました ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))。


SQL データベース オブジェクトのクライアント アプリケーションからデバッグするためには、アプリケーションのデバッグをサポートするために、データベースを構成することが欠かせません。 サーバー エクスプ ローラーでデータベースを右クリックして、アプリケーションのデバッグ オプションをオンになっていることを確認してください。 さらに、SQL デバッガーと統合して、接続プールを無効にする ASP.NET アプリケーションを構成する必要があります。 次の手順のステップ 2 で詳しく説明された、[ストアド プロシージャのデバッグ](debugging-stored-procedures-cs.md)チュートリアル。

ASP.NET アプリケーションとデータベースを構成した後は、スタートアップ プロジェクトとして、ASP.NET web サイトを設定し、デバッグを開始します。 ブレークポイントがある管理対象のオブジェクトの 1 つを呼び出すページを閲覧する場合、アプリケーションは停止し、コントロールが有効になります経由で、デバッガーに、できるコードをステップ実行、図 28 に示すようにします。

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>手順 13: 手動でのコンパイルと配置マネージ データベース オブジェクト

SQL Server プロジェクトを簡単に作成、コンパイル、およびマネージ データベース オブジェクトを展開します。 残念ながら、SQL Server のプロジェクトでは、Visual Studio の Professional およびチーム システムのエディションで使用できるのみです。 Visual Web Developer、または Standard Edition の Visual Studio を使用してマネージ データベース オブジェクトを使用する場合は、手動で作成して、それらをデプロイする必要があります。 これには、4 つの手順が含まれます。

1. マネージ データベース オブジェクトのソース コードを含むファイルを作成します。
2. オブジェクトをアセンブリにコンパイルします。
3. SQL Server 2005 データベースにアセンブリを登録し、
4. アセンブリに適切なメソッドを指す SQL Server のデータベース オブジェクトを作成します。

S を新規作成するこれらのタスクを示しています。 これらの製品を返すストアド プロシージャを管理対象の`UnitPrice`が指定された値より大きい。 という名前のコンピューターに新しいファイルを作成`GetProductsWithPriceGreaterThan.cs`ファイルに次のコードを入力し、(することができますを使用して、Visual Studio、メモ帳または任意のテキスト エディターこれを実現する)。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

このコードはほぼと同じで、`GetProductsWithPriceLessThan`手順 5. で作成したメソッド。 唯一の違いは、メソッド名、`WHERE`句、およびクエリで使用されるパラメーターの名前。 戻り、`GetProductsWithPriceLessThan`メソッド、`WHERE`句を読み取る:`WHERE UnitPrice < @MaxPrice`します。 ここで、`GetProductsWithPriceGreaterThan`を使用して:`WHERE UnitPrice > @MinPrice`します。

これで、このクラスをアセンブリにコンパイルする必要があります。 コマンドラインから保存したディレクトリに移動、`GetProductsWithPriceGreaterThan.cs`ファイルを開き、c# コンパイラを使用して (`csc.exe`) アセンブリにクラス ファイルをコンパイルします。


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

場合、フォルダーを含む`csc.exe`で s システムではなく`PATH`、完全パスを参照する必要があります`%WINDOWS%\Microsoft.NET\Framework\version\`、次のように。


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![GetProductsWithPriceGreaterThan.cs をアセンブリにコンパイルします。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**図 29**: コンパイル`GetProductsWithPriceGreaterThan.cs`an アセンブリに ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))。


`/t`フラグは、c# クラス ファイルを (実行可能ファイルではなく) DLL にコンパイルする必要がありますを指定します。 `/out`フラグは、生成されたアセンブリの名前を指定します。

> [!NOTE]
> コンパイルではなく、`GetProductsWithPriceGreaterThan.cs`また使用することがコマンドラインからのクラス ファイル[Visual c# Express Edition](https://msdn.microsoft.com/vstudio/express/visualcsharp/)または Visual Studio Standard Edition で別のクラス ライブラリ プロジェクトを作成します。 S ren Jacob Lauritsen がこのような Visual c# Express Edition プロジェクトのコードを用意して、`GetProductsWithPriceGreaterThan`ストアド プロシージャと 2 つの管理ストアド プロシージャおよび UDF は、3、5、および 10 の手順で作成します。 Ren s プロジェクトには、対応するデータベース オブジェクトを追加するために必要な T-SQL コマンドも含まれています。


アセンブリにコンパイルされたコードで、SQL Server 2005 のデータベース内でアセンブリを登録する準備が整いました。 これは、コマンドを使用して、T-SQL で実行できる`CREATE ASSEMBLY`、または SQL Server Management Studio を使用します。 注目を使用すると、Management Studio を使用してことができます。

Management studio で、Northwind データベースの [プログラミング] フォルダーを展開します。 そのサブフォルダーの 1 つは、アセンブリです。 データベースを新しいアセンブリを手動で追加するには、アセンブリ フォルダーを右クリックし、コンテキスト メニューから新しいアセンブリを選択します。 この表示では、新しいアセンブリ ダイアログ ボックス (図 30) します。 [参照] ボタンをクリックして、`ManuallyCreatedDBObjects.dll`アセンブリだけコンパイルされると、し、データベースにアセンブリを追加するには、[ok] をクリックします。 表示しないように、`ManuallyCreatedDBObjects.dll`オブジェクト エクスプ ローラーでアセンブリ。


[![ManuallyCreatedDBObjects.dll アセンブリをデータベースに追加します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**図 30**: 追加、`ManuallyCreatedDBObjects.dll`データベースにアセンブリ ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))。


![オブジェクト エクスプ ローラーで、ManuallyCreatedDBObjects.dll が一覧表示します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**図 31**:`ManuallyCreatedDBObjects.dll`オブジェクト エクスプ ローラーで表示されます


Northwind データベースへのアセンブリが追加されていますまだに関連付ける必要があるストアド プロシージャ、`GetProductsWithPriceGreaterThan`アセンブリのメソッド。 これを実現するには、新しいクエリ ウィンドウを開き、次のスクリプトを実行します。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

という名前の Northwind データベースに新しいストアド プロシージャが作成されます`GetProductsWithPriceGreaterThan`マネージ メソッドに関連付けます`GetProductsWithPriceGreaterThan`(クラスである`StoredProcedures`、アセンブリである`ManuallyCreatedDBObjects`)。

上記のスクリプトを実行した後、オブジェクト エクスプ ローラーでストアド プロシージャ フォルダーを更新します。 -ストアド プロシージャの新しいエントリが表示`GetProductsWithPriceGreaterThan`の横にあるロック アイコンがあります。 このストアド プロシージャをテストするには、入力し、クエリ ウィンドウで、次のスクリプトを実行します。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

上記のコマンドが製品の情報を表示する 32 の図に示すよう、 `UnitPrice` $24.95 より大きい。


[![オブジェクト エクスプ ローラーで、ManuallyCreatedDBObjects.dll が一覧表示します。](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**図 32**:`ManuallyCreatedDBObjects.dll`オブジェクト エクスプ ローラーで表示されます ([フルサイズの画像を表示する をクリックします](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))。


## <a name="summary"></a>まとめ

Microsoft SQL Server 2005 では、統合と、共通言語ランタイム (CLR)、データベース オブジェクトをマネージ コードを使用して作成できるを提供します。 以前は、これらのデータベース オブジェクトは、T-SQL を使用して作成するだけでしたが、次に .NET のプログラミング (C#) のような言語を使用してこれらのオブジェクトを作成します。 作成したこのチュートリアルでは 2 つのマネージ ストアド プロシージャとマネージ ユーザー定義関数。

Visual Studio の SQL Server のプロジェクトの種類には、作成、コンパイル、およびマネージ データベース オブジェクトの配置が容易になります。 さらに、豊富なデバッグ サポートを提供します。 ただし、SQL Server のプロジェクトの種類では、Visual Studio の Professional およびチーム システムのエディションで使用できるのみです。 にとって Visual Web Developer、または Standard Edition の Visual Studio、作成、コンパイル、および展開の手順を使用する必要があります手動で実行する、手順 13 で説明したようにします。

満足のプログラミングです。

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ユーザー定義関数の長所と短所](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [マネージ コードで SQL Server 2005 のオブジェクトを作成します。](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [SQL Server 2005 でマネージ コードを使用してトリガーを作成します。](http://www.15seconds.com/issue/041006.htm)
- [方法: を作成および CLR の SQL を実行するサーバーのストアド プロシージャ](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [作成、および CLR の SQL Server ユーザー定義関数を実行する方法](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [方法: 編集、 `Test.sql` SQL オブジェクトを実行するスクリプト](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [ユーザーの概要には、関数が定義されています。](http://www.sqlteam.com/item.asp?ItemID=1955)
- [マネージ コードと SQL Server 2005 (ビデオ)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [TRANSACT-SQL リファレンス](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [チュートリアル: マネージ コードでストアド プロシージャを作成します。](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、S ren Jacob Lauritsen でした。 この記事では、S ren を確認するほかには、マネージ データベース オブジェクトを手動でコンパイルするためには、この記事のダウンロードに含まれている Visual c# Express Edition プロジェクトを作成します。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](debugging-stored-procedures-cs.md)
> [次へ](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
