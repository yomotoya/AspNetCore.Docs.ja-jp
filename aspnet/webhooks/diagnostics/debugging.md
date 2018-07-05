---
uid: webhooks/diagnostics/debugging
title: ASP.NET Webhook のデバッグ |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook をデバッグする方法。
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801989"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="8e5e9-103">ASP.NET Webhook のデバッグ</span><span class="sxs-lookup"><span data-stu-id="8e5e9-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="8e5e9-104">Azure でのデバッグ</span><span class="sxs-lookup"><span data-stu-id="8e5e9-104">Debugging in Azure</span></span>

<span data-ttu-id="8e5e9-105">Azure で実行中に、Web アプリケーションをデバッグするチュートリアルを参照してください[Visual Studio を使用して Azure App Service で web アプリのトラブルシューティング](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)します。</span><span class="sxs-lookup"><span data-stu-id="8e5e9-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="8e5e9-106">ソースとシンボルを使用したデバッグ</span><span class="sxs-lookup"><span data-stu-id="8e5e9-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="8e5e9-107">に加えて、独自のコードをデバッグするには、および実際には、Microsoft ASP.NET Webhook に直接デバッグすることはすべて .NET の。</span><span class="sxs-lookup"><span data-stu-id="8e5e9-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="8e5e9-108">これは、ローカルまたはリモートでデバッグするかどうかに関係なく動作します。</span><span class="sxs-lookup"><span data-stu-id="8e5e9-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="8e5e9-109">Visual Studio を選択して、ソースとシンボルの検索を最初に、構成**デバッグ**し**オプションと設定**します。</span><span class="sxs-lookup"><span data-stu-id="8e5e9-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="8e5e9-110">このようなオプションを設定します。</span><span class="sxs-lookup"><span data-stu-id="8e5e9-110">Set the options like this:</span></span>

![オプションと設定](_static/SourceSymbols.png)

<span data-ttu-id="8e5e9-112">リンクを追加し、 [symbolsource.org](http://symbolsource.org)ソースとシンボルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="8e5e9-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="8e5e9-113">移動して、**シンボル**上部のメニューのタブとシンボルの場所として、次を追加。</span><span class="sxs-lookup"><span data-stu-id="8e5e9-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="8e5e9-114">さらに、キャッシュ ディレクトリに短い名前であることを確認します。それ以外の場合、ファイル名は、シンボルを読み込むことができませんは長すぎます取得できます。</span><span class="sxs-lookup"><span data-stu-id="8e5e9-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="8e5e9-115">パスのサンプルに示します。</span><span class="sxs-lookup"><span data-stu-id="8e5e9-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="8e5e9-116">設定は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="8e5e9-116">The settings should look similar to this:</span></span>

![オプションのシンボル ファイルの場所の例](_static/SymSource.png)
