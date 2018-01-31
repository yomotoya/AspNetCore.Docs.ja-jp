---
title: "コントローラーのメソッドとビュー"
author: rick-anderson
description: "コントローラー、ビュー、DataAnnotations の操作"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 200f02f9815966653b3b46918737c60d11f11d5a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="f1aef-103">コントローラーのメソッドとビュー</span><span class="sxs-lookup"><span data-stu-id="f1aef-103">Controller methods and views</span></span>

<span data-ttu-id="f1aef-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1aef-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f1aef-105">ムービー アプリは上々の滑り出しでしたが、表示が理想的ではありません。</span><span class="sxs-lookup"><span data-stu-id="f1aef-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="f1aef-106">時刻の表示が好ましくありませんし (下の画像の 12:00:00 AM)、**ReleaseDate** は 2 つの単語にするべきです。</span><span class="sxs-lookup"><span data-stu-id="f1aef-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![索引ビュー: Release Date が 1 語 (スペースなし) で、ムービーの公開日がすべて 12 AM になっています。](working-with-sql/_static/m55.png)

<span data-ttu-id="f1aef-108">*Models/Movie.cs* ファイルを開き、下の画像で強調表示されている行を追加します。</span><span class="sxs-lookup"><span data-stu-id="f1aef-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="f1aef-109">赤の波線を右クリックし、**[クイック アクションとリファクタリング]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f1aef-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![コンテキスト メニューに **[クイック アクションとリファクタリング]** が表示されます。](controller-methods-views/_static/qa.png)


<span data-ttu-id="f1aef-111">`using System.ComponentModel.DataAnnotations;` をタップします。</span><span class="sxs-lookup"><span data-stu-id="f1aef-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![一覧の一番上にある System.ComponentModel.DataAnnotations を使用する](controller-methods-views/_static/da.png)

  <span data-ttu-id="f1aef-113">Visual Studio により `using System.ComponentModel.DataAnnotations;` が追加されます。</span><span class="sxs-lookup"><span data-stu-id="f1aef-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="f1aef-114">不要な `using` ステートメントを削除しましょう。</span><span class="sxs-lookup"><span data-stu-id="f1aef-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="f1aef-115">既定では、明るい灰色のフォントで表示されます。</span><span class="sxs-lookup"><span data-stu-id="f1aef-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="f1aef-116">*Movie.cs* ファイル内をどこでもよいので右クリックし、**[using の削除と並べ替え]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f1aef-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![using の削除と並べ替え](controller-methods-views/_static/rm.png)

<span data-ttu-id="f1aef-118">変更後のコード:</span><span class="sxs-lookup"><span data-stu-id="f1aef-118">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="f1aef-119">[前へ](working-with-sql.md)
[次へ](search.md)</span><span class="sxs-lookup"><span data-stu-id="f1aef-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
