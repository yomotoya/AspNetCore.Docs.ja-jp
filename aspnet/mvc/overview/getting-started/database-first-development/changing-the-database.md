---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'ASP.NET MVC で最初の EF Database: データベースの変更 |Microsoft ドキュメント'
author: tfitzmac
description: MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 63ee8768a43dbdac80922e3adbedd3378c10da73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>ASP.NET MVC で最初の EF Database: データベースの変更
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブルの列に対応します。
> 
> 系列のこの部分は、データベース構造への更新を行うと、web アプリケーション全体で変更を伝達について説明します。


## <a name="add-a-column"></a>列の追加

データベースのテーブルの構造を更新する場合、変更内容をデータ モデル、ビュー、およびコント ローラーに反映されることを確認する必要があります。

このチュートリアルでは、学生のミドル ネームを記録する、受講者テーブルに新しい列を追加します。 この列を追加するには、データベース プロジェクトを開き Student.sql ファイルを開きます。 デザイナーまたは T-SQL コードのどちらかを通じてという名前の列を追加**MiddleName**を nvarchar (50) は、NULL 値を指定できます。

![ミドル ネームを追加します。](changing-the-database/_static/image1.png)

この変更をデータベース プロジェクト (または f5 キー) を起動して、ローカル データベースに配置します。 新しいフィールドは、テーブルに追加されます。 表示されないこと、SQL Server オブジェクト エクスプ ローラーで、ウィンドウで [更新] ボタンをクリックします。

![新しい列を表示します。](changing-the-database/_static/image2.png)

データベース テーブルに新しい列が存在しますが、データ モデル クラス内に現在存在しません。 新しい列に含めるモデルを更新する必要があります。 **モデル**フォルダーを開き、 **ContosoModel.edmx**モデル ダイアグラムを表示するファイル。 学生のモデルに MiddleName プロパティが含まれていないことに注意してください。 デザイン サーフェイス上の任意の場所を右クリックし、選択**データベースからモデルを更新**です。

![モデルを更新します](changing-the-database/_static/image3.png)

更新ウィザードで選択、**更新** タブおよび**学生**テーブル。

![更新ウィザード](changing-the-database/_static/image4.png)

**[完了]**をクリックします。

データベース ダイアグラムには、新しい更新プロセスが完了したら、 **MiddleName**プロパティです。 保存、 **ContosoModel.edmx**ファイル。 新しいプロパティに反映されるまでのこのファイルを保存する必要があります、 **Student.cs**クラスです。 データベースとモデルを更新したようになりました。

ソリューションをビルドします。

残念ながら、ビューはまだ、新しいプロパティを含んでいません。 ビューを更新するには、2 つのオプション - ことができますか、再生成するビューには、Student クラスのスキャフォールディングをもう一度追加しても、既存のビューに新しいプロパティを手動で追加することができます。 このチュートリアルでは、スキャフォールディングを追加します再を自動的に生成されたビューにカスタマイズされた変更が加えられるいないためです。 ビューを変更し、それらの変更を破棄したくない場合に手動でプロパティを追加することを検討する可能性があります。

ビューが再作成されたためには、削除、**受講者**下にあるフォルダー**ビュー**、および削除、 **StudentsController**です。 右クリックし、**コント ローラー**フォルダーのスキャフォールディングを追加し、**学生**モデル。 コント ローラーの名前をもう一度、 **StudentsController**です。 **[OK]** を選択します。

ビューには、MiddleName プロパティが含まれています。

![ミドル ネームを表示します。](changing-the-database/_static/image5.png)

次のセクションでは、student レコードに関する詳細を表示するビューをカスタマイズするためのコードを追加します。

> [!div class="step-by-step"]
> [前へ](generating-views.md)
> [次へ](customizing-a-view.md)
