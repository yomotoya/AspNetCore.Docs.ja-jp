---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Azure Web ロールで SignalR パフォーマンス カウンターの使用 |Microsoft Docs
author: guardrex
description: インストールして Azure Web ロールで SignalR パフォーマンス カウンターを使用する方法。
keywords: ASP.NET,signalr,performance カウンター、azure の web ロール
ms.author: aspnetcontent
ms.date: 02/11/2017
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: b082e4052efa468543e7c2d92e4795234941beeb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840077"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Azure Web ロールで SignalR パフォーマンス カウンターの使用

作成者: [Luke Latham](https://github.com/guardrex)

SignalR パフォーマンス カウンターは、Azure Web ロールで、アプリのパフォーマンスを監視するために使用されます。 カウンターは、Microsoft Azure Diagnostics によってキャプチャされます。 Azure で SignalR パフォーマンス カウンターをインストールする*signalr.exe*を同じツールをスタンドアロンまたはオンプレミスのアプリで使用します。 Azure ロールでは、一時的なものであるために、アプリをインストールし、起動時に SignalR パフォーマンス カウンターの登録を構成します。

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Microsoft の Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **注: SDK のインストール後、コンピューターを再起動します。**
* Microsoft Azure サブスクリプション: Azure の無料試用版アカウントのサインアップを参照してください。 [Azure 無料試用版](https://azure.microsoft.com/free/)します。

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>SignalR パフォーマンス カウンターを公開する Azure Web ロールのアプリケーションの作成

1. Visual Studio 2015 を開きます。

2. Visual Studio 2015 では、次のように選択します。**ファイル** > **新規** > **プロジェクト**します。

3. **テンプレート**のウィンドウ、**新しいプロジェクト**] ウィンドウの [、 **Visual c#** ノードを選択、**クラウド**ノードと選択、 **Azure クラウド サービス**テンプレート。 アプリの名前を付けます**SignalRPerfCounters**選択 **[ok]** します。

   ![新しいクラウド アプリケーション](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. **新しい Microsoft Azure クラウド サービス**ダイアログ ボックスで、 **ASP.NET Web ロール**を選択し、>、ロールをプロジェクトに追加するボタンをクリックします。 **[OK]** を選択します。

   ![ASP.NET Web ロールを追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. **新しい ASP.NET Web アプリケーション - WebRole1**ダイアログ ボックスで、 **MVC**テンプレート、および選択し**OK**。

   ![MVC と Web API を追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. **ソリューション エクスプ ローラー**、オープン、 *diagnostics.wadcfgx*ファイル**WebRole1**します。

   ![ソリューション エクスプ ローラーの場合は diagnostics.wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. ファイルの内容を置き換えて、次の構成をファイルを保存します。

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. 開く、**パッケージ マネージャー コンソール**から**ツール** > **NuGet パッケージ マネージャー**します。 SignalR、SignalR ユーティリティ パッケージの最新バージョンをインストールするには、次のコマンドを入力します。

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. 起動時やがリサイクルされるときに、ロール インスタンスに SignalR パフォーマンス カウンターをインストールするアプリを構成します。 **ソリューション エクスプ ローラー**を右クリックし、 **WebRole1**順に選択して**追加** > **新しいフォルダー**します。 新しいフォルダーの名前*スタートアップ*します。

   ![スタートアップ フォルダーを追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. コピー、 *signalr.exe*ファイル (を使用して追加、 **Microsoft.AspNet.SignalR.Utils**パッケージ) から\<プロジェクト フォルダー >/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils\< 。バージョン >/するツール、*スタートアップ*前の手順で作成したフォルダーです。

11. **ソリューション エクスプ ローラー**を右クリックし、*スタートアップ*フォルダーと選択**追加** > **既存項目の**します。 表示されるダイアログ ボックスで、次のように選択します。 *signalr.exe*選択**追加**します。

    ![Signalr.exe をプロジェクトに追加します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. 右クリックし、*スタートアップ*フォルダーを作成します。 **[追加]** > **[新しい項目]** の順に選択します。 選択、**全般**ノードを選択**テキスト ファイル**、新しい項目の名前と*SignalRPerfCounterInstall.cmd*します。 このコマンド ファイルは、SignalR パフォーマンス カウンターを web ロールにインストールされます。

    ![SignalR パフォーマンス カウンターのインストール バッチ ファイルを作成します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Visual Studio で作成すると、 *SignalRPerfCounterInstall.cmd*ファイルが自動的に開きますのメイン ウィンドウにします。 次のスクリプトを使用して、ファイルの内容を交換し、保存して、ファイルを閉じます。 このスクリプトが実行される*signalr.exe*、ロール インスタンスに SignalR パフォーマンス カウンターを追加します。

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. 選択、 *signalr.exe*ファイル**ソリューション エクスプ ローラー**します。 ファイルの**プロパティ**設定**出力ディレクトリにコピー**に**常にコピー**します。

    ![コピーを常にコピーする出力ディレクトリに設定します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. 前の手順を繰り返して、 *SignalRPerfCounterInstall.cmd*ファイル。

    
16. 右クリックし、 *SignalRPerfCounterInstall.cmd*ファイルおよび選択**プログラムから開く**します。 表示されるダイアログ ボックスで、次のように選択します。**バイナリ エディター**選択と**OK**します。

    ![バイナリ エディターで開く](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. バイナリ エディターでは、ファイルの先頭バイトを選択し、削除します。 ファイルを保存して閉じます。

    ![先頭バイトを削除します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. 開いている*ServiceDefinition.csdef*を実行するスタートアップ タスクを追加して、 *SignalrPerfCounterInstall.cmd*ファイル サービスの起動時に。

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. 開いている`Views/Shared/_Layout.cshtml`と jQuery のバンドルのスクリプト ファイルの末尾から削除します。

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. 継続的に呼び出す JavaScript クライアントを追加、`increment`サーバー上のメソッド。 開いている`Views/Home/Index.cshtml`内容を次のコードに置き換えます。

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. 新しいフォルダーを作成、 **WebRole1**という名前のプロジェクト*Hubs*します。 右クリックし、 *Hubs*フォルダー**ソリューション エクスプ ローラー**を選択します**Web** > **SignalR**、選び**SignalR ハブ クラス (v2)** します。 新しいハブの名前を付けます*MyHub.cs*選択**追加**します。

    ![[新しい項目の追加] ダイアログ ボックスのハブ フォルダーに追加する SignalR ハブ クラス](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs*のメイン ウィンドウが自動的に開きます。 内容を次のコードに置き換えますを保存し、ファイルを閉じます。

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)* 接続密度テスト SignalR コードベースで提供されるツールです。 Crank は、永続的な接続を必要とするために追加するサイトを使用してテストするときにします。 新しいフォルダーを追加、 **WebRole1**という名前のプロジェクト*PersistentConnections*します。 このフォルダーを右クリックして**追加** > **クラス**します。 新しいクラス ファイルに名前*MyPersistentConnections.cs*選択**追加**します。

24. Visual Studio が開き、 *MyPersistentConnections.cs*メイン ウィンドウ内のファイル。 内容を次のコードに置き換えますを保存し、ファイルを閉じます。

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. 使用して、`Startup`クラス、SignalR オブジェクトは、OWIN の起動時を起動します。 開くか作成*Startup.cs*内容を次のコードに置き換えます。

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    上記のコードで、`OwinStartup`属性は OWIN を開始するには、このクラスをマークします。 `Configuration`メソッドは、SignalR を開始します。
    
26. Microsoft Azure エミュレーターでキーを押してアプリケーションをテスト**F5**します。

    > [!NOTE]
    > 発生した場合、 **FileLoadException**で**MapSignalR**でバインド リダイレクトを変更する*web.config*次。

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. 約 1 分間待機します。 Visual Studio で Cloud Explorer ツール ウィンドウを開きます (**ビュー** > **Cloud Explorer**) パスを展開および`(Local)/Storage Accounts/(Development)/Tables`します。 ダブルクリック**WADPerformanceCountersTable**します。 テーブルのデータで SignalR カウンターが表示されます。 テーブルが表示されない場合は、Azure ストレージの資格情報を再入力する必要があります。 選択する必要があります、**更新**ボタンをクリックして、テーブルで**Cloud Explorer**または選択、**更新**テーブル内のデータを表示するテーブルを開くウィンドウのボタン。

    ![Visual Studio Cloud Explorer で WAD パフォーマンス カウンター テーブルを選択します。](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![WAD パフォーマンス カウンターのテーブルで収集されたカウンターの表示](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. クラウドでアプリケーションをテストするには、更新、 **ServiceConfiguration.Cloud.cscfg**ファイルし、設定、`Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString`有効な Azure Storage アカウント接続文字列にします。

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Azure サブスクリプションにアプリケーションを展開します。 アプリケーションを Azure にデプロイする方法の詳細については、次を参照してください。[を作成して、クラウド サービスをデプロイする方法](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)します。

30. 数分待ってから。 **Cloud Explorer**先ほど構成したストレージ アカウントを見つけて、検索、`WADPerformanceCountersTable`内のテーブル。 テーブルのデータで SignalR カウンターが表示されます。 テーブルが表示されない場合は、Azure ストレージの資格情報を再入力する必要があります。 選択する必要があります、**更新**ボタンをクリックして、テーブルで**Cloud Explorer**または選択、**更新**テーブル内のデータを表示するテーブルを開くウィンドウのボタン。

感謝します。 特別な[Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard)の元のコンテンツをこのチュートリアルで使用します。
