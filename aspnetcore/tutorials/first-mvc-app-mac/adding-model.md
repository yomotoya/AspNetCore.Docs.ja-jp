---
title: "ASP.NET MVC Core アプリにモデルを追加する"
author: rick-anderson
description: "単純な ASP.NET Core アプリケーションにモデルを追加します。"
keywords: "ASP.NET Core, MVC, スキャフォールド, スキャフォールディング"
ms.author: riande
manager: wpickett
ms.devlang: csharp
ms.date: 09/22/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-eeee-1638-b903-b593059e9f39
ms.technology: aspnet
ms.prod: .net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 0e2c47c7d9903d6d300fa293dd0530a2f65f77d8
ms.sourcegitcommit: e6bcd56a4b11e20ff55df004971f9ed384937342
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* *Models* フォルダーを右クリックし、**[追加]** > **[新しいファイル]** の順に選択します。 
* **[新しいファイル]** ダイアログで次を実行します。

  * 左側のウィンドウで **[全般]** を選択します。
  * 中央ウィンドウで **[空のクラス]** を選択します。
  * クラスに **Movie** という名前を付け、**[新規]** を選択します。

`Movie` クラスに次のプロパティを追加します。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` フィールドは、データベースで主キー用に必要です。

プロジェクトをビルドして、エラーがないことを確認します。 これで、**M**VC アプリに、**モ**デルがあることになります。

## <a name="prepare-the-project-for-scaffolding"></a>スキャフォールディング用のプロジェクトの準備

- プロジェクト ファイルを右クリックし、**[ツール]、[ファイルの編集]** の順に選択します。

  ![前述の手順を参照](adding-model/_static/1.png)

- *MvcMovie.csproj* ファイルに次の強調表示されている NuGet パッケージを追加します。
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- ファイルを保存します。

- *Models/MvcMovieContext.cs* ファイルを作成して、次の `MvcMovieContext` クラスを追加します。[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- *Startup.cs* ファイルを開き、using を 2 つ追加します。[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- *Startup.cs* ファイルに、次のデータベース コンテキストを追加します。

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  これは、Entity Framework にデータ モデルにどのモデル クラスが含まれるか示します。 データベースに Movie テーブルとして反映される、Movie オブジェクトの*エンティティ セット*の 1 つを定義します。

- プロジェクトをビルドして、エラーがないことを確認します。

## <a name="scaffold-the-moviecontroller"></a>MovieController のスキャフォールディング

プロジェクト フォルダーでターミナル ウィンドウを開き、次のコマンドを実行します。

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
`No executable found matching command "dotnet-aspnet-codegenerator", verify`エラーが発生した場合は、次のようにします。

 * プロジェクト ディレクトリに移動します。 プロジェクト ディレクトリには、*Program.cs*、*Startup.cs* および *.csproj* ファイルがあります。
 * dotnet バージョンは 1.1 以上です。 `dotnet` を実行し、バージョンを取得します。
 * [MvcMovie.csproj ファイル](#prepare-the-project-for-scaffolding)に `<DotNetCliToolReference>` 要素が追加されました。
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

スキャフォールディング エンジンによって次が作成されます。

* ムービー コントローラー (*Controllers/MoviesController.cs*)
* 作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (*Views/Movies/\*.cshtml*)

[CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。 ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。

### <a name="add-the-files-to-visual-studio"></a>Visual Studio へのファイルの追加

* Visual Studio プロジェクトに、*MovieController.cs* ファイルを追加します。

  * *Controllers* フォルダーを右クリックし、**[追加]、[ファイルの追加]** の順に選択します。
  * *MovieController.cs* ファイルを選択します。

* *Movies* フォルダーおよびビューを追加します。

  * *Views* フォルダーを右クリックし、**[追加]、[既存のフォルダーの追加]** の順に選択します。
  * *Views* フォルダーに移動し、*Views\Movies* を選択し、**[開く]** を選択します。
  * **[Select files to add from Movies]\(Movies から追加するファイルを選択\)** ダイアログ ボックスで、**[Include All]\(すべて含める\)**、**[OK]** の順に選択します。

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

これで、データを表示、編集、更新および削除できるデータベースができました。 次のチュートリアルでは、そのデータベースを使用します。

## <a name="additional-resources"></a>その他の技術情報

* [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)
* [グローバライズとローカライズ](xref:fundamentals/localization)

>[!div class="step-by-step"]
[前のチュートリアル ビューの追加](adding-view.md)
[次のチュートリアル SQL の使用](working-with-sql.md)  
