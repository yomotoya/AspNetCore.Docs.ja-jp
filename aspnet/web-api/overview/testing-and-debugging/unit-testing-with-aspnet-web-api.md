---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: 単体テストの ASP.NET Web API 2 |Microsoft Docs
author: tfitzmac
description: このガイダンスとアプリケーションは、単純な単体テスト、Web API 2 アプリケーションを作成する方法を説明します。 このチュートリアルでは、単体テストのプロジェクトを含める方法を使用しています.
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 91cca2789b66b7b8983f8786b506c5fc71db8b75
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795435"
---
<a name="unit-testing-aspnet-web-api-2"></a>単体テストの ASP.NET Web API 2
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

[完成したプロジェクトのダウンロード](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> このガイダンスとアプリケーションは、単純な単体テスト、Web API 2 アプリケーションを作成する方法を説明します。 このチュートリアルでは、単体テスト プロジェクトをソリューションに含めるし、コント ローラー メソッドから返された値をチェックするテスト メソッドを記述する方法を示します。
>
> このチュートリアルでは、ASP.NET Web API の基本的な概念に慣れてを前提としています。 入門チュートリアルについては、次を参照してください。 [ASP.NET Web API 2 の概要](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)します。
>
> このトピックの「単体テストは、単純なデータ シナリオに意図的に制限されます。 単体テストより高度なデータ シナリオで次を参照してください。 [Entity Framework のモック作成時に ASP.NET Web API 2 の単体テスト](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)します。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web API 2

## <a name="in-this-topic"></a>このトピックの内容

このトピックは、次のセクションで構成されています。

- [前提条件](#prereqs)
- [コードをダウンロードします。](#download)
- [単体テスト プロジェクトでアプリケーションを作成します。](#appwithunittest)
    - [アプリケーションを作成するときに、単体テスト プロジェクトを追加します。](#whencreate)
    - [単体テスト プロジェクトを既存のアプリケーションに追加します。](#addtoexisting)
- [Web API 2 アプリケーションを設定します。](#setupproject)
- [テスト プロジェクトで NuGet パッケージをインストールします。](#testpackages)
- [テストを作成します。](#tests)
- [テストを実行します。](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>必須コンポーネント

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community、Professional または Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>コードをダウンロードします。

ダウンロード、[完成したプロジェクト](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)します。 ダウンロード可能なプロジェクトには、このトピックでは、単体テストのコードが含まれています、 [Entity Framework のモック作成時に単体テストの ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)トピック。

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>単体テスト プロジェクトでアプリケーションを作成します。

アプリケーションを作成するときに、単体テスト プロジェクトを作成するか、単体テスト プロジェクトを既存のアプリケーションに追加します。 このチュートリアルでは、2 つの単体テスト プロジェクトの作成方法を示します。 このチュートリアルを実行するには、どちらの方法を使用することができます。

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>アプリケーションを作成するときに、単体テスト プロジェクトを追加します。

という名前の新しい ASP.NET Web アプリケーション作成**StoreApp**します。

![プロジェクトを作成します。](unit-testing-with-aspnet-web-api/_static/image1.png)

新しい ASP.NET プロジェクト ウィンドウで選択、**空**テンプレート フォルダーを追加し、Web API の参照をコアしします。 選択、**単体テストを追加**オプション。 単体テスト プロジェクトが自動的に名前付き**StoreApp.Tests**します。 この名前を保持することができます。

![単体テスト プロジェクトを作成します。](unit-testing-with-aspnet-web-api/_static/image2.png)

アプリケーションを作成した後は、2 つのプロジェクトが含まれている参照してください。

![2 つのプロジェクト](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>単体テスト プロジェクトを既存のアプリケーションに追加します。

アプリケーションを作成したときに単体テスト プロジェクトを作成しなかった場合は、いつでも追加できます。 たとえば、StoreApp、という名前のアプリケーションが既にあるし、単体テストを追加したいとします。 単体テスト プロジェクトを追加するには、ソリューションを右クリックして**追加**と**新しいプロジェクト**します。

![新しいプロジェクトをソリューションに追加します。](unit-testing-with-aspnet-web-api/_static/image4.png)

選択**テスト**クリックし、左側のウィンドウで**単体テスト プロジェクト**プロジェクトの種類。 プロジェクトに名前を**StoreApp.Tests**します。

![単体テスト プロジェクトを追加します。](unit-testing-with-aspnet-web-api/_static/image5.png)

ソリューションの単体テスト プロジェクトが表示されます。

単体テスト プロジェクトでは、元のプロジェクトへのプロジェクト参照を追加します。

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Web API 2 アプリケーションを設定します。

StoreApp プロジェクトで、追加のクラス ファイルを**モデル**という名前のフォルダー **Product.cs**します。 ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

ソリューションをビルドします。

Controllers フォルダーを右クリックして**追加**と**スキャフォールディングされた新しい項目**します。 選択**Web API 2 コント ローラー - 空**します。

![新しいコント ローラーを追加します。](unit-testing-with-aspnet-web-api/_static/image6.png)

コント ローラー名を設定**SimpleProductController**、 をクリック**追加**します。

![コント ローラーを指定します。](unit-testing-with-aspnet-web-api/_static/image7.png)

既存のコードを次のコードに置き換えます。 この例を簡略化するのには、データがデータベースではなく、リストに格納されます。 このクラスで定義されている一覧は、実稼働データを表します。 コント ローラーが Product オブジェクトの一覧をパラメーターとして受け取るコンス トラクターが含まれることに注意してください。 このコンス トラクターを使用すると、テスト データを渡すときに単体テストします。 2 つは、コント ローラーも含む**async**メソッドの単体テストの非同期メソッドを説明するためにします。 これらの非同期メソッドを呼び出すことによって実装された**Task.FromResult** 、余分なコードが、通常、メソッドを最小限に抑えるにリソースを消費する操作が含まれます。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

GetProduct メソッドのインスタンスを返します、 **IHttpActionResult**インターフェイス。 IHttpActionResult は、Web API 2 では、新機能の 1 つと、単体テストの開発が簡単になります。 IHttpActionResult インターフェイスを実装するクラスは、 [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)名前空間。 これらのクラスは、操作要求からのような応答を表し、HTTP 状態コードに対応します。

ソリューションをビルドします。

テスト プロジェクトを設定する準備が整いました。

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>テスト プロジェクトで NuGet パッケージをインストールします。

空のテンプレートを使用してアプリケーションを作成するときに単体テスト プロジェクト (StoreApp.Tests) にインストールされている NuGet パッケージは含まれません。 Web API テンプレートなどの他のテンプレートには、単体テスト プロジェクトで NuGet パッケージ一部にはが含まれます。 このチュートリアルでは、Microsoft ASP.NET Web API 2 Core パッケージをテスト プロジェクトを含める必要があります。

StoreApp.Tests プロジェクトを右クリックして**NuGet パッケージの管理**します。 そのプロジェクトにパッケージを追加する StoreApp.Tests プロジェクトを選択する必要があります。

![パッケージを管理します。](unit-testing-with-aspnet-web-api/_static/image8.png)

検索して、Microsoft ASP.NET Web API 2 コア パッケージをインストールします。

![web api のコア パッケージをインストールします。](unit-testing-with-aspnet-web-api/_static/image9.png)

NuGet パッケージの管理ウィンドウを閉じます。

<a id="tests"></a>
## <a name="create-tests"></a>テストの作成

既定では、テスト プロジェクトには、UnitTest1.cs をという名前を空のテスト ファイルが含まれています。 このファイルは、テスト メソッドの作成に使用する属性を示します。 単体テスト用には、このファイルを使用するか、独自のファイルを作成します。

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

このチュートリアルでは、独自のテスト クラスを作成します。 UnitTest1.cs ファイルを削除することができます。 という名前のクラスを追加**TestSimpleProductController.cs**コードを次のコードに置き換えます。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>テストの実行

テストを実行する準備が整いました。 マークされているメソッドはすべて、 **TestMethod**属性がテストされます。 **テスト**メニュー項目、テストを実行します。

![テストの実行](unit-testing-with-aspnet-web-api/_static/image11.png)

開く、**テスト エクスプ ローラー**ウィンドウで、テストの結果を注意してください。

![テスト結果](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>まとめ

このチュートリアルを完了しました。 このチュートリアルでは、データは、単体テストの条件に重点を置く意図的に簡略化されました。 単体テストより高度なデータ シナリオで次を参照してください。 [Entity Framework のモック作成時に ASP.NET Web API 2 の単体テスト](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)します。
