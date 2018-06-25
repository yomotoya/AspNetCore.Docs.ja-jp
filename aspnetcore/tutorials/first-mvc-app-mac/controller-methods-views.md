---
title: ASP.NET Core MVC アプリのコントローラーのメソッドとビュー
author: rick-anderson
description: コントローラー、ビュー、DataAnnotations の操作
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 0bd57597c74918829de38764326e35df08ab1e05
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279078"
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a>ASP.NET Core MVC アプリのコントローラーのメソッドとビュー

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ムービー アプリは上々の滑り出しでしたが、表示が理想的ではありません。 時刻の表示が好ましくないし (下の画像の 12:00:00 AM)、**ReleaseDate** は 2 つの単語にするべきです。

![索引ビュー: Release Date が 1 語 (スペースなし) で、ムービーの公開日がすべて 12 AM になっています。](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

*Models/Movie.cs* ファイルを開き、下の画像で強調表示されている行を追加します。

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

アプリケーションをビルドし、実行します。

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [前 - SQLite の操作](working-with-sql.md)
> [次 - 検索の追加](search.md)
