---
title: ASP.NET Core アプリで生成済みページを更新する
author: rick-anderson
description: ASP.NET Core アプリで生成済みページを更新する方法について説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: f47a68840a6307b69bc92a7b157037d91dce5422
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477216"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="a082f-103">ASP.NET Core アプリで生成済みページを更新する</span><span class="sxs-lookup"><span data-stu-id="a082f-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="a082f-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a082f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a082f-105">ムービー アプリは上々の滑り出しでしたが、表示が理想的ではありません。</span><span class="sxs-lookup"><span data-stu-id="a082f-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="a082f-106">時刻の表示が好ましくなく (下の画像の 12:00:00 AM)、**ReleaseDate** は **Release Date** (2 つの単語) にするべきです。</span><span class="sxs-lookup"><span data-stu-id="a082f-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![ムービー データが表示された、Chrome で開かれているムービー アプリケーション](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="a082f-108">生成されたコードの更新</span><span class="sxs-lookup"><span data-stu-id="a082f-108">Update the generated code</span></span>

<span data-ttu-id="a082f-109">*Models/Movie.cs* ファイルを開き、下のコードで強調表示されている行を追加します。</span><span class="sxs-lookup"><span data-stu-id="a082f-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="a082f-110">赤の波線を右クリックし、**[クイック アクションとリファクタリング]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a082f-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![コンテキスト メニューに **[クイック アクションとリファクタリング]** が表示されます。](da1/qa.png)

<span data-ttu-id="a082f-112">`using System.ComponentModel.DataAnnotations;` を選択します。</span><span class="sxs-lookup"><span data-stu-id="a082f-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![一覧の一番上にある System.ComponentModel.DataAnnotations を使用する](da1/da.png)

  <span data-ttu-id="a082f-114">Visual Studio により `using System.ComponentModel.DataAnnotations;` が追加されます。</span><span class="sxs-lookup"><span data-stu-id="a082f-114">Visual Studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="a082f-115">[前: SQL Server LocalDB の使用](xref:tutorials/razor-pages/sql)
> [次: 検索の追加](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="a082f-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>
