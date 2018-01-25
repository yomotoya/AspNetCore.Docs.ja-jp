---
uid: web-api/overview/older-versions/self-host-a-web-api
title: "ASP.NET Web API 1 (c#) を自己ホスト |Microsoft ドキュメント"
author: MikeWasson
description: "ASP.NET Web API には、IIS は不要です。 自己ホスト プロセスを独自の web API をホストできます。 このチュートリアルでは、applic コンソール内の web API をホストする方法を説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a>ASP.NET Web API 1 (c#) を自己ホストします。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web API には、IIS は不要です。 自己ホスト プロセスを独自の web API をホストできます。 このチュートリアルでは、コンソール アプリケーション内部の web API をホストする方法を示します。
> 
> **新しいアプリケーションでは、Web API の自己ホストするのに OWIN を使用する必要があります。** 参照してください[OWIN を使用して ASP.NET Web API 2 を自己ホスト](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)です。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Web API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>コンソール アプリケーション プロジェクトを作成します。

Visual Studio を起動し、選択**新しいプロジェクト**から、**開始**ページ。 またはから、**ファイル**メニューの **新規**し**プロジェクト**です。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#**ノード。 **Visual c#** **Windows**です。 プロジェクト テンプレートの一覧で選択**コンソール アプリケーション**です。 プロジェクトに名前を&quot;SelfHost&quot;  をクリック**OK**です。

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>ターゲット フレームワーク (Visual Studio 2010) の設定します。

Visual Studio 2010 を使用している場合は、.NET Framework 4.0 をターゲット フレームワークを変更します。 (既定では、プロジェクト テンプレートの対象、 [.Net Framework クライアント プロファイル](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile))。

ソリューション エクスプ ローラーでプロジェクトを右クリックし、**プロパティ**です。 **ターゲット フレームワーク** ドロップダウン リストで、ターゲット フレームワークを .NET Framework 4.0 に変更します。 変更を適用するメッセージが表示されたら、クリックして**はい**です。

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>NuGet Package Manager をインストールします。

NuGet パッケージ マネージャーは、Web API アセンブリを ASP.NET 以外のプロジェクトに追加する最も簡単な方法です。

NuGet Package Manager がインストールされているかどうかをチェックするをクリックして、**ツール**Visual Studio のメニュー。 メニュー項目を表示する場合は**ライブラリ パッケージ マネージャー**、NuGet Package Manager 必要があります。

NuGet Package Manager をインストールします。

1. Visual Studio を起動します。
2. **ツール**メニューの **拡張機能と更新プログラム**です。
3. **拡張機能と更新**ダイアログで、**オンライン**です。
4. "NuGet Package Manager"が表示されない場合は、検索ボックスに"nuget package manager"を入力します。
5. NuGet パッケージ マネージャーを選択し、クリックして**ダウンロード**です。
6. ダウンロードが完了したら、インストールが求められます。
7. インストールが完了したら、Visual Studio を再起動するように求められます可能性があります。

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Web API NuGet パッケージを追加します。

NuGet Package Manager をインストールした後は、Web API Self-Host パッケージをプロジェクトに追加します。

1. **ツール**メニューの **ライブラリ パッケージ マネージャー**です。 *注*: は表示されませんこのメニュー項目、NuGet Package Manager は正しくインストールされていることを確認してください。
2. 選択**ソリューションの NuGet パッケージを管理しています.**
3. **注目パッケージの管理**ダイアログで、**オンライン**です。
4. [検索] ボックスに入力&quot;Microsoft.AspNet.WebApi.SelfHost&quot;です。
5. ASP.NET Web API Self Host パッケージを選択し、をクリックして**インストール**です。
6. パッケージをインストールした後にをクリックして**閉じる**ダイアログ ボックスを閉じます。

> [!NOTE]
> Microsoft.AspNet.WebApi.SelfHost、いない AspNetWebApi.SelfHost をという名前のパッケージをインストールすることを確認してください。


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>モデルとコント ローラーを作成します。

このチュートリアルは、同じモデルとコント ローラーとしてのクラスを使用して、[作業の開始](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)チュートリアルです。

という名前のパブリック クラスを追加`Product`です。

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

という名前のパブリック クラスを追加`ProductsController`です。 このクラスから派生**System.Web.Http.ApiController**です。

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

このコント ローラーでコードの詳細については、次を参照してください。、[作業の開始](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)チュートリアルです。 このコント ローラーには、次の 3 つの GET 操作を定義します。

| URI | 説明 |
| --- | --- |
| /api/products | すべての製品の一覧を取得します。 |
| /api/products/*id* | ID によって製品を取得します。 |
| /api/products/?category=*category* | カテゴリによって製品の一覧を取得します。 |

## <a name="host-the-web-api"></a>Web API をホストします。

Program.cs ファイルを開き、次の追加のステートメントを使用します。

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

次のコードを追加、**プログラム**クラスです。

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(省略可能)HTTP URL Namespace 予約を追加します。

このアプリケーションがリッスンする`http://localhost:8080/`です。 既定では、特定の HTTP アドレスでリッスンするいると、管理者特権が必要です。 このチュートリアルを実行するときにそのため、エラーが発生この:「HTTP が URL http://+:8080/ を登録できません」はこのエラーを回避する 2 つの方法があります。

- 管理者特権での管理者のアクセス許可で Visual Studio を実行または
- Netsh.exe を使用して、URL を予約するアカウントのアクセス許可を与えます。

Netsh.exe を使用して、管理者特権でコマンド プロンプトを開きし、次のコマンド: 次のコマンドを入力します。

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

ここで*machine \username*は自分のユーザー アカウントです。

完了したら、自己ホスト型を必ず、予約を削除します。

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Web API、クライアント アプリケーションから呼び出す (c#)

Web API を呼び出す単純なコンソール アプリケーションを作成してみましょう。

新しいコンソール アプリケーション プロジェクトをソリューションに追加します。

- ソリューション エクスプ ローラーでソリューションを右クリックし、**新しいプロジェクトの追加**です。
- という名前の新しいコンソール アプリケーションを作成する&quot;ClientApp&quot;です。

![](self-host-a-web-api/_static/image5.png)

NuGet パッケージ マネージャーを使用して、ASP.NET Web API Core Libraries パッケージを追加します。

- [ツール] メニューから選択**ライブラリ パッケージ マネージャー**です。
- 選択**ソリューションの NuGet パッケージを管理しています.**
- **NuGet パッケージの管理**ダイアログで、**オンライン**です。
- [検索] ボックスに入力&quot;Microsoft.AspNet.WebApi.Client&quot;です。
- Microsoft ASP.NET Web API Client Libraries パッケージを選択し、をクリックして**インストール**です。

ClientApp で自己ホスト型プロジェクトへの参照を追加します。

- ソリューション エクスプ ローラーで、ClientApp プロジェクトを右クリックします。
- 選択**参照の追加**です。
- **参照マネージャー** ] ダイアログで、**ソリューション**[**プロジェクト**です。
- 自己ホスト型プロジェクトを選択します。
- **[OK]**をクリックします。

![](self-host-a-web-api/_static/image6.png)

Client/Program.cs ファイルを開きます。 次の追加**を使用して**ステートメント。

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

静的なを追加**HttpClient**インスタンス。

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

リスト ID を使用して製品のすべての製品と製品の一覧をカテゴリ別一覧を次のメソッドを追加します。

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

これらの各メソッドには、同様のパターンが次に示します。

1. 呼び出す**HttpClient.GetAsync**適切な URI に GET 要求を送信します。
2. 呼び出す**HttpResponseMessage.EnsureSuccessStatusCode**です。 このメソッドは、HTTP 応答状態がエラー コードの場合、例外をスローします。
3. 呼び出す**ReadAsAsync&lt;T&gt;** を HTTP 応答から CLR 型を逆シリアル化します。 このメソッドで定義されている、拡張メソッドは、 **System.Net.Http.HttpContentExtensions**です。

**されます**と**ReadAsAsync**メソッドは、非同期の両方です。 返される**タスク**を非同期操作を表すオブジェクト。 取得する、**結果**プロパティは、操作が完了するまでに、スレッドをブロックします。

詳細については、非ブロッキング呼び出しを作成する方法を含む、HttpClient を使用して次を参照してください。 [Web API から、.NET クライアントを呼び出す](../advanced/calling-a-web-api-from-a-net-client.md)です。

これらのメソッドを呼び出す前に BaseAddress プロパティを設定する HttpClient インスタンス"`http://localhost:8080`"です。 例:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

これは、次を出力します。 (最初に、自己ホスト型アプリケーションを実行してください。)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
