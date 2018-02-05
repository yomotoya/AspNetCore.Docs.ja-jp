---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: "SignalR パフォーマンス カウンターを使用して、Azure Web ロールで |Microsoft ドキュメント"
author: guardrex
description: "インストールして、Azure Web ロールで SignalR パフォーマンス カウンターを使用する方法です。"
keywords: "ASP.NET,signalr,performance カウンター、azure web ロール"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 2f6c6feb030fc17f95e7862c39029569f3d8c5dc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/05/2018
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>SignalR パフォーマンス カウンターを使用して、Azure の Web ロール

作成者: [Luke Latham](https://github.com/guardrex)

SignalR パフォーマンス カウンターは、Azure Web ロールで、アプリのパフォーマンスの監視に使用されます。 カウンターは、Microsoft Azure Diagnostics によってキャプチャされます。 SignalR パフォーマンス カウンターを使用した Azure にインストールする*signalr.exe*、スタンドアロンまたは内部設置型のアプリに使用される、同じツールです。 Azure のロールが一時的なので、アプリをインストールし、起動時の SignalR パフォーマンス カウンターを登録を構成します。

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **注: SDK をインストールした後、コンピューターを再起動します。**
* Microsoft Azure サブスクリプション: 無料の試用アカウントを Azure にサインアップするには、「 [Azure 無料試用版](https://azure.microsoft.com/free/)です。

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>SignalR パフォーマンス カウンターを表示する Azure Web ロール アプリケーションの作成

1. Visual Studio 2015 を開きます。

2. Visual Studio 2015 では、次のように選択します。**ファイル** > **新規** > **プロジェクト**です。

3. **テンプレート**のペイン、**新しいプロジェクト** ウィンドウの下、 **Visual c#**ノードで、選択、**クラウド**ノードを選択、 **Azure クラウド サービス**テンプレート。 アプリの名前を付けます**SignalRPerfCounters**を選択し、 **OK**です。

   ![新しいクラウド アプリケーション](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. **新しい Microsoft Azure クラウド サービス**ダイアログで、選択**ASP.NET Web ロール**を選択し、>、プロジェクトにロールを追加するボタンをクリックします。 **[OK]** を選択します。

   ![ASP.NET Web ロールを追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. **新しい ASP.NET Web アプリケーション - WebRole1**ダイアログで、選択、 **MVC**テンプレート、および選択**OK**です。

   ![MVC と Web API を追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. **ソリューション エクスプ ローラー**を開き、 *diagnostics.wadcfgx*下にあるファイル**WebRole1**です。

   ![ソリューション エクスプ ローラー diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. 次の構成で、ファイルの内容を交換し、ファイルを保存します。

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. 開く、 **Package Manager Console**から**ツール** > **NuGet Package Manager**です。 SignalR、SignalR ユーティリティ パッケージの最新バージョンをインストールするには、次のコマンドを入力します。

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. 起動時やがリサイクルされるときに、ロール インスタンスに SignalR パフォーマンス カウンターをインストールするアプリを構成します。 **ソリューション エクスプ ローラー**を右クリックし、 **WebRole1**プロジェクトし、選択**追加** > **新しいフォルダー**です。 新しいフォルダーの名前を付けます*スタートアップ*です。

   ![スタートアップ フォルダーを追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. コピー、 *signalr.exe*ファイル (を使用して追加、 **Microsoft.AspNet.SignalR.Utils**パッケージ) から\<プロジェクト フォルダー >/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils\< 。バージョン > ツールを開始する、*スタートアップ*前の手順で作成したフォルダーです。

11. **ソリューション エクスプ ローラー**を右クリックし、*スタートアップ*フォルダーと選択**追加** > **既存項目の**します。 表示されるダイアログ ボックスで、次のように選択します。 *signalr.exe*選択**追加**です。

    ![Signalr.exe をプロジェクトに追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. 右クリックし、*スタートアップ*フォルダーを作成します。 **[追加]** > **[新しい項目]** の順に選択します。 選択、**全般**ノード、**テキスト ファイル**、し、新しい項目の名前を付けます*SignalRPerfCounterInstall.cmd*です。 このコマンド ファイルは、SignalR パフォーマンス カウンターを web ロールにインストールされます。

    ![SignalR パフォーマンス カウンターのインストールのバッチ ファイルを作成します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Visual Studio を作成すると、 *SignalRPerfCounterInstall.cmd*ファイルが自動的に開きますのメイン ウィンドウでします。 次のスクリプト ファイルの内容を置き換えますし、保存してファイルを閉じます。 このスクリプトは実行*signalr.exe*、ロール インスタンスに SignalR パフォーマンス カウンターを追加します。

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. 選択、 *signalr.exe*ファイル**ソリューション エクスプ ローラー**です。 ファイルの**プロパティ**設定、**出力ディレクトリにコピー**に**常にコピー**です。

    ![常にコピーする出力ディレクトリにコピーを設定します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. 前の手順を繰り返して、 *SignalRPerfCounterInstall.cmd*ファイル。

    
16. 右クリックし、 *SignalRPerfCounterInstall.cmd*ファイルおよび選択した**ファイルを開く**です。 表示されるダイアログ ボックスで、次のように選択します。**バイナリ エディター**選択**OK**です。

    ![バイナリ エディターで開く](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. バイナリ エディターで、ファイルの先頭バイトを選択し、削除します。 ファイルを保存して閉じます。

    ![先頭バイトを削除します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. 開いている*ServiceDefinition.csdef*を実行するスタートアップ タスクを追加して、 *SignalrPerfCounterInstall.cmd*ファイル、サービスを開始するとき。

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. 開いている`Views/Shared/_Layout.cshtml`と jQuery のバンドル スクリプト ファイルの末尾から削除します。

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. 継続的に呼び出す JavaScript クライアントを追加、`increment`サーバー上のメソッドです。 開いている`Views/Home/Index.cshtml`し、内容を次のコードに置き換えます。

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. 新しいフォルダーを作成、 **WebRole1**という名前のプロジェクト*ハブ*です。 右クリックし、*ハブ*フォルダー**ソリューション エクスプ ローラー** **Web** > **SignalR**を選択し、 **SignalR ハブ クラス (v2)**です。 新しいハブの名前を付けます*MyHub.cs*選択**追加**です。

    ![SignalR ハブ クラス フォルダーへの追加、ハブで新しい項目の追加 ダイアログ](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs*が自動的にメイン ウィンドウで開きます。 内容を次のコードに置き換え、保存したファイルを閉じます。

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  SignalR コードベースで提供されるツールのテスト接続密度がします。 クランクは、永続的な接続を必要とするため追加する 1 つを使用するサイトをテストする場合。 新しいフォルダーを追加、 **WebRole1**というプロジェクト*PersistentConnections*です。 このフォルダーを右クリックし **追加** > **クラス**です。 新しいクラス ファイルの名前を付けます*MyPersistentConnections.cs*選択**追加**です。

24. Visual Studio で開く、 *MyPersistentConnections.cs*メイン ウィンドウ内のファイルです。 内容を次のコードに置き換え、保存したファイルを閉じます。

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. 使用して、 `Startup` OWIN の起動時にはクラス、SignalR のオブジェクトを開始します。 開くか作成*Startup.cs*し、内容を次のコードに置き換えます。

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    上記のコードで、`OwinStartup`属性は、OWIN を開始するには、このクラスをマークします。 `Configuration`メソッドは、SignalR を開始します。
    
26. Microsoft Azure エミュレーターでキーを押してアプリケーションをテスト**f5 キーを押して**です。

    > [!NOTE]
    > 発生した場合、 **FileLoadException**で**MapSignalR**でバインド リダイレクトを変更する*web.config*以下。

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. 約 1 分間待機します。 Visual Studio で、クラウド エクスプ ローラー ツール ウィンドウを開きます (**ビュー** > **Cloud Explorer**) パスを展開および`(Local)/Storage Accounts/(Development)/Tables`です。 ダブルクリックして**WADPerformanceCountersTable**です。 SignalR カウンター テーブル データが表示されます。 テーブルが見つからない場合は、Azure ストレージの資格情報を再入力する必要があります。 選択する必要があります、**更新**内のテーブルを表示するボタン**Cloud Explorer**かを選択して、**更新**をテーブル内のデータを表示するテーブルを開くウィンドウのボタンをクリックします。

    ![Visual Studio Cloud Explorer で WAD パフォーマンス カウンター テーブルを選択します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD パフォーマンス カウンター テーブルで収集されたカウンターの表示](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. クラウドでアプリケーションをテストするには、更新、 **ServiceConfiguration.Cloud.cscfg**ファイルし、設定、`Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`に有効な Azure ストレージ アカウント接続文字列。

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Azure サブスクリプションへのアプリケーションを展開します。 Azure にアプリケーションを展開する方法の詳細については、「[を作成し、クラウド サービスを展開する方法](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)です。

30. 数分待ってからです。 **Cloud Explorer**上で構成したストレージ アカウントを見つけて、検索、`WADPerformanceCountersTable`内のテーブルです。 SignalR カウンター テーブル データが表示されます。 テーブルが見つからない場合は、Azure ストレージの資格情報を再入力する必要があります。 選択する必要があります、**更新**内のテーブルを表示するボタン**Cloud Explorer**かを選択して、**更新**をテーブル内のデータを表示するテーブルを開くウィンドウのボタンをクリックします。

感謝の特別な[Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard)の元の内容をこのチュートリアルで使用します。
