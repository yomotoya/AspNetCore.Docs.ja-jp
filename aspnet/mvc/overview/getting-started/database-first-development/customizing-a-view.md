---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'チュートリアル: ASP.NET MVC アプリで EF Database First のビューをカスタマイズします。'
description: このチュートリアルでは、プレゼンテーションを強化するために自動的に生成されたビューの変更について説明します。
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236498"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>チュートリアル: ASP.NET MVC アプリで EF Database First のビューをカスタマイズします。

MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。

このチュートリアルでは、プレゼンテーションを強化するために自動的に生成されたビューの変更について説明します。

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * コースを受講者の詳細ページに追加します。
> * コースがページに追加されていることを確認します。

## <a name="prerequisites"></a>必須コンポーネント

* [データベースを変更します。](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>コースを受講者の詳細に追加します。

生成されたコードにより、アプリケーションの出発点として適していますが、こと必ずしもはできませんのすべてのアプリケーションで必要な機能。 アプリケーションの特定の要件を満たすためにコードをカスタマイズすることができます。 現時点では、アプリケーションは、選択した学生の登録済みのコースを表示されません。 このセクションには、各学生の登録済みのコースを追加します、**詳細**受講者を表示します。

開いている**ビュー** > **学生** > *Details.cshtml*します。 最後の下&lt;/dl&gt;タグが終了する前に&lt;/div&gt;タグは、次のコードを追加します。

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

このコードでは、選択した受講者で、Enrollment テーブルの各レコードの行を表示するテーブルを作成します。 **表示**メソッドは、式を表すオブジェクト (modelItem) の HTML をレンダリングします。 Display メソッドを使用する (コードでプロパティ値を単純に埋め込む) ではなく、値はその型と、その型のテンプレートに基づいて正しく書式設定を確認します。 この例では、各式はループでは、現在のレコードを 1 つのプロパティを返し、値は、プリミティブ型をテキストとして表示されます。

## <a name="confirm-courses-are-added"></a>コースが追加されたことを確認します。

ソリューションを実行します。 をクリックして**受講者の一覧**選択と**詳細**受講者の 1 つ。 表示されますが、ビューには登録済みのコースが含まれています。

![登録と学生](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>次の手順
このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * 学生の詳細ページに追加されたコース
> * コースがページに追加されたことを確認します。

検証の要件を指定し、書式設定を表示するデータ注釈を追加する方法については、次のチュートリアルに進んでください。
> [!div class="nextstepaction"]
> [データ検証を強化します。](enhancing-data-validation.md)
