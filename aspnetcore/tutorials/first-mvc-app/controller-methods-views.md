---
title: ASP.NET Core のコントローラーのメソッドとビュー
author: rick-anderson
description: ASP.NET Core でコントローラー メソッド、ビュー、DataAnnotations を使用する方法について説明します。
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 42a63044cd14873ff334a728c6c8304214ee8575
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011340"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="77e31-103">ASP.NET Core のコントローラーのメソッドとビュー</span><span class="sxs-lookup"><span data-stu-id="77e31-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="77e31-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="77e31-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="77e31-105">ムービー アプリは上々の滑り出しでしたが、表示が理想的ではありません。</span><span class="sxs-lookup"><span data-stu-id="77e31-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="77e31-106">時刻の表示が好ましくありませんし (下の画像の 12:00:00 AM)、**ReleaseDate** は 2 つの単語にするべきです。</span><span class="sxs-lookup"><span data-stu-id="77e31-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![索引ビュー: Release Date が 1 語 (スペースなし) で、ムービーの公開日がすべて 12 AM になっています。](working-with-sql/_static/m55.png)

<span data-ttu-id="77e31-108">*Models/Movie.cs* ファイルを開き、下の画像で強調表示されている行を追加します。</span><span class="sxs-lookup"><span data-stu-id="77e31-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="77e31-109">[前へ](working-with-sql.md)
> [次へ](search.md)</span><span class="sxs-lookup"><span data-stu-id="77e31-109">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
