---
title: "コントローラーのメソッドとビュー"
author: rick-anderson
description: "コントローラー、ビュー、DataAnnotations の操作"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-bb71-4adf-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 87516295c6e8c49b492312afda80c92c968e778b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="3fc25-104">コントローラーのメソッドとビュー</span><span class="sxs-lookup"><span data-stu-id="3fc25-104">Controller methods and views</span></span>

<span data-ttu-id="3fc25-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3fc25-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3fc25-106">ムービーアプリは上々の滑り出しでしたが、表示が理想的ではありません。</span><span class="sxs-lookup"><span data-stu-id="3fc25-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="3fc25-107">時刻の表示が好ましくありませんし (下の画像の 12:00:00 AM)、**ReleaseDate** は 2 つの単語にするべきです。</span><span class="sxs-lookup"><span data-stu-id="3fc25-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![索引ビュー: Release Date が 1 語 (スペースなし) で、ムービーの公開日がすべて 12 AM になっています。](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="3fc25-109">*Models/Movie.cs* ファイルを開き、下の画像で強調表示されている行を追加します。</span><span class="sxs-lookup"><span data-stu-id="3fc25-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

<span data-ttu-id="3fc25-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]</span><span class="sxs-lookup"><span data-stu-id="3fc25-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]</span></span>

<span data-ttu-id="3fc25-111">アプリケーションをビルドし、実行します。</span><span class="sxs-lookup"><span data-stu-id="3fc25-111">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="3fc25-112">[前 - SQLite の操作](working-with-sql.md)
[次 - 検索の追加](search.md)</span><span class="sxs-lookup"><span data-stu-id="3fc25-112">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  