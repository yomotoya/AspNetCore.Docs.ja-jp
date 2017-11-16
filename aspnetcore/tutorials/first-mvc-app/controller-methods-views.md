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
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views"></a>コントローラーのメソッドとビュー

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ムービーアプリは上々の滑り出しでしたが、表示が理想的ではありません。 時刻の表示が好ましくありませんし (下の画像の 12:00:00 AM)、**ReleaseDate** は 2 つの単語にするべきです。

![索引ビュー: Release Date が 1 語 (スペースなし) で、ムービーの公開日がすべて 12 AM になっています。](working-with-sql/_static/m55.png)

*Models/Movie.cs* ファイルを開き、下の画像で強調表示されている行を追加します。

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

赤の波線を右クリックし、**[クイック アクションとリファクタリング]** を選択します。

  ![コンテキスト メニューに **[クイック アクションとリファクタリング]** が表示されます。](controller-methods-views/_static/qa.png)


`using System.ComponentModel.DataAnnotations;` をタップします。

  ![一覧の一番上にある System.ComponentModel.DataAnnotations を使用する](controller-methods-views/_static/da.png)

  Visual Studio により `using System.ComponentModel.DataAnnotations;` が追加されます。

不要な `using` ステートメントを削除しましょう。 既定では、明るい灰色のフォントで表示されます。 *Movie.cs* ファイル内をどこでもよいので右クリックし、**[using の削除と並べ替え]** を選択します。

![using の削除と並べ替え](controller-methods-views/_static/rm.png)

変更後のコード:

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[前へ](working-with-sql.md)
[次へ](search.md)  
