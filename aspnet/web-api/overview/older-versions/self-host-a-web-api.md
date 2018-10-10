---
uid: web-api/overview/older-versions/self-host-a-web-api
title: セルフホスト ASP.NET Web API 1 (c#) |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API では、IIS は必要ありません。 Web API は、独自のホスト プロセスで自己ホストできます。 このチュートリアルでは、アプリのコンソール内で web API をホストする方法を使用しています.
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 63d192a6fa2aafef3770d5b0b97ec32e001b69db
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912703"
---
<a name="self-host-aspnet-web-api-1-c"></a>ASP.NET Web API 1 (c#) を自己ホストします。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> ASP.NET Web API では、IIS は必要ありません。 Web API は、独自のホスト プロセスで自己ホストできます。 このチュートリアルでは、コンソール アプリケーション内部の web API をホストする方法を示します。
> 
> **新しいアプリケーションは、OWIN を使用して、Web API を自己ホストする必要があります。** 参照してください[OWIN を使用して、ASP.NET Web API 2 をセルフホスト](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Web API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>コンソール アプリケーション プロジェクトを作成します。

Visual Studio を起動し、選択**新しいプロジェクト**から、**開始**ページ。 またはから、**ファイル**メニューの **新規**し**プロジェクト**します。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Windows**します。 プロジェクト テンプレートの一覧で選択**コンソール アプリケーション**します。 プロジェクトに名前を&quot;SelfHost&quot;  をクリック**OK**します。

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>ターゲット フレームワーク (Visual Studio 2010) に設定します。

Visual Studio 2010 を使用している場合は、.NET Framework 4.0 をターゲット フレームワークを変更します。 (既定では、プロジェクト テンプレートの対象として、 [.Net Framework クライアント プロファイル](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile))。

ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**プロパティ**します。 **ターゲット フレームワーク**ドロップダウン リストで、ターゲット フレームワークを .NET Framework 4.0 に変更します。 変更を適用するメッセージが表示されたら、クリックして**はい**します。

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>NuGet パッケージ マネージャーをインストールします。

NuGet パッケージ マネージャーは、Web API アセンブリを非 ASP.NET プロジェクトに追加する最も簡単な方法です。

NuGet パッケージ マネージャーがインストールされていることを確認するには、クリックして、**ツール**Visual Studio のメニュー。 メニュー項目を表示する場合は**NuGet パッケージ マネージャー**、NuGet パッケージ マネージャーがあります。

NuGet パッケージ マネージャーをインストールするには

1. Visual Studio を起動します。
2. **[ツール]** メニューから **[拡張機能と更新プログラム]** を選択します。
3. **拡張機能と更新**ダイアログ ボックスで、**オンライン**します。
4. 「NuGet パッケージ マネージャー」が表示されない場合は、検索ボックスに「nuget パッケージ マネージャー」を入力します。
5. NuGet パッケージ マネージャーを選択し、クリックして**ダウンロード**します。
6. ダウンロードの完了後は、インストールを促されます。
7. インストールの完了後は、Visual Studio を再起動する求め可能性があります。

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Web API の NuGet パッケージを追加します。

NuGet パッケージ マネージャーをインストールした後は、プロジェクトに Web API Self-Host パッケージを追加します。

1. **ツール**メニューの  **NuGet パッケージ マネージャー**します。 *注*: かどうかは表示されないこのメニュー項目をその NuGet パッケージ マネージャーが正しくインストールされていることを確認します。
2. 選択**ソリューションの NuGet パッケージの管理**
3. **NugGet パッケージの管理**ダイアログ ボックスで、**オンライン**します。
4. 検索ボックスに「 &quot;Microsoft.AspNet.WebApi.SelfHost&quot;します。
5. ASP.NET Web API の自己ホスト パッケージを選択し、クリックして**インストール**します。
6. パッケージをインストールした後にをクリックして**閉じます**ダイアログ ボックスを閉じます。

> [!NOTE]
> Microsoft.AspNet.WebApi.SelfHost、AspNetWebApi.SelfHost しないという名前のパッケージをインストールしてください。

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>モデルとコント ローラーを作成します。

このチュートリアルと同じモデルとコント ローラー クラスを使用して、 [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)チュートリアル。

という名前のパブリック クラスを追加`Product`します。

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

という名前のパブリック クラスを追加`ProductsController`します。 このクラスから派生**System.Web.Http.ApiController**します。

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

このコント ローラー コードの詳細については、次を参照してください。、 [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)チュートリアル。 このコント ローラーには、次の 3 つの GET 操作を定義します。

| URI | 説明 |
| --- | --- |
| /api/products | すべての製品の一覧を取得します。 |
| /api/products/*id* | ID によって製品を取得します。 |
| /api/products/?category=*category* | カテゴリ別、製品の一覧を取得します。 |

## <a name="host-the-web-api"></a>Web API をホストします。

Program.cs ファイルを開き、次を追加するステートメントを使用します。

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

次のコードを追加、**プログラム**クラス。

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(省略可能)HTTP URL Namespace 予約を追加します。

このアプリケーションがリッスン`http://localhost:8080/`します。 既定では、特定の HTTP アドレスでリッスンしている管理者特権が必要です。 このチュートリアルを実行するときにそのため、エラーが発生この:"HTTP URL を登録できませんでした http://+:8080/" はこのエラーを回避するために 2 つの方法があります。

- Visual Studio を管理者として昇格されたアクセス許可を持つ実行または
- Netsh.exe を使用して、URL を予約するアカウントのアクセス許可を与えます。

Netsh.exe を使用して、管理者特権でコマンド プロンプトを開きし、次のコマンド: 次のコマンドを入力します。

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

場所*machine \username など*は、ユーザー アカウントです。

自己ホストを終了したら、必ず、予約を削除します。

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>クライアント アプリケーション (c#) から Web API の呼び出し

Web API を呼び出す単純なコンソール アプリケーションを記述してみましょう。

新しいコンソール アプリケーション プロジェクトをソリューションに追加します。

- ソリューション エクスプ ローラーでソリューションを右クリックし、選択**新しいプロジェクトの追加**します。
- という名前の新しいコンソール アプリケーションを作成&quot;ClientApp&quot;します。

![](self-host-a-web-api/_static/image5.png)

ASP.NET Web API のコア ライブラリのパッケージを追加するには、NuGet パッケージ マネージャーを使用します。

- [ツール] メニューで、次のように選択します。 **NuGet パッケージ マネージャー**します。
- 選択**ソリューションの NuGet パッケージの管理**
- **NuGet パッケージの管理**ダイアログ ボックスで、**オンライン**します。
- 検索ボックスに「 &quot;Microsoft.AspNet.WebApi.Client&quot;します。
- Microsoft ASP.NET Web API クライアント ライブラリのパッケージを選択し、クリックして**インストール**します。

ClientApp で自己ホスト型プロジェクトへの参照を追加します。

- ソリューション エクスプ ローラーで、ClientApp プロジェクトを右クリックします。
- **[参照の追加]** をクリックします。
- **参照マネージャー**ダイアログで、**ソリューション**を選択します**プロジェクト**します。
- 自己ホスト型プロジェクトを選択します。
- **[OK]** をクリックします。

![](self-host-a-web-api/_static/image6.png)

Client/Program.cs ファイルを開きます。 次の追加**を使用して**ステートメント。

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

追加の静的な**HttpClient**インスタンス。

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

カテゴリ別一覧、製品 ID を使用して、すべての製品と製品の一覧を一覧表示する次のメソッドを追加します。

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

これらの各メソッドには、同じパターンがとおりです。

1. 呼び出す**HttpClient.GetAsync**適切な URI に GET 要求を送信します。
2. 呼び出す**HttpResponseMessage.EnsureSuccessStatusCode**します。 このメソッドは、HTTP 応答の状態がエラー コードの場合、例外をスローします。
3. 呼び出す**ReadAsAsync&lt;T&gt;** を HTTP 応答から CLR 型を逆シリアル化します。 このメソッドで定義されている、拡張メソッドは、 **System.Net.Http.HttpContentExtensions**します。

**GetAsync**と**ReadAsAsync**メソッドは、非同期のどちらもします。 返される**タスク**非同期操作を表すオブジェクト。 取得、**結果**プロパティは、操作が完了するまでスレッドをブロックします。

非ブロッキング呼び出しを実行する方法など、HttpClient を使用しての詳細については、次を参照してください。 [Web API から、.NET クライアントを呼び出す](../advanced/calling-a-web-api-from-a-net-client.md)します。

これらのメソッドを呼び出す前に BaseAddress プロパティを設定する HttpClient インスタンス"`http://localhost:8080`"。 例えば:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

これは、次を出力します。 (自己ホスト型アプリケーションを最初に実行してください。)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
