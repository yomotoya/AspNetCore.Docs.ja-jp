---
title: ASP.NET Core と MongoDB で Web API を構築する
author: prkhandelwal
description: このチュートリアルは、MongoDB NoSQL データベースを使用して ASP.NET Core Web API を構築する方法を説明します。
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 01/31/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 5e146261fdc8354fc9f4295a8af317e5cc36332f
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667337"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>ASP.NET Core と MongoDB で Web API を作成する

作成者: [Pratik Khandelwal](https://twitter.com/K2Prk) および [Scott Addie](https://twitter.com/Scott_Addie)

このチュートリアルでは、[MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL データベース上で Create、Read、Update、Delete の各操作 (CRUD) を実行するWeb API を作成します。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
> * MongoDB を構成する
> * MongoDB データベースを作成する
> * MongoDB のコレクションとスキーマを定義する
> * Web API から MongoDB CRUD 操作を実行する

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必須コンポーネント

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.2 以降](https://www.microsoft.com/net/download/all)
* **ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) バージョン 15.9 以降
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2.2 以降](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Visual Studio Code 用 C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* [.NET Core SDK 2.2 以降](https://www.microsoft.com/net/download/all)
* [Visual Studio for Mac バージョン 7.7 以降](https://www.visualstudio.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>MongoDB を構成する

Windows を使用する場合、MongoDB は既定では *C:\\Program Files\\MongoDB* にインストールされます。 *C:\\Program Files\\MongoDB\\Server\\\<バージョン番号>\\bin* を `Path` 環境変数に追加します。 この変更により、開発用コンピューターのどこからでも MongoDB にアクセスできるようになります。

次の手順では mongo シェルを使用して、データベースを作成し、コレクションを作成し、ドキュメントを保存します。 mongo のシェル コマンドについて詳しくは、「[Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell)」(mongo シェルの使用) をご覧ください。

1. データを格納するために開発用コンピューター上のディレクトリを選択します。 たとえば、Windows では *C:\\BooksData* です。 存在しない場合はディレクトリを作成します。 mongo シェルでは新しいディレクトリは作成されません。
1. コマンド シェルを開きます。 次のコマンドを実行して、既定のポート 27017 で MongoDB に接続します。 忘れずに、前の手順で選択したディレクトリで `<data_directory_path>` を置き換えます。

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. 別のコマンド シェル インスタンスを開きます。 次のコマンドを実行して、既定のテスト データベースに接続します。

    ```console
    mongo
    ```

1. コマンド シェルで次を実行します。

    ```console
    use BookstoreDb
    ```

    これがまだ存在していない場合、*BookstoreDb* という名前のデータベースが作成されます。 データベースが存在する場合は、トランザクションのために接続されます。

1. 次のコマンドを使用して `Books` コレクションを作成します。

    ```console
    db.createCollection('Books')
    ```

    次のような結果が表示されます。

    ```console
    { "ok" : 1 }
    ```

1. 次のコマンドを使用して、`Books` コレクションのスキーマを定義し、2 つのドキュメントを挿入します。

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    次のような結果が表示されます。

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. 次のコマンドを使用して、データベース内のドキュメントを表示します。

    ```console
    db.Books.find({}).pretty()
    ```

    次のような結果が表示されます。

    ```console
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
      "Name" : "Design Patterns",
      "Price" : 54.93,
      "Category" : "Computers",
      "Author" : "Ralph Johnson"
    }
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
      "Name" : "Clean Code",
      "Price" : 43.15,
      "Category" : "Computers",
      "Author" : "Robert C. Martin"
    }
    ```

    スキーマによって、自動生成された型が `ObjectId` の `_id` プロパティが各ドキュメントに追加されます。

データベースの準備ができました。 ASP.NET Core Web API の作成を開始できます。

## <a name="create-the-aspnet-core-web-api-project"></a>ASP.NET Core Web API プロジェクトを作成する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. **[ファイル]** > **[新規作成]** > **[プロジェクト]** に移動します。
1. **[ASP.NET Core Web アプリケーション]** を選択し、プロジェクト名を「*BooksApi*」として、**[OK]** をクリックします。
1. **[.NET Core]** ターゲット フレームワークと **[ASP.NET Core 2.1]** を選択します。 **[API]** プロジェクト テンプレートを選択し、**[OK]** をクリックします。
1. [NuGet ギャラリー:MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) に関するページを参照して、MongoDB 用 .NET ドライバーの最新の安定バージョンを確認します。 **[パッケージ マネージャー コンソール]** ウィンドウで、プロジェクトのルートに移動します。 次のコマンドを実行して、MongoDB 用の .NET ドライバーをインストールします。

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. コマンド シェルで次のコマンドを実行します。

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    .NET Core をターゲットとする新しい ASP.NET Core Web API プロジェクトが生成され、Visual Studio Code で開きます。

1. **[はい]** をクリックするのは、*[Required assets to build and debug are missing from 'BooksApi'.Add them?]\(構築とデバッグに必要なアセットが 'BooksApi' にありません。追加しますか?\)* という通知が表示された場合です。
1. [NuGet ギャラリー:MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) に関するページを参照して、MongoDB 用 .NET ドライバーの最新の安定バージョンを確認します。 **[統合端末]** を開き、プロジェクトのルートに移動します。 次のコマンドを実行して、MongoDB 用の .NET ドライバーをインストールします。

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. **[ファイル]** > **[新しいソリューション]** > **[.NET Core]** > **[アプリ]** に移動します。
1. **[ASP.NET Core Web API]** C# プロジェクト テンプレートを選択し、**[次へ]** をクリックします。
1. **[ターゲット フレームワーク]** ドロップダウン リストで **[.NET Core 2.2]** を選択し、**[次へ]** をクリックします。
1. **[プロジェクト名]** に「*BooksApi*」と入力し、**[作成]** をクリックします。
1. **[ソリューション]** パッドで、プロジェクトの **[依存関係]** ノードを右クリックし、**[パッケージを追加]** を選択します。
1. 検索ボックスに「*MongoDB.Driver*」と入力し、*MongoDB.Driver* パッケージを選択して、**[パッケージを追加]** をクリックします。
1. **[ライセンスへの同意]** ダイアログの **[承諾]** ボタンをクリックします。

---

## <a name="add-a-model"></a>モデルを追加する

1. *Models* ディレクトリをプロジェクトのルートに追加します。
1. 次のコードを使用して、`Book` クラスを *Models* ディレクトリに追加します。

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

上記のクラスでは、`Id` プロパティは:

* 共通言語ランタイム (CLR) オブジェクトを MongoDB コレクションにマッピングするために必須です。
* ドキュメントの主キーとしてこのプロパティを指定するために、`[BsonId]` で注釈を付けられています。
* `ObjectId` ではなく `string` 型としてパラメーターを渡すことができるようにするために、`[BsonRepresentation(BsonType.ObjectId)]` で注釈を付けられています。 Mongo によって `string` から `ObjectId` への変換が処理されます。

クラスのその他のプロパティは、`[BsonElement]` 属性で注釈を付けられています。 属性の値は、MongoDB コレクションでのプロパティ名を表します。

## <a name="add-a-crud-operations-class"></a>CRUD 操作のクラスを追加する

1. *Services* ディレクトリをプロジェクトのルートに追加します。
1. 次のコードを使用して、`BookService` クラスを *Services* ディレクトリに追加します。

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. MongoDB の接続文字列を *appsettings.json* に追加します。

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    上記の `BookstoreDb` プロパティは `BookService` クラス コンストラクターでアクセスされます。

1. `Startup.ConfigureServices` で、`BookService` クラスを依存関係挿入システムに登録します。

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    上記のサービスの登録は、コンシューマー クラスでコンス トラクターの挿入をサポートするために必要です。

`BookService` クラスは次の `MongoDB.Driver` メンバーを使用して、データベースに対する CRUD 操作を実行します。

* `MongoClient` &ndash; データベース操作を実行するためにサーバー インスタンスを読み取ります。 このクラスのコンストラクターには MongoDB 接続文字列が提供されます。

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* `IMongoDatabase` &ndash; 操作を実行する Mongo データベースを表します。 このチュートリアルでは、インターフェイスで汎用の `GetCollection<T>(collection)` メソッドを使用し、特定のコレクション内のデータへのアクセスを取得します。 CRUD 操作をこのコレクションに対して実行できるのは、このメソッドが呼び出された後です。 `GetCollection<T>(collection)` メソッドの呼び出しの内容は次のとおりです。
  * `collection` はコレクション名です。
  * `T` はコレクションに格納されている CLR オブジェクト型です。

`GetCollection<T>(collection)` は、コレクションを表す `MongoCollection` オブジェクトを返します。 このチュートリアルでは、コレクションに対して次のメソッドを呼び出します。

* `Find<T>` &ndash; 指定された検索基準と一致するコレクション内のすべてのドキュメントを返します。
* `InsertOne` &ndash; 指定されたオブジェクトを新しいドキュメントとしてコレクションに挿入します。
* `ReplaceOne` &ndash; 指定された検索基準と一致する 1 つのドキュメントを、指定されたオブジェクトで置き換えます。
* `DeleteOne` &ndash; 指定された検索基準と一致する 1 つのドキュメントを削除します。

## <a name="add-a-controller"></a>コントローラーの追加

1. 次のコードを使用して、`BooksController` クラスを *Controllers* ディレクトリに追加します。

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    上記の Web API コントローラー:

    * `BookService` クラスを使用して CRUD 操作を実行します。
    * GET、POST、PUT、DELETE HTTP 要求をサポートするアクション メソッドが含まれます。
1. アプリケーションをビルドし、実行します。
1. ブラウザーで `http://localhost:<port>/api/books` にナビゲートします。 次の JSON 応答が示されます。

    ```json
    [
      {
        "id":"5bfd996f7b8e48dc15ff215d",
        "bookName":"Design Patterns",
        "price":54.93,
        "category":"Computers",
        "author":"Ralph Johnson"
      },
      {
        "id":"5bfd996f7b8e48dc15ff215e",
        "bookName":"Clean Code",
        "price":43.15,
        "category":"Computers",
        "author":"Robert C. Martin"
      }
    ]
    ```

## <a name="next-steps"></a>次の手順

ASP.NET Core Web API の構築について詳しくは、次のリソースを参照してください。

* <xref:web-api/index>
* <xref:web-api/action-return-types>
