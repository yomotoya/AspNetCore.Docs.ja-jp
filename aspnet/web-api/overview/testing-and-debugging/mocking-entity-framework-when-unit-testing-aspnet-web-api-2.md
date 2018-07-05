---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Entity Framework のモックを作成するときに単体テストの ASP.NET Web API 2 |Microsoft Docs
author: tfitzmac
description: このガイダンスとアプリケーションは、Entity Framework を使用する Web API 2 アプリケーションの単体テストを作成する方法を説明します。 変更する方法を示しますが、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f1ff2fda85a6d56a6bbb76b1bff740301ab0c70d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371870"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Entity Framework のモックを作成するときに単体テストの ASP.NET Web API 2
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> このガイダンスとアプリケーションは、Entity Framework を使用する Web API 2 アプリケーションの単体テストを作成する方法を説明します。 テストは、コンテキスト オブジェクトを渡すことを有効にするスキャフォールディングされたコント ローラーを変更する方法と Entity Framework で動作するテスト オブジェクトを作成する方法を示しています。
> 
> ASP.NET Web API を使用した単体テストの概要については、次を参照してください。 [ASP.NET Web API 2 で Unit Testing](unit-testing-with-aspnet-web-api.md)します。
> 
> このチュートリアルでは、ASP.NET Web API の基本的な概念に慣れてを前提としています。 入門チュートリアルについては、次を参照してください。 [ASP.NET Web API 2 の概要](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2


## <a name="in-this-topic"></a>このトピックの内容

このトピックは、次のセクションで構成されています。

- [前提条件](#prereqs)
- [コードをダウンロードします。](#download)
- [単体テスト プロジェクトでアプリケーションを作成します。](#appwithunittest)
- [モデル クラスを作成します。](#modelclass)
- [コント ローラーを追加します。](#controller)
- [追加の依存関係の挿入](#dependency)
- [テスト プロジェクトで NuGet パッケージをインストールします。](#testpackages)
- [テストのコンテキストを作成します。](#testcontext)
- [テストを作成します。](#tests)
- [テストを実行します。](#runtests)

」の手順を既に完了している場合[ASP.NET Web API 2 で Unit Testing](unit-testing-with-aspnet-web-api.md)、セクションに進んで[コント ローラーの追加](#controller)します。

<a id="prereqs"></a>
## <a name="prerequisites"></a>必須コンポーネント

Visual Studio 2017 Community、Professional または Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>コードをダウンロードします。

ダウンロード、[完成したプロジェクト](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)します。 ダウンロード可能なプロジェクトには、このトピックでは、単体テストのコードが含まれています、 [ASP.NET Web API 2 の単体テスト](unit-testing-with-aspnet-web-api.md)トピック。

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>単体テスト プロジェクトでアプリケーションを作成します。

アプリケーションを作成するときに、単体テスト プロジェクトを作成するか、単体テスト プロジェクトを既存のアプリケーションに追加します。 このチュートリアルでは、アプリケーションを作成するときに単体テスト プロジェクトの作成を示します。

という名前の新しい ASP.NET Web アプリケーション作成**StoreApp**します。

新しい ASP.NET プロジェクト ウィンドウで選択、**空**テンプレート フォルダーを追加し、Web API の参照をコアしします。 選択、**単体テストを追加**オプション。 単体テスト プロジェクトが自動的に名前付き**StoreApp.Tests**します。 この名前を保持することができます。

![単体テスト プロジェクトを作成します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

アプリケーションを作成すると、表示されます - 2 つのプロジェクトが含まれている**StoreApp**と**StoreApp.Tests**します。

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>モデル クラスを作成します。

StoreApp プロジェクトで、追加のクラス ファイルを**モデル**という名前のフォルダー **Product.cs**します。 ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

ソリューションをビルドします。

<a id="controller"></a>
## <a name="add-the-controller"></a>コント ローラーを追加します。

Controllers フォルダーを右クリックして**追加**と**スキャフォールディングされた新しい項目**します。 Entity Framework を使用して、アクションがある Web API 2 コント ローラーを選択します。

![新しいコント ローラーを追加します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

次の値を設定します。

- コント ローラー名: **「productcontroller」**
- モデル クラス:**製品**
- データ コンテキスト クラス: [選択**新しいデータ コンテキスト**ボタンの下に表示される値を設定する]

![コント ローラーを指定します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

クリックして**追加**自動的に生成されたコードで、コント ローラーを作成します。 コードには、作成、取得、更新、Product クラスのインスタンスを削除するためのメソッドが含まれています。 次のコードは、メソッドの製品を追加します。 メソッドがのインスタンスを返すことに注意してください。 **IHttpActionResult**します。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult は、Web API 2 では、新機能の 1 つと、単体テストの開発が簡単になります。

次のセクションでは、容易に生成されたコードをカスタマイズしますコント ローラーにテスト オブジェクトを渡します。

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>追加の依存関係の挿入

現時点では、「productcontroller」クラスは、ハードコーディング StoreAppContext クラス インスタンスを使用します。 依存関係の挿入をという名前のパターンを使用すると、アプリケーションを変更し、ハード コーディングされた依存関係を削除します。 縛りをなくすことによってテストするときにモック オブジェクトに渡すことができます。

右クリックし、**モデル**フォルダー、という名前の新しいインターフェイスを追加および**IStoreAppContext**します。

このコードを次のコードに置き換えます。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

StoreAppContext.cs ファイルを開き、次の強調表示された変更を加えます。 注意する重要な変更点は次のとおりです。

- StoreAppContext クラス IStoreAppContext インターフェイスを実装します。
- MarkAsModified メソッドを実装します。


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

ProductController.cs ファイルを開きます。 コードを強調表示されているように、既存のコードを変更します。 これらの変更は StoreAppContext で依存関係を中断し、別のオブジェクト コンテキスト クラスを渡すには、他のクラスを有効にします。 この変更は、単体テスト中に、テスト コンテキストに渡すことを有効になります。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

これは、もう 1 つ変更が「productcontroller」で行う必要があります。 **PutProduct**メソッドは、置換するエンティティの状態を設定する行が MarkAsModified メソッドの呼び出しで変更します。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

ソリューションをビルドします。

テスト プロジェクトを設定する準備が整いました。

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>テスト プロジェクトで NuGet パッケージをインストールします。

空のテンプレートを使用してアプリケーションを作成するときに単体テスト プロジェクト (StoreApp.Tests) にインストールされている NuGet パッケージは含まれません。 Web API テンプレートなどの他のテンプレートには、単体テスト プロジェクトで NuGet パッケージ一部にはが含まれます。 このチュートリアルでは、Entity Framework パッケージとテスト プロジェクトに Microsoft ASP.NET Web API 2 Core パッケージを含める必要があります。

StoreApp.Tests プロジェクトを右クリックして**NuGet パッケージの管理**します。 そのプロジェクトにパッケージを追加する StoreApp.Tests プロジェクトを選択する必要があります。

![パッケージを管理します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

オンラインのパッケージから検索して、EntityFramework パッケージ (バージョン 6.0 以降) をインストールします。 EntityFramework パッケージが既にインストールされていることが表示される場合は、StoreApp.Tests プロジェクトではなく StoreApp プロジェクトを選択した可能性があります。

![Entity Framework を追加します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

検索して、Microsoft ASP.NET Web API 2 コア パッケージをインストールします。

![web api のコア パッケージをインストールします。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

NuGet パッケージの管理ウィンドウを閉じます。

<a id="testcontext"></a>
## <a name="create-test-context"></a>テストのコンテキストを作成します。

という名前のクラスを追加**TestDbSet**テスト プロジェクトにします。 このクラスは、テスト データ セットの基本クラスとして機能します。 このコードを次のコードに置き換えます。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

という名前のクラスを追加**TestProductDbSet**を次のコードを含むテスト プロジェクトにします。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

という名前のクラスを追加**TestStoreAppContext**既存のコードを次のコードに置き換えます。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>テストの作成

既定では、テスト プロジェクトにという名前の空のテスト ファイルが含まれます。 **UnitTest1.cs**します。 このファイルは、テスト メソッドの作成に使用する属性を示します。 このチュートリアルでは、新しいテスト クラスを追加するために、このファイルを削除することができます。

という名前のクラスを追加**TestProductController**テスト プロジェクトにします。 このコードを次のコードに置き換えます。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>テストの実行

テストを実行する準備が整いました。 マークされているメソッドはすべて、 **TestMethod**属性がテストされます。 **テスト**メニュー項目、テストを実行します。

![テストの実行](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

開く、**テスト エクスプ ローラー**ウィンドウで、テストの結果を注意してください。

![テスト結果](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
