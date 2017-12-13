---
uid: webhooks/diagnostics/debugging
title: "デバッグ ASP.NET Webhook |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET Webhook をデバッグする方法です。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="4a196-103">ASP.NET の Webhook のデバッグ</span><span class="sxs-lookup"><span data-stu-id="4a196-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="4a196-104">Azure でのデバッグ</span><span class="sxs-lookup"><span data-stu-id="4a196-104">Debugging in Azure</span></span>

<span data-ttu-id="4a196-105">Azure で実行中に、Web アプリケーションをデバッグするには、このチュートリアルに参照してください[Visual Studio を使用して Azure App service web アプリのトラブルシューティングを行う](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)です。</span><span class="sxs-lookup"><span data-stu-id="4a196-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="4a196-106">ソースとシンボルでのデバッグ</span><span class="sxs-lookup"><span data-stu-id="4a196-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="4a196-107">に加えて、独自のコードをデバッグするには、Microsoft ASP.NET の Webhook に直接と実際にデバッグすることはすべて .NET のです。</span><span class="sxs-lookup"><span data-stu-id="4a196-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="4a196-108">これは、ローカルまたはリモートでデバッグするかどうかに関係なく動作します。</span><span class="sxs-lookup"><span data-stu-id="4a196-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="4a196-109">まず、構成に移動して、ソースとシンボルを検索する Visual Studio**デバッグ**し**オプションと設定**です。</span><span class="sxs-lookup"><span data-stu-id="4a196-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="4a196-110">次のようにオプションを設定します。</span><span class="sxs-lookup"><span data-stu-id="4a196-110">Set the options like this:</span></span>

![オプションと設定](_static/SourceSymbols.png)

<span data-ttu-id="4a196-112">リンクを追加[symbolsource.org](http://symbolsource.org)ソースとシンボルをダウンロードするためです。</span><span class="sxs-lookup"><span data-stu-id="4a196-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="4a196-113">移動して、**シンボル**タブ上のメニューのシンボルの場所として、以下を追加。</span><span class="sxs-lookup"><span data-stu-id="4a196-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="4a196-114">さらに、キャッシュ ディレクトリに短い名前があることを確認します。それ以外の場合、ファイル名を取得できますが長すぎますをロードしないシンボルが発生します。</span><span class="sxs-lookup"><span data-stu-id="4a196-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="4a196-115">サンプルのパスは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="4a196-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="4a196-116">設定は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="4a196-116">The settings should look similar to this:</span></span>

![オプションのシンボル ファイルの場所の例](_static/SymSource.png)
