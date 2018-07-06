---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: SignalR 1.x プロジェクトをバージョン 2 にアップグレード |Microsoft Docs
author: pfletcher
description: このトピックでは、SignalR に既存の SignalR 1.x プロジェクトをアップグレードする方法を説明します 2.x、およびアップグレードの処理中に発生する可能性のある問題をトラブルシューティングする方法.。
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: 393beb1ef696bd2dfae25789f79a67157780a219
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824164"
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a>SignalR 1.x プロジェクトをバージョン 2 にアップグレードします。
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> このトピックでは、SignalR に既存の SignalR 1.x プロジェクトをアップグレードする方法を説明します。 2.x、およびアップグレードの処理中に発生する可能性のある問題をトラブルシューティングする方法。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR のバージョン 1 と 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>このチュートリアルで Visual Studio 2012 の使用
> 
> 
> このチュートリアルでは、Visual Studio 2012 を使用するには、次の操作を行います。
> 
> - 更新プログラム、[パッケージ マネージャー](http://docs.nuget.org/docs/start-here/installing-nuget)最新バージョンにします。
> - インストール、 [Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)します。
> - Web Platform Installer で検索してインストール**ASP.NET と Visual Studio 2012 for Web Tools 2013.1**します。 SignalR クラスの Visual Studio テンプレートなどのインストールはこの**ハブ**します。
> - 一部のテンプレート (など**OWIN Startup クラス**) はできなくなります。 これらの場合には、クラス ファイルを代わりに使用します。
> 
> 
> ## <a name="questions-and-comments"></a>意見やご質問
> 
> このチュートリアルの立った方法と、ページの下部にあるコメントで改良できるフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)します。


SignalR 2 を使用して server プラットフォーム間で一貫性のある開発エクスペリエンスを提供する[OWIN](http://owin.org)します。 この記事では、バージョン 2 に SignalR 1.x アプリケーションを更新するために必要ないくつかの手順について説明します。

SignalR 2 のアプリケーションをアップグレードすることをお勧め、SignalR 1.x でもがサポートされています。

このチュートリアルでは、SignalR 2 の web ホスト アプリケーションをアップグレードする方法について説明します。 自己ホスト型アプリケーション (コンソール アプリケーション、Windows サービス、または他のプロセス内のサーバーをホストしているもの) は、SignalR 2 でサポートされるようになりました。 SignalR 2 による自己ホスト型アプリケーションの作成を開始する方法については、次を参照してください。[チュートリアル: SignalR セルフホスト](../deployment/tutorial-signalr-self-host.md)します。

## <a name="contents"></a>目次

次のセクションでは、SignalR プロジェクト、および発生する可能性のある問題をトラブルシューティングする方法へのアップグレードに関連するタスクについて説明します。

- [例: SignalR 2 のチュートリアル入門のアップグレード](#example)
- [アップグレード中に発生したエラーのトラブルシューティング](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a>例: SignalR 2 の概要チュートリアル アプリケーションのアップグレード

このセクションで作成したアプリケーションを更新します、[チュートリアル入門の SignalR 1.x バージョン](../older-versions/index.md)SignalR 2 を使用します。

1. 作業開始のチュートリアルが終了したら、プロジェクトを右クリックし、選択**プロパティ**します。 いることを確認、**ターゲット フレームワーク**に設定されている **.NET Framework 4.5。**
2. パッケージ マネージャー コンソールを開きます。 削除 SignalR 1.x は、次のコマンドを使用して、プロジェクトから。

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. 次のコマンドを使用する SignalR 2 をインストールします。

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. HTML ページでは、ここで、プロジェクトに含まれるスクリプトのバージョンと一致する SignalR のスクリプト参照を更新します。

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. グローバル アプリケーション クラスでは、MapHubs への呼び出しを削除します。

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. ソリューションを右クリックして**追加**、**新しい項目.**.ダイアログ ボックスで、次のように選択します。 **Owin Startup クラス**します。 新しいクラスの名前**Startup.cs**します。

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. Startup.cs の内容を次のコードに置き換えます。

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    アセンブリ属性を実行する Owin のスタートアップ プロセスをクラスを追加します。、 `Configuration` Owin の開始時のメソッド。 さらに呼び出される、`MapSignalR`メソッドで、アプリケーションのすべての SignalR ハブのルートを作成します。
8. プロジェクトを実行し、メイン ページの URL をコピー ブラウザーまたはブラウザー ウィンドウで以前のようにします。 各ページで、ユーザー名が求められ、各ページから送信されたメッセージは両方のブラウザー ウィンドウに表示されます。

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a>アップグレード中に発生したエラーのトラブルシューティング

このセクションでは、アップグレード中に発生する可能性のある問題について説明します。 エラーと SignalR アプリケーションで発生する可能性のある問題の詳細な一覧を参照してください。 [SignalR トラブルシューティング](../testing-and-debugging/troubleshooting.md)します。

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a>' の呼び出しでは、次のメソッドまたはプロパティ間であいまいです '

参照が、このエラーが発生`Microsoft.AspNet.SignalR.Owin`は削除されません。 このパッケージは非推奨とされます。参照を削除する必要があり、自己ホスト型のパッケージの 1.x バージョンをアンインストールする必要があります。

### <a name="hub-methods-fail-silently"></a>ハブ メソッドがサイレント モードで失敗します。

クライアント スクリプト参照が最新、していることを確認、 `OwinStartup` Startup クラスがある、適切なクラスと、プロジェクトのアセンブリ名の属性します。 また、お使いのブラウザーでハブ アドレス (signalr/ハブ) を開いて再試行してください。問題の原因の詳細については、表示されるすべてのエラーが提供されます。
