---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: ストアド プロシージャ (VB) のデバッグ |Microsoft Docs
author: rick-anderson
description: Visual Studio Professional および Team System のエディションを使用すると、ブレークポイントを設定し、SQL Server 内のストアド プロシージャにステップ、格納されているデバッグを行う.
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: a4a507205ff8e837f64e01acf84f6376006a87b9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806652"
---
<a name="debugging-stored-procedures-vb"></a>ストアド プロシージャのデバッグ (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip)または[PDF のダウンロード](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> Visual Studio Professional および Team System のエディションを使用すると、ブレークポイントを設定し、SQL Server 内のストアド プロシージャにステップ、アプリケーション コードのデバッグと同じくらい簡単ストアド プロシージャをデバッグできます。 このチュートリアルでは、ダイレクト データベース デバッグと、アプリケーションは、ストアド プロシージャのデバッグについて説明します。


## <a name="introduction"></a>はじめに

Visual Studio では、豊富なデバッグ エクスペリエンスを提供します。 いくつかのキーストロークやマウスの数回のクリックで、プログラムの実行を停止し、その状態と制御フローを確認するブレークポイントを使用することも可能です。 と共に、アプリケーション コードをデバッグするには、Visual Studio は、SQL Server 内からストアド プロシージャのデバッグのサポートを提供します。 ASP.NET 分離コード クラスまたはビジネス ロジック層のクラスのコード内でブレークポイントを設定することができますと同様、すぎることができますに配置するストアド プロシージャ内で。

このチュートリアルにステップ インするストアド プロシージャ内での Visual Studio サーバー エクスプ ローラーからもときにヒットしたブレークポイントを設定するのには、ストアド プロシージャは実行中の ASP.NET アプリケーションから呼び出す方法に注目します。

> [!NOTE]
> 残念ながら、ストアド プロシージャのみにステップ インでき Professional およびチーム システムのバージョンの Visual Studio でデバッグします。 Visual Web Developer、または standard のバージョンの Visual Studio を使用している場合は、ストアド プロシージャをデバッグするために必要な手順について説明しますが、コンピューターに次の手順をレプリケートすることはできません、読むへようこそ。


## <a name="sql-server-debugging-concepts"></a>SQL Server デバッグの概念

Microsoft SQL Server 2005 の統合を提供するもの、[共通言語ランタイム (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx)、これは、すべての .NET アセンブリで使用されるランタイム。 その結果、SQL Server 2005 では、マネージ データベース オブジェクトをサポートしています。 つまり、Visual Basic のクラスのメソッドとしてストアド プロシージャおよびユーザー定義関数 (Udf) のようなデータベース オブジェクトを作成できます。 これにより、これらのストアド プロシージャ、Udf では、.NET Framework と、独自のカスタム クラスから機能を利用できます。 もちろん、SQL Server 2005 では、T-SQL でのデータベース オブジェクトに対してのサポートもが提供されます。

SQL Server 2005 では、T-SQL とマネージ データベース オブジェクトの両方に対するデバッグのサポートを提供します。 ただし、これらのオブジェクトは、Visual Studio 2005 Professional やチーム システムのエディションでのみデバッグできます。 このチュートリアルでは、デバッグ T-SQL でのデータベース オブジェクトを説明します。 次のチュートリアルがマネージ データベース オブジェクトをデバッグします。

[概要の T-SQL と CLR の SQL Server 2005 でデバッグ](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx)からのブログ エントリ、 [SQL Server 2005 の CLR 統合チーム](https://blogs.msdn.com/sqlclr/default.aspx)Visual Studio から SQL Server 2005 のオブジェクトをデバッグする 3 つの方法が強調表示されます。

- **ダイレクト データベース デバッグ (DDD)** - サーバー エクスプ ローラーでストアド プロシージャ、Udf など、任意の SQL データベース オブジェクトにステップ インできます。 手順 1. で DDD を見ていきます。
- **アプリケーションのデバッグ**-データベース オブジェクト内のブレークポイントを設定し、ASP.NET アプリケーションを実行します。 データベース オブジェクトが実行されたときに、ブレークポイントにヒットして、コントロールがデバッガーに引き継が。 アプリケーションのデバッグができませんステップをデータベース オブジェクトにアプリケーション コードからに注意してください。 する必要があります明示的にブレークポイントを設定でこれらのストアド プロシージャまたは Udf デバッガーを停止します。 アプリケーションのデバッグが検査手順 2. で開始されます。
- **SQL Server のプロジェクトからデバッグ**-Visual Studio Professional やチーム システムのエディションは、一般的にマネージ データベース オブジェクトを作成するために使用する SQL Server プロジェクトの種類。 SQL Server プロジェクトを使用してを究明してその内容を次のチュートリアルでデバッグします。

Visual Studio では、ローカルおよびリモートの SQL Server インスタンス上のストアド プロシージャをデバッグできます。 ローカルの SQL Server インスタンスでは、Visual Studio と同じコンピューターにインストールされている 1 つです。 使用している SQL Server データベースが開発用コンピューターにない場合は、リモート インスタンスを見なされます。 これらのチュートリアルについては使用しているローカルの SQL Server インスタンス。 リモート SQL server インスタンスでストアド プロシージャのデバッグには、ローカル インスタンス上のストアド プロシージャのデバッグより多くの構成手順が必要です。

ローカルの SQL Server インスタンスを使用している場合は、手順 1 から始まります。 と末尾には、このチュートリアルに取り組みます。 リモートの SQL Server インスタンスを使用している場合ただしは、デバッグ時に確実にする最初の必要性をリモート インスタンス上の SQL Server ログインを持つ Windows ユーザー アカウントを使用して、開発用コンピューターにログに記録されます。 Moveover、このデータベースのログインと、実行中の ASP.NET アプリケーションからデータベースへの接続に使用するデータベース ログインの両方のメンバーである必要があります、`sysadmin`ロール。 リモート インスタンスをデバッグするには、Visual Studio と SQL Server の構成の詳細については、このチュートリアルの最後に、リモート インスタンスのセクションで T-SQL デバッグ データベース オブジェクトを参照してください。

最後に、T-SQL でのデータベース オブジェクトのデバッグをサポートがないこととして、.NET アプリケーションのデバッグをサポートと豊富な機能を理解します。 たとえば、ブレークポイント条件およびフィルター処理はサポートされていません、のみ、デバッグ ウィンドウのサブセットが使用可能、エディット コンティニュを使用することはできません、役に立たないなど、イミディ エイト ウィンドウが表示されます。 参照してください[デバッガー コマンドや機能に関する制限事項](https://msdn.microsoft.com/library/ms165035(VS.80).aspx)詳細についてはします。

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>手順 1: は、ストアド プロシージャに直接ステップ イン

Visual Studio では、データベース オブジェクトを直接デバッグが容易にします。 S、ダイレクト データベース デバッグ (DDD) の機能を使用して、ステップ インする方法を確認できるように、 `Products_SelectByCategoryID` Northwind データベースでストアド プロシージャ。 名前からわかるように、 `Products_SelectByCategoryID` ; 特定のカテゴリの製品情報を返しますで作成された、[型指定されたデータセット s Tableadapter の既存のストアド プロシージャの使用](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)チュートリアル。 サーバー エクスプ ローラーに移動して起動し、Northwind データベース ノードを展開します。 次に、ストアド プロシージャ フォルダーにドリル ダウンを右クリックし、`Products_SelectByCategoryID`ストアド プロシージャ、およびコンテキスト メニューからストアド プロシージャにステップ イン オプションを選択します。 これにより、デバッガーが開始されます。

以降、`Products_SelectByCategoryID`ストアド プロシージャが必要ですが、`@CategoryID`入力パラメーターは、この値を提供するように求められます。 飲み物に関する情報を返す、1 を入力します。


![値 1 を使用して、@CategoryIDパラメーター](debugging-stored-procedures-vb/_static/image1.png)

**図 1**: の値 1 を使用して、`@CategoryID`パラメーター


値を指定した後、`@CategoryID`パラメーター、ストアド プロシージャを実行します。 を完了するまで実行するのではなく、デバッガーが停止の最初のステートメントを実行します。 ストアド プロシージャの現在の位置を示す、余白の黄色の矢印に注意してください。 表示および [ウォッチ] ウィンドウで、またはストアド プロシージャでパラメーター名の上に置くことによって、パラメーター値を編集できます。


[![ストアド プロシージャの最初のステートメントでデバッガーが停止しました](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**図 2**: ストアド プロシージャの最初のステートメントでデバッガーが停止しました ([フルサイズの画像を表示する をクリックします](debugging-stored-procedures-vb/_static/image4.png))。


一度に 1 つのストアド プロシージャのステートメントをステップには、ツールバーの [ステップ オーバー] ボタンをクリックします。 または F10 キーを押してください。 `Products_SelectByCategoryID`ストアド プロシージャには、1 つが含まれています。`SELECT`ステートメントでは、1 つのステートメントをステップ オーバーと、ストアド プロシージャの実行が完了、f10 キーを押すようにします。 ストアド プロシージャが完了すると、その出力は、出力ウィンドウに表示され、デバッガーが終了します。

> [!NOTE]
> ステートメント レベルで発生する T-SQL デバッグステップ インすることはできません、`SELECT`ステートメント。


## <a name="step-2-configuring-the-website-for-application-debugging"></a>手順 2: アプリケーションのデバッグの web サイトを構成します。

サーバー エクスプ ローラーから直接ストアド プロシージャのデバッグは便利ですが、多くのシナリオで関心は、ASP.NET アプリケーションから呼び出された場合は、ストアド プロシージャをデバッグします。 Visual Studio 内からストアド プロシージャにブレークポイントを追加して、ASP.NET アプリケーションのデバッグを開始します。 ブレークポイントとストアド プロシージャが、アプリケーションから呼び出されると、実行がブレークポイントで停止し、表示および変更してストアド プロシージャのパラメーターの値を手順 1 で行ったのと同じように、そのステートメントをステップします。

アプリケーションから呼び出されるストアド プロシージャのデバッグを始めることができます、前に、SQL Server デバッガーと統合する ASP.NET web アプリケーションに指示する必要があります。 ソリューション エクスプ ローラーで web サイト名を右クリックして開始 (`ASPNET_Data_Tutorial_74_VB`)。 コンテキスト メニューからプロパティ ページのオプションを選択し、左側で、開始オプション項目を選択し、デバッガーのセクションでは SQL Server のチェック ボックスをオン (図 3 を参照してください)。


[![アプリケーションのプロパティ ページで、SQL Server のチェック ボックスをオンします。](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**図 3**: アプリケーションのプロパティ ページで SQL Server のチェック ボックスをオン ([フルサイズの画像を表示する をクリックします](debugging-stored-procedures-vb/_static/image7.png))。


さらに、接続プールが無効にするために、アプリケーションで使用されるデータベース接続文字列を更新する必要があります。 データベースへの接続が閉じられたときに、対応する`SqlConnection`オブジェクトが使用可能な接続のプールに配置されます。 データベースへの接続を確立するときに、使用可能な接続オブジェクトをこのプールから取得できるではなく作成し、新しい接続を確立することです。 このプールの接続オブジェクトのによってパフォーマンスが向上し、既定で有効です。 ただし、デバッグするときにデバッグ インフラストラクチャは、プールから取得された接続を使用する場合正しく確立しないため、接続がプールを無効にします。

無効になっている接続プールを更新、`NORTHWNDConnectionString`で`Web.config`設定が含まれるように`Pooling=false`します。


[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> 完了したら、ASP.NET アプリケーションを使用して SQL Server、デバッグを必ず接続プールを削除することで元に戻す、`Pooling`接続文字列からの設定 (または設定することで`Pooling=true`)。


この時点で ASP.NET アプリケーションは Visual Studio web アプリケーションから呼び出されたときに、SQL Server データベース オブジェクトをデバッグすることを許可するように構成されています。 後は、ストアド プロシージャにブレークポイントを追加し、デバッグを開始するだけです。

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>手順 3: ブレークポイントを追加して、デバッグ

開く、`Products_SelectByCategoryID`ストアド プロシージャとの先頭にブレークポイントを設定、`SELECT`ステートメントの余白の適切な場所をクリックしてまたはの開始時にカーソルを配置することで、`SELECT`ステートメントと f9 キーを押します。 図 4 に示すように、ブレークポイントは余白に赤い丸が表示されます。


[![ブレークポイントを設定、Products_SelectByCategoryID でストアド プロシージャ](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**図 4**: にブレークポイントを設定、`Products_SelectByCategoryID`ストアド プロシージャ ([フルサイズの画像を表示する をクリックします](debugging-stored-procedures-vb/_static/image10.png))。


SQL データベース オブジェクトのクライアント アプリケーションからデバッグするためには、アプリケーションのデバッグをサポートするために、データベースを構成することが欠かせません。 最初にブレークポイントを設定すると、この設定を切り替えることが自動的に必要がありますを再確認することをお勧めします。 右クリックし、`NORTHWND.MDF`サーバー エクスプ ローラー ノード。 コンテキスト メニューには、チェックされたメニュー項目というアプリケーションのデバッグを含める必要があります。


![アプリケーションのデバッグ オプションが有効になっていることを確認します。](debugging-stored-procedures-vb/_static/image11.png)

**図 5**: アプリケーションのデバッグ オプションが有効になっていることを確認します。


ブレークポイントを設定およびアプリケーションのデバッグ オプションを有効に、ASP.NET アプリケーションから呼び出されたときに、ストアド プロシージャをデバッグする準備が整いました。 デバッグ メニューに移動して、デバッガーを起動し、ツールバーの再生 アイコンが緑をクリックして、f5 キーを押して、デバッグの開始 を選択します。 デバッガーを起動され、web サイトを起動します。

`Products_SelectByCategoryID`でストアド プロシージャが作成された、[型指定されたデータセット s Tableadapter の既存のストアド プロシージャの使用](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)チュートリアル。 対応する web ページ (`~/AdvancedDAL/ExistingSprocs.aspx`) このストアド プロシージャによって返される結果を表示する GridView が含まれています。 ブラウザーからこのページを参照してください。 ページ内のブレークポイントに達すると、`Products_SelectByCategoryID`ストアド プロシージャにヒットして、Visual Studio に制御が返されます。 同じように手順 1 で、ビュー、ストアド プロシージャのステートメントをステップできパラメーター値を変更できます。


[![ExistingSprocs.aspx ページは、飲み物を最初に表示されます。](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**図 6**:`ExistingSprocs.aspx`ページは、飲み物を最初に表示されます ([フルサイズの画像を表示する をクリックします](debugging-stored-procedures-vb/_static/image14.png))。


[![ストアド プロシージャのブレークポイントに達しました](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**図 7**: ブレークポイントに達するストアド プロシージャの s ([フルサイズの画像を表示する をクリックします](debugging-stored-procedures-vb/_static/image17.png))。


図 7 に示すの値のウォッチ ウィンドウとして、`@CategoryID`パラメーターが 1 です。 これは、ため、`ExistingSprocs.aspx`が飲み物のカテゴリで、製品を最初に表示されます ページ、 `CategoryID` 1 の値。 ドロップダウン リストから別のカテゴリを選択します。 これによりポストバックが発生して、再実行します、`Products_SelectByCategoryID`ストアド プロシージャ。 この時間が、もう一度、ブレークポイントにヒット、`@CategoryID`パラメーター s の値がドロップダウン リストを選択したアイテムを反映して`CategoryID`します。


[![ドロップダウン リストから別のカテゴリを選択します。](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**図 8**: ドロップダウン リストから別のカテゴリを選択 ([フルサイズの画像を表示する をクリックします](debugging-stored-procedures-vb/_static/image20.png))。


[![@CategoryIDパラメーターは、Web ページから選択したカテゴリを反映](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**図 9**:`@CategoryID`パラメーターは、Web ページからカテゴリを選択が反映されます ([フルサイズの画像を表示する をクリックします](debugging-stored-procedures-vb/_static/image23.png))。


> [!NOTE]
> 場合内のブレークポイント、`Products_SelectByCategoryID`にアクセスすると、ストアド プロシージャがヒットしない、 `ExistingSprocs.aspx`  ページを ASP.NET アプリケーションのプロパティ ページの デバッガー セクションで、SQL Server のチェック ボックスがチェックされている、接続プールがされているかどうかを確認無効な場合、およびデータベースのオプションのアプリケーションのデバッグが有効になっています。 引き続き再表示される場合は問題が発生 Visual Studio を再起動し、もう一度やり直してください。


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>リモート インスタンスで T-SQL でのデータベース オブジェクトのデバッグ

Visual Studio と同じコンピューター上の SQL Server データベースのインスタンスが、Visual Studio でのデータベース オブジェクトのデバッグは非常に簡単です。 ただし、SQL Server と Visual Studio は、別のコンピューターがある場合、いくつか慎重に構成はすべて正常に動作させる必要しです。 これには 2 つの主要なタスクに直面していますがあります。

- ADO.NET を使用してデータベースへの接続に使用するログインに属していることを確認、`sysadmin`ロール。
- 開発用コンピューターに Visual Studio で使用される Windows ユーザー アカウントが有効な SQL Server ログイン アカウントに属しているであることを確認、`sysadmin`ロール。

最初の手順は比較的簡単です。 最初に、ASP.NET アプリケーションからデータベースに接続し、次に、SQL Server Management Studio でのログイン アカウントを追加するために使用するユーザー アカウントを識別、`sysadmin`ロール。

2 番目のタスクでは、アプリケーションのデバッグに使用する Windows ユーザー アカウントがリモート データベースで有効なログインである必要があります。 ただしを使用して、ワークステーションにログオンする Windows アカウントが SQL Server で有効なログインではない可能性があります。 特定のログイン アカウントを SQL Server に追加するではなく、SQL Server デバッグ アカウントとしていくつかの Windows ユーザー アカウントを指定する方が適切がなります。 次に、リモート SQL Server インスタンスのデータベース オブジェクトをデバッグするには、その Windows ログイン アカウントの資格情報を使用して Visual Studio を実行します。

例は、点をわかりやすく必要があります。 という名前の Windows アカウントがあることを想像してみてください`SQLDebug`Windows ドメイン内で。 このアカウントは有効なログインとのメンバーとして、リモートの SQL Server インスタンスに追加する必要があります、`sysadmin`ロール。 次に、Visual Studio からリモートの SQL Server インスタンスをデバッグするには必要がありますとして Visual Studio を実行する、`SQLDebug`ユーザー。 これにログとして、ワークステーションからログインすることによって行うことが`SQLDebug`、簡単なアプローチでも、Visual Studio を起動しは、独自の資格情報を使用して、ワークステーションにログインし、使用することと`runas.exe`として Visual Studio を起動する、`SQLDebug`ユーザー。 `runas.exe` 別のユーザー アカウントの名前で実行される特定のアプリケーションを使用します。 として Visual Studio を起動する`SQLDebug`コマンドラインで次のステートメントを入力する可能性があります。


[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

このプロセスの詳細については、次を参照してください[William R. Vaughn](http://betav.com/BLOG/billva/) s*銀河を Visual Studio と SQL Server、第 7 版ガイド*だけでなく[How To: SQL Server のアクセス許可の設定。デバッグの](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx)します。

> [!NOTE]
> 開発用コンピューターには、Windows XP Service Pack 2 が実行されている場合は、リモート デバッグを許可するインターネット接続ファイアウォールを構成する必要があります。 [方法を: SQL Server 2005 のデバッグを有効にする](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx)記事では、これには 2 つの手順に関する注意事項: (a) で Visual Studio ホスト コンピューターで、追加する必要があります`Devenv.exe`例外の一覧と、TCP 135 ポートを開くと (b) にリモート (SQL) コンピューターには、開く必要がありますTCP 135 ポートし、追加`sqlservr.exe`例外の一覧にします。 ドメイン ポリシーでは、ネットワーク通信を IPSec を通じて行う必要がある場合、UDP 4500 と UDP 500 のポートを開く必要があります。


## <a name="summary"></a>まとめ

Visual Studio には、.NET アプリケーションのコードのデバッグのサポートを提供するだけでなく、さまざまな SQL Server 2005 のデバッグ オプションも提供します。 このチュートリアルではこれらのオプションの 2 つのしました。 ダイレクト データベース デバッグとアプリケーションのデバッグ。 T-SQL でのデータベース オブジェクトを直接デバッグするには、するには、サーバー エクスプ ローラーを使用してオブジェクトを検索、それを右クリックし、し、ステップ インを選択します。 これにより、デバッガーを起動し、この時点で、オブジェクトのステートメントとビューをステップおよびパラメーターの値を変更、データベース オブジェクトの最初のステートメントで停止します。 手順 1 でステップ インするこのアプローチを使用して、`Products_SelectByCategoryID`ストアド プロシージャ。

アプリケーションのデバッグと、ブレークポイントのデータベース オブジェクト内で直接設定できます。 (ASP.NET web アプリケーションの場合) などのクライアント アプリケーションからブレークポイントを使用して、データベース オブジェクトが呼び出されると、デバッガーは、プログラムを停止します。 アプリケーションのデバッグは、どのようなアプリケーションにより呼び出される特定のデータベース オブジェクトをより明確に示しているために役立ちます。 ただし、少しの構成とダイレクト データベース デバッグよりもセットアップが必要です。

データベース オブジェクトは、SQL Server のプロジェクトからもデバッグできます。 SQL Server プロジェクトを使用してに注目するは、次のチュートリアルでのマネージ データベース オブジェクトを使用して作成およびデバッグする方法。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

> [!div class="step-by-step"]
> [前へ](protecting-connection-strings-and-other-configuration-information-vb.md)
> [次へ](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
