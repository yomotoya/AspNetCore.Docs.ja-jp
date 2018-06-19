---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'ASP.NET MVC で最初の EF データベース: ビューを生成する |Microsoft ドキュメント'
author: tfitzmac
description: MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: b60e89a187a879255eb051dc87241714cef6fa63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879752"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>ASP.NET MVC で最初の EF データベース: ビューを生成します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブルの列に対応します。
> 
> 系列のこの部分は、ASP.NET のスキャフォールディングを使用して、コント ローラーとビューを生成するについて説明します。


## <a name="add-scaffold"></a>スキャフォールディングを追加します。

標準的なデータ操作モデル クラスを提供するコードを生成する準備ができたらです。 コードを追加するには、scaffold 項目を追加します。 追加することができます。 スキャフォールディングの型の多くのオプションがあります。このチュートリアルでは、コント ローラーと前のセクションで作成した Student と登録のモデルに対応するビューには、スキャフォールディングが含まれます。

プロジェクト内の一貫性を維持するために、新しいコント ローラーを追加、既存**コント ローラー**フォルダーです。 右クリックし、**コント ローラー**フォルダー、および選択**追加**–**スキャフォールディングされた新しい項目**です。

![スキャフォールディングを追加します。](generating-views/_static/image1.png)

選択、 **MVC 5 コント ローラー、ビューがある Entity Framework を使用して**オプション。 このオプションは、コント ローラーとビューの更新、削除、作成、およびモデルにデータを表示するに生成されます。

![mvc コント ローラーを追加します。](generating-views/_static/image2.png)

選択**学生**を選択して、モデル クラス、 **ContosoUniversityEntities**コンテキスト クラスです。 としてコント ローラーの名前をそのまま**StudentsController**、

![コント ローラーを指定します。](generating-views/_static/image3.png)

**[追加]** をクリックします。

エラーが発生した場合は、前のセクションで、プロジェクトをビルドしていない可能性があります。 場合は、プロジェクトをビルドして再度スキャフォールディングの項目を追加ください。

コードの生成処理が完了したら、新しいコント ローラーとビューをプロジェクトに表示されます。

![ビューを表示します。](generating-views/_static/image4.png)

同じ手順をもう一度、実行しますが、登録クラス、スキャフォールディングを追加します。 完了したらが必要、 **EnrollmentsController.cs**ファイル、および下にあるフォルダー**ビュー**という名前**登録**の作成、削除、詳細、編集とインデックス表示モード。

![ビューを表示します。](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>新しいビューへのリンクを追加します。

やすく、新しいビューに移動するための受講者との登録、インデックス ビューに、いくつかのハイパーリンクを追加できます。 ファイルを開く**Views/Home/Index.cshtml**、これは、サイトのホーム ページです。 Jumbotron、次のコードを追加します。

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

ActionLink メソッドの最初のパラメーターは、リンクに表示するテキストです。 2 番目のパラメーターはアクションで、3 番目のパラメーターは、コント ローラーの名前です。 たとえば、最初のリンクは、StudentsController でインデックスの操作を指します。 実際のハイパーリンクは、これらの値から構成されます。 最初のリンクが最終的にユーザーを受け取り、 **Index.cshtml**ファイルの場所、**ビュー/受講者**フォルダーです。

## <a name="display-student-views"></a>学生のビューを表示します。

プロジェクトに正しく追加コードが、生徒数の一覧を表示し、編集、作成、またはデータベースの学生レコードを削除することができますを確認します。

右クリックし、 **Views/Home/Index.cshtml**ファイル、および選択した**ブラウザーで表示**です。 このページで、学生のリストへのリンクをクリックします。

![](generating-views/_static/image6.png)

このページで、このデータを変更するには、受講者とのリンクの一覧を確認します。

![学生のリスト](generating-views/_static/image7.png)

クリックして、**新規作成**リンクし、新しい学生のいくつかの値を提供します。

![新しい学生を作成します。](generating-views/_static/image8.png)

をクリックして**作成**、し、リストに新しい学生が追加されたことを確認します。

![新しい学生を使用してリスト](generating-views/_static/image9.png)

選択、**編集**リンク、および、受講者の値の一部を変更します。

![学生を編集します。](generating-views/_static/image10.png)

をクリックして**保存**、し学生のレコードが変更されたことを確認します。

最後に、選択、**削除**リンクしをクリックして、レコードを削除することを確認、**削除**ボタンをクリックします。

![学生を削除します。](generating-views/_static/image11.png)

すべてのコードを記述することがなく、Student テーブルにデータの一般的な操作を実行するビューを追加しました。

お気付きフィールドのテキスト ラベルがデータベースのプロパティに基づくこと (など**LastName**) が指定されて必ずしも web ページに表示します。 たとえば、ことをお勧めするラベル**姓**です。 このチュートリアルで後ほど、この表示の問題を修正します。

## <a name="display-enrollment-views"></a>登録のビューを表示します。

データベースには、Student と登録との間でコースと登録のテーブル間の一対多リレーションシップの一対多リレーションシップが含まれています。 登録用のビューは、これらのリレーションシップを正しく処理されます。 サイトと選択のホーム ページに移動、**まで登録数の一覧**リンクし、**新規作成**リンクします。 ビューには、新しい登録レコードを作成するためのフォームが表示されます。 具体的には、フォームに関連するテーブルからの値のみが含まれている 2 つのドロップダウン リストが含まれていることを確認します。

![登録を作成します。](generating-views/_static/image12.png)

さらに、指定された値の検証が自動的にに基づいて適用、フィールドのデータ型。 グレードでは、互換性のない値を指定しようとする場合、エラー メッセージが表示されるように、番号が必要です。

![検証メッセージ](generating-views/_static/image13.png)

自動的に生成されたビューが、データベース内のデータをユーザーが操作を有効にすることを確認します。 このシリーズの次のチュートリアルでは、データベースを更新し、web アプリケーションで対応する変更を行います。

> [!div class="step-by-step"]
> [前へ](creating-the-web-application.md)
> [次へ](changing-the-database.md)
