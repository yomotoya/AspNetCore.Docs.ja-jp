---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF Database First と ASP.NET MVC: ビューの生成 |Microsoft Docs'
author: tfitzmac
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 58f8be181f80f5b27353e9ef5cafe48bcb370e29
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399611"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>EF Database First と ASP.NET MVC: ビューを生成します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。
> 
> シリーズのこの部分は、ASP.NET スキャフォールディングを使用して、コント ローラーとビューを生成するについて説明します。


## <a name="add-scaffold"></a>スキャフォールディングを追加します。

モデル クラスの標準的なデータ操作を提供するコードを生成する準備が整いました。 コードを追加するには、スキャフォールディングの項目を追加します。 型のスキャフォールディングを追加することができます。 多くのオプションがあります。このチュートリアルでスキャフォールディングをコント ローラーと、前のセクションで作成した受講者と登録のモデルに対応するビューが含まれます。

プロジェクトでの一貫性を維持するには、既存に、新しいコント ローラーを追加**コント ローラー**フォルダー。 右クリックし、**コント ローラー**フォルダー、および選択**追加**–**スキャフォールディングされた新しい項目**します。

![スキャフォールディングを追加します。](generating-views/_static/image1.png)

選択、 **MVC 5 コント ローラーとビュー、Entity Framework を使用して**オプション。 このオプションは、コント ローラーとビューの更新、削除、作成、およびモデルのデータを表示するに生成されます。

![mvc コント ローラーを追加します。](generating-views/_static/image2.png)

選択**学生**を選択して、モデルのクラス、 **ContosoUniversityEntities**コンテキスト クラス。 としてコント ローラー名をそのまま**StudentsController**、

![コント ローラーを指定します。](generating-views/_static/image3.png)

**[追加]** をクリックします。

エラーが発生した場合は、前のセクションで、プロジェクトをビルドしていない可能性があります。 そうである場合、プロジェクトのビルドを再試行してくださいし、スキャフォールディングされた項目を再度追加します。

コード生成プロセスが完了したら、新しいコント ローラーとビューをプロジェクトに表示されます。

![ビューを表示します。](generating-views/_static/image4.png)

ここでも、同じ手順を実行しますが、登録クラスのスキャフォールディングを追加します。 完了したらが必要、 **EnrollmentsController.cs**ファイル、および下にあるフォルダー**ビュー**という名前**登録**で作成、削除、詳細、編集、およびインデックス表示モード。

![ビューを表示します。](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>新しいビューにリンクを追加します。

受講者と登録のやすく、新しいビューに移動するためのインデックス ビューに、いくつかのハイパーリンクを追加できます。 ファイルを開きます**Views/Home/Index.cshtml**、これは、サイトのホーム ページ。 Jumbotron は、次のコードを追加します。

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

ActionLink 方法の場合は、最初のパラメーターは、リンクに表示するテキストです。 2 番目のパラメーターは、アクションと 3 番目のパラメーターは、コント ローラーの名前です。 たとえば、最初のリンクは StudentsController でインデックスの操作を指します。 実際のハイパーリンクは、これらの値から構成されます。 最初のリンクはユーザーが最終的には、 **Index.cshtml**ファイル内で、**ビュー/学生**フォルダー。

## <a name="display-student-views"></a>学生のビューを表示します。

プロジェクトに正しく追加コードが、受講者の一覧を表示し、編集、作成、またはデータベース内の生徒レコードを削除することができますを確認します。

右クリックし、 **Views/Home/Index.cshtml**ファイル、および選択**ブラウザーで表示**します。 このページで、学生の一覧については、リンクをクリックします。

![](generating-views/_static/image6.png)

このページでは、このデータを変更するには、受講者とのリンクの一覧に注目してください。

![受講者の一覧](generating-views/_static/image7.png)

をクリックして、**新規作成**リンクし、新しい学生のいくつかの値を提供します。

![新しい学生を作成します。](generating-views/_static/image8.png)

クリックして**作成**、し、新しい学生が一覧に追加されます。

![新しい学生の一覧](generating-views/_static/image9.png)

選択、**編集**リンク、および学生の値の一部を変更します。

![学生を編集します。](generating-views/_static/image10.png)

をクリックして**保存**、および学生のレコードが変更されたことを確認します。

最後に、選択、**削除**リンクし、クリックして、レコードを削除することを確認、**削除**ボタンをクリックします。

![学生を削除します。](generating-views/_static/image11.png)

すべてのコードを記述することがなく、Student テーブルで、データに対する一般的な操作を実行するビューが追加されました。

お気付きかもしれませんが、フィールドのテキスト ラベルをデータベースのプロパティに基づくこと (など**LastName**) されるとは限りません web ページに表示します。 たとえばのラベルをしたい場合があります**姓**します。 チュートリアルの後半では、この表示の問題を修正します。

## <a name="display-enrollment-views"></a>登録のビューを表示します。

データベースには、学生と登録のテーブルとコースと登録のテーブル間の一対多リレーションシップの間の一対多リレーションシップが含まれています。 登録のためのビューは、これらのリレーションシップを正しく処理します。 サイトと選択のホーム ページに移動し、**登録の一覧**リンクをクリックし、**新規作成**リンク。 ビューには、新しい登録レコードを作成するためのフォームが表示されます。 具体的には、フォームに関連するテーブルからの値で設定された 2 つのドロップダウン リストが含まれていることを確認します。

![登録を作成します。](generating-views/_static/image12.png)

さらに、指定された値の検証が自動的に適用に基づいてフィールドのデータ型にします。 グレードでは、互換性のない値を指定しようとする場合、エラー メッセージが表示されるように、番号が必要です。

![検証メッセージ](generating-views/_static/image13.png)

自動的に生成されたビューが、データベース内のデータを使用するユーザーを有効にすることを確認します。 このシリーズの次のチュートリアルでは、データベースを更新し、web アプリケーションに対応する変更を加えます。

> [!div class="step-by-step"]
> [前へ](creating-the-web-application.md)
> [次へ](changing-the-database.md)
