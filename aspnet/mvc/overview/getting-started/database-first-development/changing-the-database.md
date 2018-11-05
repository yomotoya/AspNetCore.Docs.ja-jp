---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF Database First と ASP.NET MVC: データベースの変更 |Microsoft Docs'
author: Rick-Anderson
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: riande
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 8d0eff37ced757cd8be74f8171c9aaa430940010
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021275"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>EF Database First と ASP.NET MVC: データベースの変更
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。
> 
> シリーズのこの部分は、データベースの構造体への更新を行うと、web アプリケーション全体で変更を伝達について説明します。


## <a name="add-a-column"></a>列の追加

データベースのテーブルの構造を更新する場合、データ モデル、ビュー、およびコント ローラーに、変更が反映されることを確認する必要があります。

このチュートリアルでは、生徒のミドル ネームを記録する Student テーブルに新しい列を追加します。 この列を追加するには、データベース プロジェクトを開き、Student.sql ファイルを開きます。 という名前の列を追加、デザイナーまたは T-SQL コードのどちらかを通じて**MiddleName** nvarchar (50) で NULL 値を許容します。

![ミドル ネームを追加します。](changing-the-database/_static/image1.png)

この変更をデータベース プロジェクト (または f5 キー) を起動して、ローカル データベースに配置します。 新しいフィールドは、テーブルに追加されます。 表示されない場合、SQL Server オブジェクト エクスプ ローラーで、ウィンドウで [更新] ボタンをクリックします。

![新しい列を表示します。](changing-the-database/_static/image2.png)

新しい列、データベース テーブルに存在しますが、データ モデル クラス内に現在存在しません。 新しい列に含めるモデルを更新する必要があります。 **モデル**フォルダーを開き、 **ContosoModel.edmx**モデル ダイアグラムを表示するファイル。 Student モデルに MiddleName プロパティが含まれていないことに注意してください。 デザイン画面で任意の場所を右クリックし、選択**データベースからモデルを更新**します。

![モデルを更新します。](changing-the-database/_static/image3.png)

更新ウィザードで選択、**更新**タブおよび**学生**テーブル。

![更新ウィザード](changing-the-database/_static/image4.png)

**[完了]** をクリックします。

データベース ダイアグラムには、新しい更新プロセスが完了すると、 **MiddleName**プロパティ。 保存、 **ContosoModel.edmx**ファイル。 新しいプロパティに反映されるは、このファイルを保存する必要があります、 **Student.cs**クラス。 データベースとモデルを更新したようになりました。

ソリューションをビルドします。

残念ながら、ビューは引き続き、新しいプロパティを含んでいません。 ビューを更新するには、2 つのオプションがあります - もう一度、Student クラスのスキャフォールディングを追加することで、ビューを再生成するかまたは、既存のビューに新しいプロパティを手動で追加することができます。 このチュートリアルでは、スキャフォールディングを追加するもう一度自動的に生成されたビューをカスタマイズした変更を行っていないためです。 ビューを変更し、それらの変更が失われるしたくない場合、プロパティを手動で追加を検討する可能性があります。

ビューは再作成させるには、削除、**学生**の下のフォルダー**ビュー**、および削除、 **StudentsController**します。 右クリックし、**コント ローラー**フォルダーのスキャフォールディングを追加し、**学生**モデル。 コント ローラーの名前をもう一度、 **StudentsController**します。 **[OK]** を選択します。

ビューには、MiddleName プロパティが含まれています。

![ミドル ネームを表示します。](changing-the-database/_static/image5.png)

次のセクションでは、学生のレコードの詳細を表示するためのビューをカスタマイズするコードを追加します。

> [!div class="step-by-step"]
> [前へ](generating-views.md)
> [次へ](customizing-a-view.md)
