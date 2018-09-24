---
title: ASP.NET Core アプリで生成済みページを更新する
author: rick-anderson
description: ASP.NET Core アプリで生成済みページを更新する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 78490be1cfa3018c465cb1e8125918404a7e4525
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011613"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="c0d3a-103">ASP.NET Core アプリで生成済みページを更新する</span><span class="sxs-lookup"><span data-stu-id="c0d3a-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="c0d3a-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0d3a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c0d3a-105">ムービー アプリは上々の滑り出しでしたが、表示が理想的ではありません。</span><span class="sxs-lookup"><span data-stu-id="c0d3a-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="c0d3a-106">時刻の表示が好ましくなく (下の画像の 12:00:00 AM)、**ReleaseDate** は **Release Date** (2 つの単語) にするべきです。</span><span class="sxs-lookup"><span data-stu-id="c0d3a-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![ムービー データが表示された、Chrome で開かれているムービー アプリケーション](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="c0d3a-108">生成されたコードの更新</span><span class="sxs-lookup"><span data-stu-id="c0d3a-108">Update the generated code</span></span>

<span data-ttu-id="c0d3a-109">*Models/Movie.cs* ファイルを開き、下のコードで強調表示されている行を追加します。</span><span class="sxs-lookup"><span data-stu-id="c0d3a-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="c0d3a-110">赤の波線を右クリックし、**[クイック アクションとリファクタリング]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c0d3a-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![コンテキスト メニューに **[クイック アクションとリファクタリング]** が表示されます。](da1/qa.png)

<span data-ttu-id="c0d3a-112">`using System.ComponentModel.DataAnnotations;` を選択します。</span><span class="sxs-lookup"><span data-stu-id="c0d3a-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![一覧の一番上にある System.ComponentModel.DataAnnotations を使用する](da1/da.png)

  <span data-ttu-id="c0d3a-114">Visual Studio により `using System.ComponentModel.DataAnnotations;` が追加されます。</span><span class="sxs-lookup"><span data-stu-id="c0d3a-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="c0d3a-115">[前: SQL Server LocalDB の使用](xref:tutorials/razor-pages/sql)
> [検索の追加](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="c0d3a-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
