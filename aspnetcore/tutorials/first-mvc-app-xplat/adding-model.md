---
title: ".ASP.NET Core MVC アプリへのモデルの追加。"
author: rick-anderson
description: "単純な ASP.NET Core アプリケーションにモデルを追加します。"
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 81511b05a3cc11a58b93452d3c6e5305e7ee4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
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
