---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: バージョン 2 に SignalR 1.x プロジェクトのアップグレード |Microsoft ドキュメント
author: pfletcher
description: SignalR に既存の SignalR 1.x プロジェクトをアップグレードする方法を説明 2.x、およびアップグレードの処理中に発生する可能性のある問題をトラブルシューティングする方法.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26505741"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>SignalR 1.x プロジェクトをバージョン 2 にアップグレードします。
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> SignalR に既存の SignalR 1.x プロジェクトをアップグレードする方法を説明 2.x、およびアップグレードの処理中に発生する可能性のある問題をトラブルシューティングする方法です。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR バージョン 1 と 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>このチュートリアルで Visual Studio 2012 を使用します。
> 
> 
> このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。
> 
> - 更新プログラム、 [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。
> - インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)です。
> - Web Platform Installer で検索し、インストール**ASP.NET と Visual Studio 2012 用 Web ツール 2013.1**です。 SignalR クラス用の Visual Studio テンプレートなどのインストールはこの**ハブ**です。
> - 一部のテンプレート (など**OWIN スタートアップ クラス**) できなくなります。 これらの場合には、クラス ファイルを代わりに使用します。
> 
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


SignalR 2 を使用してサーバー プラットフォーム間で一貫した開発エクスペリエンスを提供する[OWIN](http://owin.org)です。 この記事では、SignalR 1.x アプリケーションをバージョン 2 に更新するために必要ないくつかの手順について説明します。

SignalR 2 にアプリケーションをアップグレードすることをお勧め、1.x でも SignalR でサポートされています。

このチュートリアルでは、SignalR 2 に web ホスト アプリケーションをアップグレードする方法について説明します。 SignalR 2 では、自己ホスト型アプリケーション (コンソール アプリケーション、Windows サービス、またはその他のプロセス内のサーバーをホストしているもの) はサポートされています。 SignalR 2 自己ホスト型アプリケーションの作成を開始する方法については、次を参照してください。[チュートリアル: 自己ホスト SignalR](../deployment/tutorial-signalr-self-host.md)です。

## <a name="contents"></a>目次

次のセクションでは、SignalR プロジェクト、および発生する可能性のある問題をトラブルシューティングする方法のアップグレードに関連するタスクについて説明します。

- [例: SignalR 2 へのチュートリアル入門のアップグレード](#example)
- [アップグレード中に発生したエラーのトラブルシューティング](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>例: SignalR 2 に概要チュートリアル アプリケーションのアップグレード

このセクションで作成したアプリケーションを更新するされます、[チュートリアル入門の SignalR 1.x バージョン](../older-versions/index.md)SignalR 2 を使用します。

1. チュートリアル入門が終了したら後、プロジェクトを右クリックし、選択**プロパティ**です。 いることを確認、**ターゲット フレームワーク**に設定されている **.NET Framework 4.5。**
2. パッケージ マネージャー コンソールを開きます。 SignalR を削除する、次のコマンドを使用してプロジェクトから 1.x:

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. 次のコマンドを使用して SignalR 2 をインストールします。

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. HTML ページで、プロジェクトに含まれるスクリプトのバージョンと一致する SignalR のスクリプト参照を更新します。

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. グローバル アプリケーション クラスでは、MapHubs への呼び出しを削除します。

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. ソリューションを右クリックし **追加**、**新しい項目の追加.**.ダイアログ ボックスで、次のように選択します。 **Owin スタートアップ クラス**です。 新しいクラスの名前を**Startup.cs**です。

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. 次のコードでは、Startup.cs の内容を置き換えます。

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    アセンブリの属性を実行する Owin の起動プロセスをクラスを追加、`Configuration`メソッド Owin 起動するとします。 これを呼び出して、`MapSignalR`メソッドで、アプリケーションのすべての SignalR ハブのルートを作成します。
8. プロジェクトを実行し、別に、メイン ページの URL をコピー ブラウザーまたはブラウザー ウィンドウで以前のようにします。 各ページは、ユーザー名を求められ、各ページから送信されたメッセージは両方のブラウザー ウィンドウに表示されます。

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>アップグレード中に発生したエラーのトラブルシューティング

このセクションでは、アップグレード中に発生する可能性のある問題について説明します。 包括的なの SignalR アプリケーションで発生する可能性のあるエラーや問題の一覧を表示するには、次を参照してください。 [SignalR トラブルシューティング](../testing-and-debugging/troubleshooting.md)です。

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>' 呼び出しが次のメソッドまたはプロパティ間であいまいな '

参照がこのエラーが発生`Microsoft.AspNet.SignalR.Owin`は削除されません。 このパッケージは廃止されました。参照を削除する必要があります、自己ホスト型のパッケージのバージョン 1.x をアンインストールする必要があります。

### <a name="hub-methods-fail-silently"></a>ハブ メソッドがサイレント モードで失敗します。

日付、およびするまで、クライアント スクリプト参照があることを確認、`OwinStartup`スタートアップ クラスには、正しいクラスと、プロジェクトのアセンブリ名の属性です。 またからハブに指定されたアドレス (signalr/ハブ) お使いのブラウザーを開いてください。表示されるエラーの詳細については、問題の原因が提供されます。
