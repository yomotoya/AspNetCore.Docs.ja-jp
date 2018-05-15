---
title: ASP.NET Core のコントローラーのメソッドとビュー
author: rick-anderson
description: ASP.NET Core でコントローラー メソッド、ビュー、DataAnnotations を使用する方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 0bf9bffbf14ff958b28d9494600f55eb3f8e0c35
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="89824-103">ASP.NET Core のコントローラーのメソッドとビュー</span><span class="sxs-lookup"><span data-stu-id="89824-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="89824-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="89824-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="89824-105">ムービー アプリは上々の滑り出しでしたが、表示が理想的ではありません。</span><span class="sxs-lookup"><span data-stu-id="89824-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="89824-106">時刻の表示が好ましくありませんし (下の画像の 12:00:00 AM)、**ReleaseDate** は 2 つの単語にするべきです。</span><span class="sxs-lookup"><span data-stu-id="89824-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![索引ビュー: Release Date が 1 語 (スペースなし) で、ムービーの公開日がすべて 12 AM になっています。](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="89824-108">*Models/Movie.cs* ファイルを開き、下の画像で強調表示されている行を追加します。</span><span class="sxs-lookup"><span data-stu-id="89824-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="89824-109">アプリケーションをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="89824-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="89824-110">[前 - SQLite の操作](working-with-sql.md)
> [次 - 検索の追加](search.md)</span><span class="sxs-lookup"><span data-stu-id="89824-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
