---
title: ASP.NET Core でのサードパーティ コンテナーによるミドルウェアのアクティブ化
author: guardrex
description: ASP.NET Core で、ファクトリベースのアクティブ化とサードパーティ コンテナーによる厳密に型指定されたミドルウェアを使用する方法を説明します。
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 02/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: c55075fd3c6fda4073d26925eab823c35d8656f5
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729893"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a>ASP.NET Core でのサードパーティ コンテナーによるミドルウェアのアクティブ化

作成者: [Luke Latham](https://github.com/guardrex)

この記事では、[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) と [IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) を、サードパーティ コンテナーによる[ミドルウェア](xref:fundamentals/middleware/index)のアクティブ化の拡張ポイントとして使用する方法について説明します。 `IMiddlewareFactory` と `IMiddleware` の概要については、[ファクトリ ベースのミドルウェアのアクティブ化](xref:fundamentals/middleware/extensibility)に関するトピックを参照してください。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

このサンプル アプリでは、`IMiddlewareFactory` の実装である `SimpleInjectorMiddlewareFactory` によるミドルウェアのアクティブ化を示します。 このサンプルでは、[Simple Injector](https://simpleinjector.org) 依存関係の挿入 (DI) コンテナーを使用しています。

サンプルのミドルウェアの実装は、クエリ文字列パラメーターで提供された値を記録します (`key`)。 ミドルウェアは、メモリ内データベースにクエリ文字列値を記録するのに、挿入されたデータベース コンテキスト (スコープ化されたサービス) を使用します。

> [!NOTE]
> このサンプル アプリでは、デモンストレーション目的でのみ [Simple Injector](https://github.com/simpleinjector/SimpleInjector) を使用しています。 Simple Injector の使用を推奨するものではありません。 Simple Injector のドキュメントと GitHub Issues に記載されているミドルウェアのアクティブ化方法は、Simple Injector の保守担当者たちから推奨されています。 詳細については、[Simple Injector のドキュメント](https://simpleinjector.readthedocs.io/en/latest/index.html)と [Simple Injector の GitHub リポジトリ](https://github.com/simpleinjector/SimpleInjector)を参照してください。

## <a name="imiddlewarefactory"></a>IMiddlewareFactory

[IMiddlewareFactory](/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) では、ミドルウェアを作成するメソッドを提供します。

サンプル アプリでは、`SimpleInjectorActivatedMiddleware` インスタンスを作成するミドルウェア ファクトリが実装されています。 このミドルウェア ファクトリでは、Simple Injector コンテナーを使用してミドルウェアを解決しています。

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a>IMiddleware

[IMiddleware](/dotnet/api/microsoft.aspnetcore.http.imiddleware) では、アプリの要求パイプライン用にミドルウェアが定義されます。

`IMiddlewareFactory` の実装によってアクティブ化されるミドルウェア (*Middleware/SimpleInjectorActivatedMiddleware.cs*):

[!code-csharp[](extensibility-third-party-container/sample/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

ミドルウェア (*Middleware/MiddlewareExtensions.cs*) の拡張機能が作成されます。

[!code-csharp[](extensibility-third-party-container/sample/Middleware/MiddlewareExtensions.cs?name=snippet1)]

`Startup.ConfigureServices` はいくつかのタスクを実行する必要があります。

* Simple Injector コンテナーを設定します。
* ファクトリとミドルウェアを登録します。
* Razor ページの Simple Injector コンテナーからアプリのデータベース コンテキストを利用できるようにします。

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet1)]

ミドルウェアは、要求を処理する `Startup.Configure` のパイプラインに登録されます。

[!code-csharp[](extensibility-third-party-container/sample/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a>その他の技術情報

* [ミドルウェア](xref:fundamentals/middleware/index)
* [ファクトリ ベースのミドルウェアのアクティブ化](xref:fundamentals/middleware/extensibility)
* [Simple Injector の GitHub リポジトリ](https://github.com/simpleinjector/SimpleInjector)
* [Simple Injector のドキュメント](https://simpleinjector.readthedocs.io/en/latest/index.html)
