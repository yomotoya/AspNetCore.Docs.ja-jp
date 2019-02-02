---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'チュートリアル: ASP.NET MVC アプリで EF Database First のデータベースを変更します。'
description: このチュートリアルでは、データベース構造に更新を行うと、web アプリケーション全体で変更を伝達に重点を置いています。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667636"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a>チュートリアル: ASP.NET MVC アプリで EF Database First のデータベースを変更します。

MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。

このチュートリアルでは、データベース構造に更新を行うと、web アプリケーション全体で変更を伝達に重点を置いています。

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * 列の追加
> * ビューにプロパティを追加します。

## <a name="prerequisites"></a>必須コンポーネント

* [ビューの生成](generating-views.md)

## <a name="add-a-column"></a>列の追加

データベースのテーブルの構造を更新する場合、データ モデル、ビュー、およびコント ローラーに、変更が反映されることを確認する必要があります。

このチュートリアルでは、生徒のミドル ネームを記録する Student テーブルに新しい列を追加します。 この列を追加するには、データベース プロジェクトを開き、Student.sql ファイルを開きます。 という名前の列を追加、デザイナーまたは T-SQL コードのどちらかを通じて**MiddleName** nvarchar (50) で NULL 値を許容します。

この変更をデータベース プロジェクト (または f5 キー) を起動して、ローカル データベースに配置します。 新しいフィールドは、テーブルに追加されます。 表示されない場合、SQL Server オブジェクト エクスプ ローラーで、ウィンドウで [更新] ボタンをクリックします。

![新しい列を表示します。](changing-the-database/_static/image2.png)

新しい列、データベース テーブルに存在しますが、データ モデル クラス内に現在存在しません。 新しい列に含めるモデルを更新する必要があります。 **モデル**フォルダーを開き、 **ContosoModel.edmx**モデル ダイアグラムを表示するファイル。 Student モデルに MiddleName プロパティが含まれていないことに注意してください。 デザイン画面で任意の場所を右クリックし、選択**データベースからモデルを更新**します。

更新ウィザードで選択、**更新**タブを選び**テーブル** > **dbo** > **学生**します。 **[完了]** をクリックします。

データベース ダイアグラムには、新しい更新プロセスが完了すると、 **MiddleName**プロパティ。 保存、 **ContosoModel.edmx**ファイル。 新しいプロパティに反映されるは、このファイルを保存する必要があります、 **Student.cs**クラス。 データベースとモデルを更新したようになりました。

ソリューションをビルドします。

## <a name="add-the-property-to-the-views"></a>ビューにプロパティを追加します。

残念ながら、ビューは引き続き、新しいプロパティを含んでいません。 ビューを更新するには、2 つのオプションがあります - もう一度、Student クラスのスキャフォールディングを追加することで、ビューを再生成するかまたは、既存のビューに新しいプロパティを手動で追加することができます。 このチュートリアルでは、スキャフォールディングを追加するもう一度自動的に生成されたビューをカスタマイズした変更を行っていないためです。 ビューを変更し、それらの変更が失われるしたくない場合、プロパティを手動で追加を検討する可能性があります。

ビューは再作成させるには、削除、**学生**の下のフォルダー**ビュー**、および削除、 **StudentsController**します。 右クリックし、**コント ローラー**フォルダーのスキャフォールディングを追加し、**学生**モデル。 コント ローラーの名前をもう一度、 **StudentsController**します。 **[追加]** を選びます。

ソリューションをもう一度ビルドします。 ビューには、MiddleName プロパティが含まれています。

![ミドル ネームを表示します。](changing-the-database/_static/image5.png)

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * 列の追加
> * ビューにプロパティを追加

学生のレコードの詳細を表示するためのビューをカスタマイズする方法については、次のチュートリアルに進んでください。
> [!div class="nextstepaction"]
> [ビューをカスタマイズします。](customizing-a-view.md)