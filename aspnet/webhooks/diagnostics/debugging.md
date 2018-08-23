---
uid: webhooks/diagnostics/debugging
title: ASP.NET Webhook のデバッグ |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook をデバッグする方法。
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837451"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="0ea7f-103">ASP.NET Webhook のデバッグ</span><span class="sxs-lookup"><span data-stu-id="0ea7f-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="0ea7f-104">Azure でのデバッグ</span><span class="sxs-lookup"><span data-stu-id="0ea7f-104">Debugging in Azure</span></span>

<span data-ttu-id="0ea7f-105">Azure で実行中に、Web アプリケーションをデバッグするチュートリアルを参照してください[Visual Studio を使用して Azure App Service で web アプリのトラブルシューティング](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)します。</span><span class="sxs-lookup"><span data-stu-id="0ea7f-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="0ea7f-106">ソースとシンボルを使用したデバッグ</span><span class="sxs-lookup"><span data-stu-id="0ea7f-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="0ea7f-107">に加えて、独自のコードをデバッグするには、および実際には、Microsoft ASP.NET Webhook に直接デバッグすることはすべて .NET の。</span><span class="sxs-lookup"><span data-stu-id="0ea7f-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="0ea7f-108">これは、ローカルまたはリモートでデバッグするかどうかに関係なく動作します。</span><span class="sxs-lookup"><span data-stu-id="0ea7f-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="0ea7f-109">Visual Studio を選択して、ソースとシンボルの検索を最初に、構成**デバッグ**し**オプションと設定**します。</span><span class="sxs-lookup"><span data-stu-id="0ea7f-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="0ea7f-110">このようなオプションを設定します。</span><span class="sxs-lookup"><span data-stu-id="0ea7f-110">Set the options like this:</span></span>

![オプションと設定](_static/SourceSymbols.png)

<span data-ttu-id="0ea7f-112">リンクを追加し、 [symbolsource.org](http://symbolsource.org)ソースとシンボルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="0ea7f-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="0ea7f-113">移動して、**シンボル**上部のメニューのタブとシンボルの場所として、次を追加。</span><span class="sxs-lookup"><span data-stu-id="0ea7f-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="0ea7f-114">さらに、キャッシュ ディレクトリに短い名前であることを確認します。それ以外の場合、ファイル名は、シンボルを読み込むことができませんは長すぎます取得できます。</span><span class="sxs-lookup"><span data-stu-id="0ea7f-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="0ea7f-115">パスのサンプルに示します。</span><span class="sxs-lookup"><span data-stu-id="0ea7f-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="0ea7f-116">設定は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="0ea7f-116">The settings should look similar to this:</span></span>

![オプションのシンボル ファイルの場所の例](_static/SymSource.png)
