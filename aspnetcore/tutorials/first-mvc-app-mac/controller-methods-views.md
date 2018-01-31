---
title: "ASP.NET Core MVC アプリのコントローラーのメソッドとビュー"
author: rick-anderson
description: "コントローラー、ビュー、DataAnnotations の操作"
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 01c20e505bd9d1591e1921701f94d102822231c8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="175a5-103">ASP.NET Core MVC アプリのコントローラーのメソッドとビュー</span><span class="sxs-lookup"><span data-stu-id="175a5-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="175a5-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="175a5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="175a5-105">ムービー アプリは上々の滑り出しでしたが、表示が理想的ではありません。</span><span class="sxs-lookup"><span data-stu-id="175a5-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="175a5-106">時刻の表示が好ましくないし (下の画像の 12:00:00 AM)、**ReleaseDate** は 2 つの単語にするべきです。</span><span class="sxs-lookup"><span data-stu-id="175a5-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![索引ビュー: Release Date が 1 語 (スペースなし) で、ムービーの公開日がすべて 12 AM になっています。](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="175a5-108">*Models/Movie.cs* ファイルを開き、下の画像で強調表示されている行を追加します。</span><span class="sxs-lookup"><span data-stu-id="175a5-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="175a5-109">アプリケーションをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="175a5-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="175a5-110">[前 - SQLite の操作](working-with-sql.md)
[次 - 検索の追加](search.md)</span><span class="sxs-lookup"><span data-stu-id="175a5-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
