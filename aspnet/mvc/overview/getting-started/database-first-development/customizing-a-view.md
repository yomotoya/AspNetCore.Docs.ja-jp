---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Database First と ASP.NET MVC: ビューのカスタマイズ |Microsoft Docs'
author: tfitzmac
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: ce450af93459f2a69557b3fe0d1ead813ae99986
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826801"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>EF Database First と ASP.NET MVC: ビューのカスタマイズ
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。
> 
> シリーズのこの部分は、プレゼンテーションを強化するために自動的に生成されたビューの変更について説明します。


## <a name="add-enrolled-courses-to-student-details"></a>登録済みのコースを受講者の詳細に追加します。

生成されたコードにより、アプリケーションの出発点として適していますが、こと必ずしもはできませんのすべてのアプリケーションで必要な機能。 アプリケーションの特定の要件を満たすためにコードをカスタマイズすることができます。 現時点では、アプリケーションは、選択した学生の登録済みのコースを表示されません。 このセクションには、各学生の登録済みのコースを追加します、**詳細**受講者を表示します。

開いている**Students/Details.cshtml**、および最後の下&lt;/dl&gt;  タブが終了する前に&lt;/div&gt;タグは、次のコードを追加します。

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

このコードでは、選択した受講者で、Enrollment テーブルの各レコードの行を表示するテーブルを作成します。 **表示**メソッドは、式を表すオブジェクト (modelItem) の HTML をレンダリングします。 Display メソッドを使用する (コードでプロパティ値を単純に埋め込む) ではなく、値はその型と、その型のテンプレートに基づいて正しく書式設定を確認します。 この例では、各式はループでは、現在のレコードを 1 つのプロパティを返し、値は、プリミティブ型をテキストとして表示されます。

もう一度/Students インデックス ビューを参照して選択**詳細**受講者の 1 つ。 表示されますが、ビューには登録済みのコースが含まれています。

![登録と学生](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [前へ](changing-the-database.md)
> [次へ](enhancing-data-validation.md)
