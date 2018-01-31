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
# <a name="controller-methods-and-views"></a>コントローラーのメソッドとビュー

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ムービー アプリは上々の滑り出しでしたが、表示が理想的ではありません。 時刻の表示が好ましくありませんし (下の画像の 12:00:00 AM)、**ReleaseDate** は 2 つの単語にするべきです。

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
