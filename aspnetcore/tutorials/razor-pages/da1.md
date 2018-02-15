---
title: "生成されたページの更新"
author: rick-anderson
description: "生成されたページを更新して、表示をわかりやすくします。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a>生成されたページの更新

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ムービー アプリは上々の滑り出しでしたが、表示が理想的ではありません。 時刻の表示が好ましくなく (下の画像の 12:00:00 AM)、**ReleaseDate** は **Release Date** (2 つの単語) にするべきです。

![ムービー データが表示された、Chrome で開かれているムービー アプリケーション](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>生成されたコードの更新

*Models/Movie.cs* ファイルを開き、下のコードで強調表示されている行を追加します。

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

赤の波線を右クリックし、[クイック アクションとリファクタリング] を選択します。

  ![コンテキスト メニューに **[クイック アクションとリファクタリング]** が表示されます。](da1/qa.png)

`using System.ComponentModel.DataAnnotations;` を選択します。

  ![一覧の一番上にある System.ComponentModel.DataAnnotations を使用する](da1/da.png)

  Visual Studio により `using System.ComponentModel.DataAnnotations;` が追加されます。

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
[前: SQL Server LocalDB の使用](xref:tutorials/razor-pages/sql)
[検索の追加](xref:tutorials/razor-pages/search)
