---
title: ".ASP.NET Core MVC アプリへのモデルの追加。"
author: rick-anderson
description: "単純な ASP.NET Core アプリケーションにモデルを追加します。"
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
manager: wpickett
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 4c09225c925c326da7e815b39f176325a04fc17b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* *Movie.cs* という名前の *Models* フォルダーにクラスを追加します。
* *Models/Movie.cs* ファイルに次のコードを追加します。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` フィールドは、データベースで主キー用に必要です。 

エラーがないことを確認するためにアプリを構築すると、**M**VC アプリに **モ**デルが追加されます。

## <a name="prepare-the-project-for-scaffolding"></a>スキャフォールディング用のプロジェクトの準備

- *MvcMovie.csproj* ファイルに次の強調表示されている NuGet パッケージを追加します。
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- ファイルを保存し、[There are unresolved dependencies]/(未解決の依存関係があります/) という内容の**情報**メッセージに対し、**[復元]** を選択します。
- *Models/MvcMovieContext.cs* ファイルを作成して、次の `MvcMovieContext` クラスを追加します。

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- *Startup.cs* ファイルを開き、using を 2 つ追加します。

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- *Startup.cs* ファイルに、次のデータベース コンテキストを追加します。

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  これは、Entity Framework にデータ モデルにどのモデル クラスが含まれるか示します。 データベースに Movie テーブルとして反映される、Movie オブジェクトの*エンティティ セット*の 1 つを定義します。

- プロジェクトをビルドして、エラーがないことを確認します。

## <a name="scaffold-the-moviecontroller"></a>MovieController のスキャフォールディング

プロジェクト フォルダーでターミナル ウィンドウを開き、次のコマンドを実行します。

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> スキャフォールディング コマンドの実行時にエラーが表示される場合、解決策として[スキャフォールディング リポジトリの問題 444](https://github.com/aspnet/scaffolding/issues/444)を参照してください。

スキャフォールディング エンジンによって次が作成されます。

* ムービー コントローラー (*Controllers/MoviesController.cs*)
* 作成、削除、詳細、編集、およびインデックス ページ用の Razor ビュー ファイル (*Views/Movies/\*.cshtml*)

[CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (作成、読み取り、更新、および削除) アクション メソッドとビューの自動作成は、*スキャフォールディング*と言います。 ムービー データベースを管理できる、完全に機能する Web アプリケーションがすぐに完成します。

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

これで、データを表示、編集、更新および削除できるデータベースができました。 次のチュートリアルでは、そのデータベースを使用します。

### <a name="additional-resources"></a>その他の技術情報

* [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)
* [グローバライズとローカライズ](xref:fundamentals/localization)

>[!div class="step-by-step"]
[前のチュートリアル - ビューの追加](adding-view.md)
[次のチュートリアル - SQL Lite の使用](working-with-sql.md)
