---
title: ASP.NET Core でのリポジトリ パターン
author: ardalis
description: ASP.NET Core アプリでリポジトリ アプリの設計パターンを実装する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342632"
---
# <a name="repository-pattern-with-aspnet-core"></a>ASP.NET Core でのリポジトリ パターン

作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)、[Luke Latham](https://github.com/guardrex)

"*リポジトリ パターン*" とは、データのアクセスをインターフェイスの抽象化の背後に分離する設計パターンです。 データベースへの接続やデータ ストレージ オブジェクトの操作を、インターフェイスの実装によって提供されるメソッドを使用して実行します。 その結果、データベースに関する問題 (接続、コマンド、閲覧者など) に対処するためにコードを呼び出す必要が生じません。

ASP.NET Core でリポジトリ パターンを実装することの利点は次のとおりです。

* ビジネス層とデータ アクセス層の間で相互に直接依存することがないため、アプリの構成が簡素になります。
* データベースへのアクセス コードが 1 つまたは複数のリポジトリによって一元的に管理されるため、コードの再利用が簡単になります。
* データベース層から独立してビジネス ドメインを単体テストできます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples)では、映画の登場人物の名前の一覧を初期化して表示するために、リポジトリ パターンが使用されます。 アプリはデータ永続化のために [Entity Framework Core](/ef/core/) と `ApplicationDbContext` クラスを使用しますが、データベースのインフラストラクチャはデータにアクセスする場所からは見えません。 データ アクセスとデータベース オブジェクトは、[リポジトリ](https://martinfowler.com/eaaCatalog/repository.html)の背後に抽象化されます。

## <a name="repository-interface"></a>リポジトリ インターフェイス

リポジトリ インターフェイスでは、実装のプロパティとメソッドが定義されます。 サンプル アプリでは、映画の登場人物データのリポジトリ インターフェイスは `ICharacterRepository` です。 `ICharacterRepository` では、アプリの `Character` インスタンスを操作するために必要な `ListAll` メソッドと `Add` メソッドが定義されます。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

`Character` は次のように定義します。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>リポジトリの具象型

インターフェイスは具象型によって実装されます。 サンプル アプリでは、`CharacterRepository` でデータベース コンテキストが管理され、`ListAll` メソッドと `Add` メソッドが実装されます。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>リポジトリ サービスの登録

リポジトリとデータベースのコンテキストは、`Startup.ConfigureServices` でサービス コンテナーに登録されます。 サンプル アプリでは、`ApplicationDbContext` は、拡張メソッド [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) を呼び出して構成します。 `ICharacterRepository` はスコープ サービスとして登録されます。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>リポジトリのインスタンスの挿入

データベースへのアクセスが必要なクラスでは、リポジトリのインスタンスがコンストラクターを介して要求され、クラスのメソッドが使用するためにプライベート フィールドに割り当てられます。 サンプル アプリでは、次の操作を行うために `ICharacterRepository` が使用されます。

* 登場人物が存在しない場合にデータベースにデータを入力します。
* 表示用に登場人物のリストを取得します。

呼び出し元のコードが、インターフェイスの実装 (`CharacterRepository`) とのみやり取りしていることに注目します。 呼び出し元のコードは `ApplicationDbContext` を直接使用しません。

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>ジェネリック リポジトリ インターフェイス

このトピックとサンプル アプリでは、リポジトリ パターンの最も簡単な実装を紹介しました。そこでは、ビジネス オブジェクトごとに 1 つのリポジトリが作成されました。 アプリが成長してオブジェクトの数が増えた場合、"*ジェネリック リポジトリ インターフェイス*" を使用することで、リポジトリ パターンを実装するために必要なコードの量を減らすことができる場合があります。 詳細については、[DevIQ: リポジトリ パターン: ジェネリック リポジトリ インターフェイス](http://deviq.com/repository-pattern/)に関するページをご覧ください。

## <a name="additional-resources"></a>その他の技術情報

* [DevIQ: リポジトリ パターン](https://deviq.com/repository-pattern/)
* [依存関係の挿入](xref:fundamentals/dependency-injection)
* [ビューへの依存性の注入](xref:mvc/views/dependency-injection)
* [コントローラーへの依存性の注入](xref:mvc/controllers/dependency-injection)
* [要件ハンドラーでの依存性の注入](xref:security/authorization/dependencyinjection)
* [制御の反転コンテナーと依存関係の挿入パターン](https://www.martinfowler.com/articles/injection.html)
