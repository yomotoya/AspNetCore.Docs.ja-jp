---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'ASP.NET MVC で最初の EF データベース: ビューをカスタマイズする |Microsoft ドキュメント'
author: tfitzmac
description: MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 8338603e032329ad03d47c6392e508aa07c6858e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867659"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>ASP.NET MVC で最初の EF データベース: ビューをカスタマイズします。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブルの列に対応します。
> 
> 系列のこの部分は、プレゼンテーションを強化するために自動的に生成されたビューの変更について説明します。


## <a name="add-enrolled-courses-to-student-details"></a>登録済みのコースを受講者の詳細に追加します。

生成されたコードでは、アプリケーションの適切な開始点が、必ずしも行われません、アプリケーションで必要な機能の一部。 アプリケーションの特定の要件を満たすためにコードをカスタマイズすることができます。 現時点では、アプリケーションは、選択したスチューデントの登録済みのコースを表示されません。 ここでは、各受講者の登録済みのコースを追加、**詳細**受講者を表示します。

開いている**Students/Details.cshtml**、および、最後の下&lt;/dl&gt;  タブが終了する前に&lt;/div&gt;タグは、次のコードを追加します。

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

このコードは、選択した受講者に、Enrollment テーブルの各レコードの行を表示するテーブルを作成します。 **表示**メソッドは、式を表すオブジェクト (modelItem) の HTML を表示します。 表示方法を使用する (単にプロパティの値への埋め込みコード) ではなく、値は、型とその種類のテンプレートに基づく正しく書式設定を確認します。 この例では各式は、ループの現在のレコードを 1 つのプロパティを返し、値は、プリミティブ型をテキストとして表示されます。

もう一度、受講者/インデックス ビューを参照して選択**詳細**受講者のいずれか。 登録済みのコースが含まれています、ビューが表示されます。

![学生の登録を](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [前へ](changing-the-database.md)
> [次へ](enhancing-data-validation.md)
