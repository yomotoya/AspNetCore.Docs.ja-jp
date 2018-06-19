---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: ストアド プロシージャ (VB) のデバッグ |Microsoft ドキュメント
author: rick-anderson
description: Visual Studio Professional edition および Team System edition を使用すると、ブレークポイントを設定し、SQL Server のストアド プロシージャにステップで、格納されているにデバッグする.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: 3391a78eaeb0add46e75048069a614ba00628f67
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876450"
---
<a name="debugging-stored-procedures-vb"></a>ストアド プロシージャ (VB) のデバッグ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip)または[PDF のダウンロード](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> Visual Studio Professional edition および Team System edition を使用すると、ブレークポイントを設定し、SQL Server のストアド プロシージャにステップで、アプリケーション コードのデバッグと同じくらい簡単ストアド プロシージャのデバッグを作成できます。 このチュートリアルでは、ダイレクト データベース デバッグと、アプリケーションは、ストアド プロシージャのデバッグについて説明します。


## <a name="introduction"></a>はじめに

Visual Studio には、豊富なデバッグ機能が用意されています。 いくつかのキー ストロークやマウスのクリックで、ブレークポイントを使用して、プログラムの実行を停止し、その状態と制御フローを確認することが可能です。 アプリケーション コードをデバッグするとは、Visual Studio は、SQL Server 内からのストアド プロシージャのデバッグのサポートを提供します。 ASP.NET 分離コード クラスまたはビジネス ロジック層クラスのコード内でブレークポイントを設定するのと同じように、すぎることができますに配置するストアド プロシージャ内で。

このチュートリアルでを見てみましょうにステップ イン ストアド プロシージャから Visual Studio 内でサーバー エクスプ ローラーにもヒットしたブレークポイントを設定するストアド プロシージャが呼び出されたとき、実行中の ASP.NET アプリケーションからどのようにします。

> [!NOTE]
> 残念ながら、ストアド プロシージャのみにステップ インして、Visual Studio のバージョンの Professional とチームのシステム バージョンをデバッグします。 Visual Web Developer または標準のバージョンの Visual Studio を使用している場合は、ストアド プロシージャをデバッグするための手順を進めながら、コンピューターでこれらの手順をレプリケートすることはできません、に沿って読み取りへようこそ。


## <a name="sql-server-debugging-concepts"></a>SQL Server デバッグの概念

Microsoft SQL Server 2005 の設計との統合環境を[共通言語ランタイム (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx)、これは、すべての .NET アセンブリで使用されるランタイム。 その結果、SQL Server 2005 では、マネージ データベース オブジェクトをサポートしています。 つまり、Visual Basic のクラスのメソッドとして、ストアド プロシージャおよびユーザー定義関数 (Udf) のようなデータベース オブジェクトを作成できます。 これにより、これらのストアド プロシージャおよび Udf を .NET Framework では、独自のカスタム クラスからの機能を利用できます。 もちろん、SQL Server 2005 は、T-SQL でデータベース オブジェクトに対するサポートも提供しています。

SQL Server 2005 では、T-SQL とマネージ データベース オブジェクトの両方に対するデバッグのサポートを提供します。 ただし、これらのオブジェクトは、Visual Studio 2005 Professional とチームのシステムのエディションでのみデバッグできます。 このチュートリアルでは、デバッグの T-SQL でデータベース オブジェクトについて調べます。 以降のチュートリアルは、マネージ データベース オブジェクトのデバッグで検索します。

[概要の T-SQL と CLR の SQL Server 2005 でデバッグ](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx)からブログ エントリ、 [SQL Server 2005 の CLR 統合のチーム](https://blogs.msdn.com/sqlclr/default.aspx)Visual Studio から SQL Server 2005 のオブジェクトをデバッグする 3 つの方法を示しています。

- **データベースのデバッグ (DDD) を直接**- サーバー エクスプ ローラーのストアド プロシージャおよび Udf など、すべての T-SQL でデータベース オブジェクトにはステップ インできます。 手順 1. で DDD を見ていきます。
- **アプリケーションのデバッグ**-データベース オブジェクト内のブレークポイントを設定したり、ASP.NET アプリケーションを実行できます。 データベース オブジェクトを実行すると、ブレークポイントにヒットして、コントロールがデバッガーに引き継がです。 アプリケーションのデバッグとおできませんステップをデータベース オブジェクトにアプリケーション コードからに注意してください。 お必要があります明示的にブレークポイントを設定でこれらのストアド プロシージャまたは Udf デバッガーを停止します。 アプリケーションのデバッグが検査手順 2. で開始されます。
- **SQL Server プロジェクトからデバッグを**-Visual Studio Professional とチームのシステムのエディションにはマネージ データベース オブジェクトの作成によく使用される SQL Server のプロジェクトの種類が含まれます。 SQL Server のプロジェクトを使用してを見ていきますとその内容を次のチュートリアルでデバッグします。

Visual Studio では、ローカルおよびリモートの SQL Server インスタンス上のストアド プロシージャをデバッグできます。 ローカルの SQL Server インスタンスとは、Visual Studio と同じコンピューターにインストールされているです。 使用している SQL Server データベースが開発用コンピューターにない場合は、リモート インスタンスを見なされます。 これらのチュートリアルの使用していたローカルの SQL Server インスタンス。 リモート SQL server インスタンス上のストアド プロシージャをデバッグするには、ローカル インスタンス上のストアド プロシージャのデバッグよりも多くの構成手順が必要です。

ローカル SQL Server インスタンスを使用している場合は、手順 1 から始まります。 と末尾には、このチュートリアルです。 リモートの SQL Server インスタンスを使用している場合が表示されます、開発用コンピューターをリモート インスタンスで SQL Server ログインを持つ Windows ユーザー アカウントを使用することを確認するをデバッグするときに最初の必要性がログに記録されます。 Moveover、このデータベースのログインと、実行中の ASP.NET アプリケーションからデータベースへの接続に使用するデータベース ログインの両方のメンバーである必要があります、`sysadmin`ロール。 リモート インスタンスをデバッグするには、Visual Studio および SQL Server の構成の詳細については、このチュートリアルの最後に、リモート インスタンスのセクションで、デバッグの T-SQL でデータベース オブジェクトを参照してください。

最後に、T-SQL でデータベース オブジェクトのデバッグをサポートされている .NET アプリケーションのデバッグをサポートと豊富な機能としてを理解します。 たとえば、ブレークポイントの条件およびフィルターはサポートされていません、のみ、デバッグ ウィンドウのサブセットが使用可能なエディット コンティニュを使用することはできません、役に立たないなど、イミディ エイト ウィンドウが表示されます。 参照してください[デバッガー コマンドおよび機能に関する制限事項](https://msdn.microsoft.com/library/ms165035(VS.80).aspx)詳細についてはします。

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>手順 1: は、ストアド プロシージャに直接ステップ イン

Visual Studio では、データベース オブジェクトを直接デバッグが容易にします。 S、ダイレクト データベース デバッグ (DDD) 機能を使用して、ステップ インする方法を確認できるように、`Products_SelectByCategoryID`ストアド プロシージャを Northwind データベースでします。 その名前からわかるように、`Products_SelectByCategoryID`が特定のカテゴリの製品情報を返しますで作成されて、[を使用して既存のストアド プロシージャを型指定されたデータセットの Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)チュートリアルです。 サーバー エクスプ ローラーに移動して起動し、Northwind データベース ノードを展開します。 次に、ストアド プロシージャ フォルダーにドリル ダウンを右クリックし、`Products_SelectByCategoryID`ストアド プロシージャ、およびコンテキスト メニューからストアド プロシージャにステップ イン オプションを選択します。 これにより、デバッガーが開始されます。

`Products_SelectByCategoryID`ストアド プロシージャが必要ですが、`@CategoryID`入力パラメーターは、この値を指定する求められます。 これにより、飲み物に関する情報が返されます 1 を入力します。


![値 1 を使用して、@CategoryIDパラメーター](debugging-stored-procedures-vb/_static/image1.png)

**図 1**: の値 1 を使用して、`@CategoryID`パラメーター


値を提供した後、`@CategoryID`パラメーター、ストアド プロシージャを実行します。 を完了するまで実行しているのではなく、デバッガーが停止の最初のステートメントを実行します。 ストアド プロシージャの現在の位置を示す、余白の黄色の矢印に注意してください。 表示して、[ウォッチ] ウィンドウ、またはストアド プロシージャでパラメーター名の上にマウスでパラメーター値を編集します。


[![ストアド プロシージャの最初のステートメントでデバッガーが停止しました](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**図 2**: ストアド プロシージャの最初のステートメントでデバッガーが停止しました ([フルサイズのイメージを表示するをクリックして](debugging-stored-procedures-vb/_static/image4.png))


一度に 1 つのストアド プロシージャのステートメントをステップ、ツールバーのステップ オーバー ボタンをクリックしてまたは F10 キーを押すとします。 `Products_SelectByCategoryID`ストアド プロシージャには、1 つが含まれています。`SELECT`ステートメントでは、1 つのステートメントにステップ オーバーし、ストアド プロシージャの実行が完了されます f10 キーを押すようにします。 ストアド プロシージャが完了すると、その出力は、出力ウィンドウに表示されます、デバッガーが終了します。

> [!NOTE]
> ステートメント レベルで発生する T-SQL デバッグステップ インすることはできません、`SELECT`ステートメントです。


## <a name="step-2-configuring-the-website-for-application-debugging"></a>手順 2: アプリケーションのデバッグの web サイトを構成します。

サーバー エクスプ ローラーから直接ストアド プロシージャのデバッグは便利ですが、多くのシナリオで興味のあるより ASP.NET アプリケーションから呼び出された場合、ストアド プロシージャをデバッグします。 Visual Studio 内からのストアド プロシージャにブレークポイントを追加したり、ASP.NET アプリケーションのデバッグを開始してできます。 アプリケーションからのブレークポイントを持つストアド プロシージャが呼び出されると、実行がブレークポイントで停止しますおできます表示およびとストアド プロシージャのパラメーター値の変更手順 1 で行ったのと同じように、そのステートメントをステップします。

アプリケーションから呼び出されたストアド プロシージャのデバッグを始めることができます、前にデバッガーでは、SQL Server を統合する ASP.NET web アプリケーションに指示する必要があります。 ソリューション エクスプ ローラーで web サイト名を右クリックして開始 (`ASPNET_Data_Tutorial_74_VB`)。 コンテキスト メニューからプロパティ ページのオプションを選択し、左側の 開始オプション項目を選択し、デバッガー セクションで SQL Server のチェック ボックスをオン (図 3 を参照してください)。


[![アプリケーションのプロパティ ページで、SQL Server のチェック ボックスをオンします。](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**図 3**: アプリケーションのプロパティ ページで SQL Server のチェック ボックスをオン ([フルサイズのイメージを表示するをクリックして](debugging-stored-procedures-vb/_static/image7.png))


さらに、接続プールを無効にするように、アプリケーションで使用されるデータベース接続文字列を更新する必要があります。 データベースへの接続が閉じられたときに、対応する`SqlConnection`オブジェクトが使用可能な接続のプールに配置されます。 データベースへの接続を確立するときに使用可能な接続オブジェクトこのプールから取得できるではなくを作成し、新しい接続を確立することです。 このプールの接続オブジェクトのパフォーマンスが向上し、既定で有効にします。 ただし、デバッグするときにデバッグ インフラストラクチャがない正しく再確立しましたがプールから作成された接続を使用する場合、接続がプールをオフにします。

無効になっている接続プールを更新、`NORTHWNDConnectionString`で`Web.config`設定が含まれるように`Pooling=false`です。


[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> 終了したら、ASP.NET アプリケーションを使用して SQL Server、デバッグを必ず接続プールを削除することで元に戻す、`Pooling`接続文字列から設定 (または設定することによって`Pooling=true`)。


この時点で、ASP.NET アプリケーションされている Visual Studio での web アプリケーションから呼び出されたときに、SQL Server データベース オブジェクトをデバッグするようにします。 ストアド プロシージャにブレークポイントを追加し、デバッグを開始するようになりましたの残りのすべてが!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>手順 3: ブレークポイントを追加して、デバッグ

開く、`Products_SelectByCategoryID`ストアド プロシージャとの先頭にブレークポイントを設定、`SELECT`ステートメントの適切な場所にある余白をクリックして、またはの開始時にカーソルを配置することによって、`SELECT`ステートメントと f9 キーを押します。 図 4 に示すように、ブレークポイントが余白に赤い円として表示されます。


[![ブレークポイントを設定、Products_SelectByCategoryID でストアド プロシージャ](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**図 4**: にブレークポイントを設定、`Products_SelectByCategoryID`ストアド プロシージャ ([フルサイズのイメージを表示するをクリックして](debugging-stored-procedures-vb/_static/image10.png))


SQL データベース オブジェクトをクライアント アプリケーションをデバッグするためには、アプリケーションのデバッグをサポートするために、データベースを構成することが不可欠です。 最初にブレークポイントを設定すると、この設定は、自動的に切り替える必要がありますを再確認することをお勧めします。 右クリックし、`NORTHWND.MDF`サーバー エクスプ ローラー内のノードです。 コンテキスト メニューには、checked というメニュー項目アプリケーションのデバッグを含める必要があります。


![アプリケーションのデバッグ オプションが有効になっていることを確認してください。](debugging-stored-procedures-vb/_static/image11.png)

**図 5**: アプリケーションのデバッグ オプションが有効になっていることを確認してください。


ブレークポイントを設定およびアプリケーションのデバッグ オプションを有効に、ASP.NET アプリケーションから呼び出された場合は、ストアド プロシージャをデバッグする準備が整いました。 デバッグ メニューに移動してデバッガーを起動し、ツールバーでの再生 アイコンの f5 キーを押すか、緑をクリックして、デバッグの開始 を選択します。 デバッガーを起動され、web サイトを起動します。

`Products_SelectByCategoryID`でストアド プロシージャが作成された、[を使用して既存のストアド プロシージャを型指定されたデータセットの Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)チュートリアルです。 対応する web ページ (`~/AdvancedDAL/ExistingSprocs.aspx`) このストアド プロシージャによって返される結果を表示する GridView が含まれています。 ブラウザーからこのページを参照してください。 ページ内のブレークポイントに達すると、`Products_SelectByCategoryID`ストアド プロシージャにヒットして、Visual Studio に制御が返されます。 同じように手順 1 で、ビュー、ストアド プロシージャのステートメントをステップできパラメーターの値を変更できます。


[![ExistingSprocs.aspx ページ、飲み物を最初に表示します。](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**図 6**:`ExistingSprocs.aspx`ページ、飲み物を最初に表示されます ([フルサイズのイメージを表示するをクリックして](debugging-stored-procedures-vb/_static/image14.png))


[![ストアド プロシージャの s ブレークポイントに到達しました](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**図 7**: ストアド プロシージャの s がブレークポイントに達した ([フルサイズのイメージを表示するをクリックして](debugging-stored-procedures-vb/_static/image17.png))


図 7 に示すの値のウォッチ ウィンドウとして、`@CategoryID`パラメーターは 1 です。 これは、ため、`ExistingSprocs.aspx`ページが最初が飲料カテゴリで製品を表示、 `CategoryID` 1 の値。 ドロップダウン リストから別のカテゴリを選択します。 そうポストバックを発生させるし、再実行、`Products_SelectByCategoryID`ストアド プロシージャです。 再び、今度は、ブレークポイントがヒットしたら、`@CategoryID`パラメーター s の値がドロップダウン リストを選択したアイテムを反映して`CategoryID`です。


[![ドロップダウン リストから別のカテゴリを選択します。](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**図 8**: ドロップダウン リストから別のカテゴリを選択 ([フルサイズのイメージを表示するをクリックして](debugging-stored-procedures-vb/_static/image20.png))


[![@CategoryIDパラメーターは、Web ページから選択したカテゴリを反映](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**図 9**:`@CategoryID`パラメーターは、Web ページからカテゴリを選択を反映 ([フルサイズのイメージを表示するをクリックして](debugging-stored-procedures-vb/_static/image23.png))


> [!NOTE]
> 場合内のブレークポイント、`Products_SelectByCategoryID`にアクセスすると、ストアド プロシージャはヒットしませんが、 `ExistingSprocs.aspx`  ページで、ASP.NET アプリケーションのプロパティ ページの デバッガー セクションで、SQL Server のチェック ボックスがチェックされたこと、接続プールされていることを確認してください無効な場合、およびデータベースのオプションのアプリケーションのデバッグが有効になっています。 Re それでも問題が発生が Visual Studio を再起動し、もう一度やり直してください。


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>リモート インスタンス上の T-SQL でデータベース オブジェクトのデバッグ

SQL Server データベース インスタンスが Visual Studio と同じコンピューターに、Visual Studio でのデータベース オブジェクトのデバッグはきわめて簡単です。 ただし、SQL Server および Visual Studio は、異なるコンピューター上に存在する場合、いくつか慎重に構成が必要に正常に動作してコンポーネントをすべて入手です。 直面してお 2 つの主要なタスクがあります。

- ADO.NET を使用してデータベースへの接続に使用するログインに属していることを確認してください、`sysadmin`ロール。
- Visual Studio によって開発用コンピューターで使用する Windows ユーザー アカウントが有効な SQL Server ログイン アカウントに属しているであることを確認、`sysadmin`ロール。

最初の手順では、比較的簡単です。 最初に、ASP.NET アプリケーションからデータベースに接続し、次に、SQL Server Management Studio からそのログイン アカウントを追加するために使用するユーザー アカウントを特定、`sysadmin`ロール。

2 番目のタスクでは、アプリケーションのデバッグを使用する Windows ユーザー アカウントがリモート データベースで有効なログインである必要があります。 ただしでワークステーションにログオンした Windows アカウントが SQL Server 上の有効なログインが高くなります。 SQL Server に、特定のログイン アカウントを追加するのではなく、SQL Server デバッグ アカウントとしてその Windows ユーザー アカウントを指定する方が適切になります。 次に、リモートの SQL Server インスタンスのデータベース オブジェクトをデバッグするには、その Windows ログイン アカウントの資格情報を使用して Visual Studio を実行します。

例は、点をわかりやすくなります。 という名前の Windows アカウントがあることを想像`SQLDebug`Windows ドメイン内で。 このアカウントは有効なログインおよびのメンバーとして、リモートの SQL Server インスタンスに追加する必要があります、`sysadmin`ロール。 次に、Visual Studio からリモートの SQL Server インスタンスをデバッグするには必要がありますとして Visual Studio を実行する、`SQLDebug`ユーザー。 これは、弊社のワークステーションからログに記録としてのログ バックアップで行うことが`SQLDebug`、独自の資格情報を使用して、ワークステーションにログインし、使用することが、Visual Studio で、簡単な方法を起動および`runas.exe`として Visual Studio を起動する、`SQLDebug`ユーザー。 `runas.exe` 別のユーザー アカウントの装ってで実行される特定のアプリケーションを許可します。 Visual Studio を起動する`SQLDebug`コマンドラインで次のステートメントを入力する可能性があります。


[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

このプロセスの詳細については、次を参照してください[William R. Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker s Visual Studio と SQL Server, エディションの 7 番目のガイド*だけでなく[操作方法: SQL Server のアクセス許可の設定。デバッグ用](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx)です。

> [!NOTE]
> 開発用コンピューターには、Windows XP Service Pack 2 が実行されている場合は、リモート デバッグを行うためのインターネット接続ファイアウォールを構成する必要があります。 [方法を: SQL Server 2005 のデバッグを有効にする](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx)資料がこれには 2 つの手順を示します: (a)、Visual Studio ホスト コンピューターに、追加する必要があります`Devenv.exe`例外の一覧と、TCP 135 ポートを開くと (b) にリモート (SQL) コンピューターには、開く必要がありますTCP 135 ポートを追加`sqlservr.exe`例外の一覧にします。 ドメイン ポリシーでは、ネットワーク通信を IPSec を使用して行う必要がある場合、UDP 4500 と UDP 500 のポートを開く必要があります。


## <a name="summary"></a>まとめ

Visual Studio は、.NET アプリケーションのコードのデバッグのサポートを提供するだけでなく、さまざまな SQL Server 2005 のデバッグ オプションも提供します。 このチュートリアルではこれらのオプションの 2 つに考えた: ダイレクト データベース デバッグと、アプリケーションのデバッグします。 T-SQL は、データベース オブジェクトを直接デバッグするには、サーバー エクスプ ローラーからオブジェクトを検索し、右クリックし、ステップ インします。 これにより、デバッガーを起動し、この時点でオブジェクトのステートメントとビューをステップし、パラメーター値の変更は、データベース オブジェクトに最初のステートメントで停止します。 手順 1. で使用して、このアプローチにステップ イン、`Products_SelectByCategoryID`ストアド プロシージャです。

アプリケーションのデバッグと、ブレークポイントのデータベース オブジェクト内で直接設定できます。 (ASP.NET web アプリケーションの場合) などのクライアント アプリケーションからのブレークポイントを使用して、データベース オブジェクトが呼び出されると、デバッガーは、プログラムが停止します。 アプリケーションのデバッグは、どのようなアプリケーション アクションが呼び出される特定のデータベース オブジェクトがより明確に表示されるために役立ちます。 ただし、少しの構成とダイレクト データベース デバッグよりもセットアップが必要です。

データベース オブジェクトは、SQL Server のプロジェクトをデバッグできます。 SQL Server のプロジェクトを使用して見ていき、次のチュートリアルでのマネージ データベース オブジェクトを使用して作成およびデバッグする方法です。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

> [!div class="step-by-step"]
> [前へ](protecting-connection-strings-and-other-configuration-information-vb.md)
> [次へ](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
