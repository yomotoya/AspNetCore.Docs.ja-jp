---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: "Entity Framework をモック作成時に単体テストの ASP.NET Web API 2 |Microsoft ドキュメント"
author: tfitzmac
description: "このガイダンスとアプリケーションは、Entity Framework を使用する Web API 2 アプリケーションの単体テストを作成する方法を説明します。 変更する方法を示しますが、."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 2d8a3df94c91d2fac79006916375764c2b90dc85
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Entity Framework をモック作成時に単体テストの ASP.NET Web API 2
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> このガイダンスとアプリケーションは、Entity Framework を使用する Web API 2 アプリケーションの単体テストを作成する方法を説明します。 Entity Framework を使用するテスト オブジェクトを作成する方法とテストは、コンテキスト オブジェクトを渡すことを有効にするスキャフォールディング コント ローラーを変更する方法を示しています。
> 
> ASP.NET Web API を使用した単体テストの概要については、次を参照してください。 [ASP.NET Web API 2 による単体テスト](unit-testing-with-aspnet-web-api.md)です。
> 
> このチュートリアルでは、ASP.NET Web API の基本概念を使い慣れて前提としています。 入門チュートリアルでは、次を参照してください。 [ASP.NET Web API 2 の概要](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)です。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2


## <a name="in-this-topic"></a>このトピックの内容

このトピックは、次のセクションで構成されています。

- [前提条件](#prereqs)
- [コードをダウンロードします。](#download)
- [単体テスト プロジェクトとアプリケーションを作成します。](#appwithunittest)
- [モデル クラスを作成します。](#modelclass)
- [コント ローラーを追加します。](#controller)
- [依存関係の挿入を追加します。](#dependency)
- [テスト プロジェクトの NuGet パッケージをインストールします。](#testpackages)
- [テスト コンテキストを作成します。](#testcontext)
- [テストを作成します。](#tests)
- [テストを実行します。](#runtests)

」の手順を既に完了している場合[ASP.NET Web API 2 による単体テスト](unit-testing-with-aspnet-web-api.md)、セクションにスキップできます[コント ローラーを追加](#controller)です。

<a id="prereqs"></a>
## <a name="prerequisites"></a>必須コンポーネント

Visual Studio 2017 Community、Professional edition または Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>コードをダウンロードします。

ダウンロード、[完成したプロジェクト](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)です。 ダウンロード可能なプロジェクトには、このトピックと単体テスト コードが含まれています、 [ASP.NET Web API 2 の単体テスト](unit-testing-with-aspnet-web-api.md)トピックです。

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>単体テスト プロジェクトとアプリケーションを作成します。

アプリケーションを作成するときに、単体テスト プロジェクトを作成するか、既存のアプリケーションに単体テスト プロジェクトを追加します。 このチュートリアルでは、アプリケーションを作成するときに、単体テスト プロジェクトを作成します。

という名前の新しい ASP.NET Web アプリケーションを作成する**StoreApp**です。

Windows では、新しい ASP.NET プロジェクト、選択、**空**テンプレート フォルダーを追加し、Web API の参照をコアです。 選択、**単体テストを追加**オプション。 単体テスト プロジェクトの名前が自動的に**StoreApp.Tests**です。 この名前を保持することができます。

![単体テスト プロジェクトを作成します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

アプリケーションを作成するには、後に表示されます - 2 つのプロジェクトが含まれている**StoreApp**と**StoreApp.Tests**です。

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>モデル クラスを作成します。

StoreApp プロジェクトにクラス ファイルを追加、**モデル**という名前のフォルダー **Product.cs**です。 ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

ソリューションをビルドします。

<a id="controller"></a>
## <a name="add-the-controller"></a>コント ローラーを追加します。

Controllers フォルダーを右クリックし **追加**と**スキャフォールディングされた新しい項目**です。 Entity Framework を使用して、アクションがある Web API 2 コント ローラーを選択します。

![新しいコント ローラーを追加します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

次の値を設定します。

- コント ローラー名: **ProductController**
- モデル クラス:**製品**
- データ コンテキスト クラス: [選択**新しいデータ コンテキスト**ボタン以下に示す値を設定する]

![コント ローラーを指定します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

をクリックして**追加**を自動的に生成されたコードで、コント ローラーを作成します。 コードには、作成、取得、更新、および Product クラスのインスタンスを削除するためのメソッドが含まれています。 次のコードは、メソッドの製品に追加します。 メソッドがのインスタンスを返すことに注意してください。 **IHttpActionResult**です。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult Web API 2 の新機能の 1 つは、単体テストの開発が簡単になります。

次のセクションでは、容易にするために生成されたコードをカスタマイズするコント ローラーにテスト オブジェクトを渡します。

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>依存関係の挿入を追加します。

現時点では、ProductController クラスでは、StoreAppContext クラスのインスタンスを使用するハード コーディングされたです。 アプリケーションを変更し、ハード コーディングされた依存関係を削除する依存関係の挿入と呼ばれるパターンを使用します。 この依存関係を分割することにより、モック オブジェクト内に渡すことができますをテストする場合。

右クリックし、**モデル**フォルダー、という名前の新しいインターフェイスを追加および**IStoreAppContext**です。

このコードを次のコードに置き換えます。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

StoreAppContext.cs ファイルを開き、次の強調表示された変更を加えます。 重要な変更点は次のとおりです。

- StoreAppContext クラス IStoreAppContext インターフェイスを実装します。
- MarkAsModified メソッドを実装します。


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

ProductController.cs ファイルを開きます。 強調表示されたコードのように既存のコードを変更します。 これらの変更は StoreAppContext で依存関係を中断し、別のオブジェクト コンテキスト クラスを渡すには、他のクラスを有効にします。 この変更は、単体テスト中に、テスト コンテキストに渡すことができますを有効になります。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

1 つの以上変更 ProductController で行う必要があります。 **PutProduct**メソッドは、置換するエンティティの状態を設定する行が MarkAsModified メソッドへの呼び出しで変更します。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

ソリューションをビルドします。

テスト プロジェクトを設定する準備が整いました。

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>テスト プロジェクトの NuGet パッケージをインストールします。

空のテンプレートを使用してアプリケーションを作成するときに、単体テスト プロジェクト (StoreApp.Tests) は、インストールされている NuGet パッケージが含まれません。 Web API テンプレートなどの他のテンプレートには、単体テスト プロジェクトで一部の NuGet パッケージが含まれます。 このチュートリアルでは、Entity Framework パッケージとテスト プロジェクトに Microsoft ASP.NET Web API 2 Core パッケージを含める必要があります。

StoreApp.Tests プロジェクトを右クリックし  **NuGet パッケージの管理**です。 そのプロジェクトにパッケージを追加するプロジェクトを StoreApp.Tests を選択する必要があります。

![パッケージを管理します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

オンラインのパッケージから検索して EntityFramework パッケージ (バージョン 6.0 またはそれ以降) をインストールします。 代わりに StoreApp プロジェクトが選択した可能性があります EntityFramework パッケージが既にインストールされている場合は、StoreApp.Tests プロジェクト。

![Entity Framework を追加します。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

検索し、Microsoft ASP.NET Web API 2 Core パッケージをインストールします。

![web api core パッケージをインストールします。](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

NuGet パッケージの管理ウィンドウを閉じます。

<a id="testcontext"></a>
## <a name="create-test-context"></a>テスト コンテキストを作成します。

という名前のクラスを追加**TestDbSet**テスト プロジェクトにします。 このクラスは、テスト データ セットの基本クラスとして機能します。 このコードを次のコードに置き換えます。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

という名前のクラスを追加**TestProductDbSet**を次のコードを含むテスト プロジェクトをします。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

という名前のクラスを追加**TestStoreAppContext**し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>テストの作成

既定では、テスト プロジェクトには、という名前の空のテスト ファイルが含まれる**UnitTest1.cs**です。 このファイルは、テスト メソッドの作成に使用する属性を示します。 このチュートリアルでは、新しいテスト クラスを追加するために、このファイルを削除することができます。

という名前のクラスを追加**TestProductController**テスト プロジェクトにします。 このコードを次のコードに置き換えます。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>テストの実行

テストを実行する準備が整いました。 マークされたメソッドのすべての**TestMethod**属性がテストされます。 **テスト**メニュー項目、テストを実行します。

![テストの実行](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

開く、**テスト エクスプ ローラー**ウィンドウ、およびテストの結果に注意してください。

![テスト結果](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
