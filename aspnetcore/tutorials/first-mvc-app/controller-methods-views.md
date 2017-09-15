---
title: "コントローラーのメソッドとビュー"
author: rick-anderson
description: "コントローラー、ビュー、DataAnnotations の操作"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="5c6b3-104">コントローラーのメソッドとビュー</span><span class="sxs-lookup"><span data-stu-id="5c6b3-104">Controller methods and views</span></span>

<span data-ttu-id="5c6b3-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5c6b3-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5c6b3-106">ムービーアプリは上々の滑り出しでしたが、表示が理想的ではありません。</span><span class="sxs-lookup"><span data-stu-id="5c6b3-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="5c6b3-107">時刻の表示が好ましくありませんし (下の画像の 12:00:00 AM)、**ReleaseDate** は 2 つの単語にするべきです。</span><span class="sxs-lookup"><span data-stu-id="5c6b3-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![索引ビュー: Release Date が 1 語 (スペースなし) で、ムービーの公開日がすべて 12 AM になっています。](working-with-sql/_static/m55.png)

<span data-ttu-id="5c6b3-109">*Models/Movie.cs* ファイルを開き、下の画像で強調表示されている行を追加します。</span><span class="sxs-lookup"><span data-stu-id="5c6b3-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

<span data-ttu-id="5c6b3-110">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="5c6b3-110">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span></span>

<span data-ttu-id="5c6b3-111">赤の波線を右クリックし、**[クイック アクションとリファクタリング]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5c6b3-111">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![コンテキスト メニューに **[クイック アクションとリファクタリング]** が表示されます。](controller-methods-views/_static/qa.png)


<span data-ttu-id="5c6b3-113">`using System.ComponentModel.DataAnnotations;` をタップします。</span><span class="sxs-lookup"><span data-stu-id="5c6b3-113">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![一覧の一番上にある System.ComponentModel.DataAnnotations を使用する](controller-methods-views/_static/da.png)

  <span data-ttu-id="5c6b3-115">Visual Studio により `using System.ComponentModel.DataAnnotations;` が追加されます。</span><span class="sxs-lookup"><span data-stu-id="5c6b3-115">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="5c6b3-116">不要な `using` ステートメントを削除しましょう。</span><span class="sxs-lookup"><span data-stu-id="5c6b3-116">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="5c6b3-117">既定では、明るい灰色のフォントで表示されます。</span><span class="sxs-lookup"><span data-stu-id="5c6b3-117">They show up by default in a light grey font.</span></span> <span data-ttu-id="5c6b3-118">*Movie.cs* ファイル内をどこでもよいので右クリックし、**[using の削除と並べ替え]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="5c6b3-118">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![using の削除と並べ替え](controller-methods-views/_static/rm.png)

<span data-ttu-id="5c6b3-120">変更後のコード:</span><span class="sxs-lookup"><span data-stu-id="5c6b3-120">The updated code:</span></span>

<span data-ttu-id="5c6b3-121">[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="5c6b3-121">[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]</span></span>

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="5c6b3-122">[前へ](working-with-sql.md)
[次へ](search.md)</span><span class="sxs-lookup"><span data-stu-id="5c6b3-122">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
