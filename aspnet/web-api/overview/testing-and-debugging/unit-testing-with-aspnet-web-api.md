---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: 単体テストの ASP.NET Web API 2 |Microsoft ドキュメント
author: tfitzmac
description: このガイダンスとアプリケーションは、単純な単体テスト、Web API 2 アプリケーションを作成する方法を説明します。 このチュートリアルでは、単体テスト プロジェクトを含める方法を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042746"
---
<a name="unit-testing-aspnet-web-api-2"></a>単体テストの ASP.NET Web API 2
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> このガイダンスとアプリケーションは、単純な単体テスト、Web API 2 アプリケーションを作成する方法を説明します。 このチュートリアルでは、単体テスト プロジェクトをソリューションに含めるし、コント ローラー メソッドから返された値を確認するテスト メソッドを記述する方法を示します。
> 
> このチュートリアルでは、ASP.NET Web API の基本概念を使い慣れて前提としています。 入門チュートリアルでは、次を参照してください。 [ASP.NET Web API 2 の概要](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)です。
> 
> このトピックの単体テストは、単純なデータ シナリオに意図的に制限されます。 単体テスト データの高度なシナリオで次を参照してください。 [Entity Framework のモック作成時に ASP.NET Web API 2 の単体テスト](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)です。
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

    - [アプリケーションを作成するときに、単体テスト プロジェクトを追加します。](#whencreate)
    - [既存のアプリケーションの単体テスト プロジェクトを追加します。](#addtoexisting)
- [Web API 2 アプリケーションを設定します。](#setupproject)
- [テスト プロジェクトの NuGet パッケージをインストールします。](#testpackages)
- [テストを作成します。](#tests)
- [テストを実行します。](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>必須コンポーネント

Visual Studio 2017 Community、Professional edition または Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>コードをダウンロードします。

ダウンロード、[完成したプロジェクト](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)です。 ダウンロード可能なプロジェクトには、このトピックと単体テスト コードが含まれています、 [Entity Framework のモック作成時に単体テストの ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)トピックです。

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>単体テスト プロジェクトとアプリケーションを作成します。

アプリケーションを作成するときに、単体テスト プロジェクトを作成するか、既存のアプリケーションに単体テスト プロジェクトを追加します。 このチュートリアルでは、単体テスト プロジェクトを作成するための両方のメソッドを使用します。 このチュートリアルを実行するには、いずれかの方法を使用できます。

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>アプリケーションを作成するときに、単体テスト プロジェクトを追加します。

という名前の新しい ASP.NET Web アプリケーションを作成する**StoreApp**です。

![プロジェクトを作成します。](unit-testing-with-aspnet-web-api/_static/image1.png)

Windows では、新しい ASP.NET プロジェクト、選択、**空**テンプレート フォルダーを追加し、Web API の参照をコアです。 選択、**単体テストを追加**オプション。 単体テスト プロジェクトの名前が自動的に**StoreApp.Tests**です。 この名前を保持することができます。

![単体テスト プロジェクトを作成します。](unit-testing-with-aspnet-web-api/_static/image2.png)

アプリケーションを作成した後は、2 つのプロジェクトが含まれている参照してください。

![2 つのプロジェクト](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>既存のアプリケーションの単体テスト プロジェクトを追加します。

アプリケーションを作成したときに、単体テスト プロジェクトを作成しなかった場合は、いつでも追加できます。 たとえば、StoreApp、という名前のアプリケーションが既にあるし、単体テストを追加します。 単体テスト プロジェクトを追加するには、ソリューションを右クリックして**追加**と**新しいプロジェクト**です。

![新しいプロジェクトをソリューションに追加します。](unit-testing-with-aspnet-web-api/_static/image4.png)

選択**テスト**クリックし、左側のウィンドウで**単体テスト プロジェクト**プロジェクトの種類。 プロジェクトに名前を**StoreApp.Tests**です。

![単体テスト プロジェクトを追加します。](unit-testing-with-aspnet-web-api/_static/image5.png)

ソリューションの単体テスト プロジェクトが表示されます。

単体テスト プロジェクトでは、元のプロジェクトへのプロジェクト参照を追加します。

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Web API 2 アプリケーションを設定します。

StoreApp プロジェクトにクラス ファイルを追加、**モデル**という名前のフォルダー **Product.cs**です。 ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

ソリューションをビルドします。

Controllers フォルダーを右クリックし **追加**と**スキャフォールディングされた新しい項目**です。 選択**Web API 2 コント ローラー - 空**です。

![新しいコント ローラーを追加します。](unit-testing-with-aspnet-web-api/_static/image6.png)

コント ローラー名を設定します**SimpleProductController**、 をクリック**追加**です。

![コント ローラーを指定します。](unit-testing-with-aspnet-web-api/_static/image7.png)

既存のコードを次のコードに置き換えます。 この例を簡略化、データは、データベースではなく、一覧に格納されます。 このクラスで定義されている一覧では、実稼働データを表します。 コント ローラーに製品オブジェクトのリストをパラメーターとして受け取るコンス トラクターが含まれることに注意してください。 このコンス トラクターを使用すると、テスト データを渡すときに単体テストします。 コント ローラーも含まれています 2 **async**メソッドを単体テストの非同期メソッドを示しています。 これらの非同期メソッドを呼び出すことによって実装された**Task.FromResult** 、余分なコードが、通常のメソッドを最小限に抑えるにリソースを消費する操作が含まれます。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

GetProduct メソッドのインスタンスを返します、 **IHttpActionResult**インターフェイスです。 IHttpActionResult Web API 2 の新機能の 1 つは、単体テストの開発が簡単になります。 IHttpActionResult インターフェイスを実装するクラスは、 [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)名前空間。 これらのクラスは、操作要求から応答できるを表し、HTTP ステータス コードに対応します。

ソリューションをビルドします。

テスト プロジェクトを設定する準備が整いました。

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>テスト プロジェクトの NuGet パッケージをインストールします。

空のテンプレートを使用してアプリケーションを作成するときに、単体テスト プロジェクト (StoreApp.Tests) は、インストールされている NuGet パッケージが含まれません。 Web API テンプレートなどの他のテンプレートには、単体テスト プロジェクトで一部の NuGet パッケージが含まれます。 このチュートリアルでは、テスト プロジェクトに、Microsoft ASP.NET Web API 2 Core パッケージを含める必要があります。

StoreApp.Tests プロジェクトを右クリックし  **NuGet パッケージの管理**です。 そのプロジェクトにパッケージを追加するプロジェクトを StoreApp.Tests を選択する必要があります。

![パッケージを管理します。](unit-testing-with-aspnet-web-api/_static/image8.png)

検索し、Microsoft ASP.NET Web API 2 Core パッケージをインストールします。

![web api core パッケージをインストールします。](unit-testing-with-aspnet-web-api/_static/image9.png)

NuGet パッケージの管理ウィンドウを閉じます。

<a id="tests"></a>
## <a name="create-tests"></a>テストの作成

既定では、テスト プロジェクトには、UnitTest1.cs をという名前の空のテスト ファイルが含まれています。 このファイルは、テスト メソッドの作成に使用する属性を示します。 単体テストの場合は、このファイルを使用するか、独自のファイルを作成することができます。

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

このチュートリアルでは、独自のテスト クラスを作成します。 UnitTest1.cs ファイルを削除することができます。 という名前のクラスを追加**TestSimpleProductController.cs**コードを次のコードに置き換えます。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>テストの実行

テストを実行する準備が整いました。 マークされたメソッドのすべての**TestMethod**属性がテストされます。 **テスト**メニュー項目、テストを実行します。

![テストの実行](unit-testing-with-aspnet-web-api/_static/image11.png)

開く、**テスト エクスプ ローラー**ウィンドウ、およびテストの結果に注意してください。

![テスト結果](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>まとめ

このチュートリアルを完了しました。 このチュートリアルでは、データは、単体テストの条件に焦点を絞る意図的に簡素化されます。 単体テスト データの高度なシナリオで次を参照してください。 [Entity Framework のモック作成時に ASP.NET Web API 2 の単体テスト](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)です。
