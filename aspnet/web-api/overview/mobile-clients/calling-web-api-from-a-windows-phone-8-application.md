---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: 8 アプリケーション (c#) を、Windows Phone から Web API を呼び出す |Microsoft Docs
author: rmcmurray
description: Windows Phone 8 アプリケーションに書籍のカタログを提供する ASP.NET Web API アプリケーションから成る完全なエンド ツー エンド シナリオを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 40be2935c52e7dcab9e682d4d15e9e75c0260223
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388423"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Windows Phone 8 アプリケーション (c#) から Web API の呼び出し
====================
によって[Robert McMurray](https://github.com/rmcmurray)

このチュートリアルでは、Windows Phone 8 アプリケーションに書籍のカタログを提供する ASP.NET Web API アプリケーションから成る完全なエンド ツー エンド シナリオを作成する方法を学びます。

### <a name="overview"></a>概要

ASP.NET Web API などの rESTful サービスは、サーバー側とクライアント側のアプリケーションのアーキテクチャを取り除くことで開発者のための HTTP ベースのアプリケーションの作成を簡略化します。 専用ソケット ベースのプロトコルの通信を作成する代わりに Web API の開発者だけを必要な HTTP メソッドを公開する (例: GET、POST、PUT、DELETE)、クライアント アプリケーションの開発者がのみを使用する必要がありますそのアプリケーションに必要な HTTP メソッド。

このエンド ツー エンド チュートリアルでは、Web API を使用して、次のプロジェクトを作成する方法を学習します。

- [このチュートリアルの最初の部分](#STEP1)はすべての書籍のカタログの管理の作成、読み取り、更新、および削除 (CRUD) 操作をサポートする ASP.NET Web API アプリケーションを作成します。 このアプリケーションを使用して、[サンプル XML ファイル (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) MSDN から。
- [このチュートリアルのパート 2 つ目の](#STEP2)は、Web API アプリケーションからデータを取得する対話型の Windows Phone 8 アプリケーションを作成します。

#### <a name="prerequisites"></a>必須コンポーネント

- Windows Phone 8 sdk がインストールされた visual Studio 2013
- Windows 8 または後で、HYPER-V がインストールされていると、64 ビット システム
- その他の要件の一覧は、次を参照してください。、*システム要件*セクションで、 [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471)ページをダウンロードします。

> [!NOTE]
> Web API と、ローカル システム上の Windows Phone 8 プロジェクトの間の接続をテストしようとするで手順を実行する必要があります、 *[にローカルで Web API アプリケーション、Windows Phone 8 エミュレーターを接続します。コンピューター](https://go.microsoft.com/fwlink/?LinkId=324014)* に関する記事をテスト環境を設定します。


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>手順 1: Web API の書店プロジェクトの作成

このエンド ツー エンドのチュートリアルの最初の手順は、すべての CRUD 操作; をサポートする Web API プロジェクトを作成するにはこのソリューションで、Windows Phone アプリケーションのプロジェクトが追加されることに注意してください。[手順 2.](#STEP2)このチュートリアルの。

1. 開いている**Visual Studio 2013**します。
2. クリックして**ファイル**、し**新しい**、し**プロジェクト**します。
3. ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**インストール済み**、し**テンプレート**、し**Visual c#**、し**Web**します。


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                展開するイメージをクリックします。                                                                |


4. 強調表示**ASP.NET Web アプリケーション**、入力**BookStore**プロジェクト名、およびクリックの **[ok]** します。
5. ときに、**新しい ASP.NET プロジェクト** ダイアログ ボックスが表示されたら、選択、 **Web API**テンプレート、およびクリック**OK**。


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                展開するイメージをクリックします。                                                                |


6. Web API プロジェクトが開いたら、プロジェクトから、サンプル コント ローラーを削除します。

    1. 展開、**コント ローラー**ソリューション エクスプ ローラーでフォルダー。
    2. 右クリックし、 **ValuesController.cs**ファイルを開き、をクリックし、**削除**します。
    3. クリックして**OK**削除の確認を求められたらします。
7. Web API プロジェクトに XML データ ファイルを追加します。このファイルには、bookstore カタログの内容が含まれています。

   1. 右クリックし、**アプリ\_データ**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、順にクリックします**新しい項目の**します。
   2. ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、強調表示、 **XML ファイル**テンプレート。
   3. ファイルに名前を**Books.xml**、 をクリックし、**追加**します。
   4. ときに、 **Books.xml**ファイルを開くと、ファイル内のコードをサンプルから XML に置き換えます**books.xml** MSDN 上のファイル。 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. 保存して XML ファイルを閉じます。

8. Bookstore モデルを Web API プロジェクトに追加します。このモデルには、書店アプリケーションの作成、読み取り、更新、および削除 (CRUD) のロジックが含まれています。

   1. 右クリックし、**モデル**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、 をクリックし、**クラス**します。
   2. ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、クラス ファイルに名前を**BookDetails.cs**、 をクリックし、**追加**します。
   3. ときに、 **BookDetails.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. 保存して閉じます、 **BookDetails.cs**ファイル。

9. Web API プロジェクトには、bookstore コント ローラーを追加します。

   1. 右クリックし、**コント ローラー**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、 をクリックし、**コント ローラー**します。
   2. ときに、**スキャフォールディングの追加** ダイアログ ボックスが表示されたら、強調表示**Web API 2 コント ローラー - 空**、 をクリックし、**追加**します。
   3. ときに、**コント ローラーの追加** ダイアログ ボックスが表示されたら、名前、コント ローラー **BooksController**、 をクリックし、**追加**します。
   4. ときに、 **BooksController.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. 保存して閉じます、 **BooksController.cs**ファイル。

10. エラーをチェックする Web API アプリケーションをビルドします。

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>手順 2: Windows Phone 8 Bookstore カタログのプロジェクトを追加します。

このエンド ツー エンド シナリオの次の手順では、Windows Phone 8 向けカタログ アプリケーションを作成します。 このアプリケーションを使用して、 *Windows Phone のデータ バインド アプリ*テンプレートでは、既定のユーザー インターフェイスで作成した Web API アプリケーションが使用されます[手順 1.](#STEP1)データ ソースとしては、このチュートリアルの。

1. 右クリックし、**書店**でソリューション、ソリューション エクスプ ローラーでをクリックし、**追加**、し**新しいプロジェクト**します。
2. ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**インストール済み**、し**Visual c#**、し**Windows Phone**します。
3. 強調表示**Windows Phone のデータ バインド アプリ**、入力**BookCatalog**名、およびクリック **[ok]** します。
4. Json.NET の NuGet パッケージを追加、 **BookCatalog**プロジェクト。

    1. 右クリックして**参照**の**BookCatalog**ソリューション エクスプ ローラーでプロジェクトをクリックして**NuGet パッケージの管理**します。
    2. ときに、 **NuGet パッケージの管理** ダイアログ ボックスが表示されたら、展開、**オンライン**セクションし、強調表示**nuget.org**します。
    3. 入力**Json.NET**検索フィールドし、検索アイコンをクリックします。
    4. 強調表示**Json.NET**検索結果、およびクリックで**インストール**します。
    5. インストールが完了したら、クリックして**閉じる**します。
5. 追加、 **BookDetails**モデルを**BookCatalog**プロジェクト; bookstore クラスの汎用モデルが含まれます。

   1. 右クリックし、 **BookCatalog**ソリューション エクスプ ローラーでプロジェクトの [**追加**、] をクリックし、**新しいフォルダー**します。
   2. 新しいフォルダーの名前**モデル**します。
   3. 右クリックし、**モデル**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、 をクリックし、**クラス**します。
   4. ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、クラス ファイルに名前を**BookDetails.cs**、 をクリックし、**追加**します。
   5. ときに、 **BookDetails.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. 保存して閉じます、 **BookDetails.cs**ファイル。

6. 更新プログラム、 **MainViewModel.cs** BookStore Web API アプリケーションと通信するための機能を含めるクラス。

   1. 展開、 **ViewModels**フォルダーをダブルクリックして、ソリューション エクスプ ローラーで、 **MainViewModel.cs**ファイル。
   2. ときに、 **MainViewModel.cs**ファイルが開かれる、次のファイルのコードに置き換えます以外の値を更新する必要がありますに注意してください、`apiUrl`定数と、Web API の実際の URL:。 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. 保存して閉じます、 **MainViewModel.cs**ファイル。

7. 更新プログラム、 **MainPage.xaml**アプリケーション名をカスタマイズするファイル。

   1. ダブルクリックして、 **MainPage.xaml**ソリューション エクスプ ローラーでファイル。
   2. ときに、 **MainPage.xaml**ファイルを開くと、次のコード行を見つけます。 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. 次のようにこれらの行に置き換えます。 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. 保存して閉じます、 **MainPage.xaml**ファイル。

8. 更新プログラム、 **DetailsPage.xaml**表示されている項目をカスタマイズするファイル。

   1. ダブルクリックして、 **DetailsPage.xaml**ソリューション エクスプ ローラーでファイル。
   2. ときに、 **DetailsPage.xaml**ファイルを開くと、次のコード行を見つけます。 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. 次のようにこれらの行に置き換えます。 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. 保存して閉じます、 **DetailsPage.xaml**ファイル。

9. エラーをチェックする Windows Phone アプリケーションをビルドします。

### <a name="step-3-testing-the-end-to-end-solution"></a>手順 3: エンド ツー エンド ソリューションをテストします。

説明したように、*の前提条件*Web API と Windows Phone 8 間の接続をテストするときに、このチュートリアルのセクションが、ローカル システムでプロジェクトの場合の手順を実行する必要があります、  *[ローカル コンピューター上の Web API アプリケーションに、Windows Phone 8 エミュレーターを接続する](https://go.microsoft.com/fwlink/?LinkId=324014)* に関する記事をテスト環境を設定します。

構成テスト環境を作成したら、Windows Phone アプリケーションをスタートアップ プロジェクトとして設定する必要があります。 これを行うには、強調表示、 **BookCatalog**アプリケーションで、ソリューション エクスプ ローラーとクリック**スタートアップ プロジェクトとして設定**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| 展開するイメージをクリックします。 |

F5 キーを押すと、Visual Studio がエミュレーターを起動両方、Windows Phone が表示されます、&quot;お待ちください&quot;メッセージ アプリケーションのデータは、Web API から取得されます。

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| 展開するイメージをクリックします。 |

すべてが成功した場合は、表示されるカタログが表示されます。

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| 展開するイメージをクリックします。 |

書籍のタイトルをタップすると、アプリケーションにより、ブックの説明が表示されます。

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| 展開するイメージをクリックします。 |

アプリケーションは、Web API と通信できない、エラー メッセージが表示されます。

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| 展開するイメージをクリックします。 |

エラー メッセージでタップすると、エラーに関する詳細が表示されます。


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 展開するイメージをクリックします。                                                                 |

