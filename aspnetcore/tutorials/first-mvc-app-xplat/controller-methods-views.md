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
ms.openlocfilehash: b72fb5077d3bd577cd6282e7a6231a0806ee1fd2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views"></a>コントローラーのメソッドとビュー

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ムービーアプリは上々の滑り出しでしたが、表示が理想的ではありません。 時刻の表示が好ましくありませんし (下の画像の 12:00:00 AM)、**ReleaseDate** は 2 つの単語にするべきです。

![索引ビュー: Release Date が 1 語 (スペースなし) で、ムービーの公開日がすべて 12 AM になっています。](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

*Models/Movie.cs* ファイルを開き、下の画像で強調表示されている行を追加します。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

アプリケーションをビルドし、実行します。

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[前 - SQLite の操作](working-with-sql.md)
[次 - 検索の追加](search.md)  
