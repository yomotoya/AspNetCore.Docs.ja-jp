---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: 8 アプリケーション (c#) を、Windows Phone から Web API を呼び出して |Microsoft ドキュメント
author: rmcmurray
description: Windows Phone 8 アプリケーションに書籍のカタログを提供する ASP.NET Web API アプリケーションで構成される完全なエンド ツー エンド シナリオを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 7d0486b4cab85ffe77fda87d4b34dd3ec0a9e8fe
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874227"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Windows Phone 8 アプリケーション (c#) からの Web API の呼び出し
====================
によって[Robert McMurray](https://github.com/rmcmurray)

このチュートリアルでは、Windows Phone 8 アプリケーションに書籍のカタログを提供する ASP.NET Web API アプリケーションで構成される完全なエンド ツー エンド シナリオを作成する方法を学習します。

### <a name="overview"></a>概要

ASP.NET Web API などの rESTful サービスは、サーバー側とクライアント側のアプリケーションのアーキテクチャを抽象化され、開発者のための HTTP ベースのアプリケーションの作成を簡略化します。 通信用にある独自のソケット ベースのプロトコルを作成するには、代わりに Web API の開発者だけをそのアプリケーションの必要条件の HTTP メソッドを公開する (例: GET、POST、PUT、DELETE)、およびクライアント アプリケーションの開発者がのみを使用する必要がありますそのアプリケーションに必要な HTTP メソッド。

このエンド ツー エンド チュートリアルでは、Web API を使用して、次のプロジェクトを作成する方法を学習します。

- [このチュートリアルの最初の部分](#STEP1)はすべての書籍のカタログを管理する作成、読み取り、更新、および削除 (CRUD) 操作をサポートする ASP.NET Web API アプリケーションを作成します。 このアプリケーションを使用して、[サンプル XML ファイル (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) MSDN からです。
- [2 番目のこのチュートリアルのパート](#STEP2)は、Web API アプリケーションからデータを取得する対話型の Windows Phone 8 アプリケーションを作成します。

#### <a name="prerequisites"></a>必須コンポーネント

- Visual Studio 2013、Windows Phone 8 SDK がインストールされていると
- Windows 8 または後でインストールされている HYPER-V と 64 ビット システム
- その他の要件の一覧は、次を参照してください。、*システム要件*セクションで、 [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471)ページをダウンロードします。

> [!NOTE]
> Web API と、ローカル システム上の Windows Phone 8 プロジェクト間の接続をテストすることがで手順に従う必要があります、 *[ローカル上の Web API アプリケーションを Windows Phone 8 エミュレーターの接続コンピューター](https://go.microsoft.com/fwlink/?LinkId=324014)* アーティクルをテスト環境を設定します。


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>手順 1: Web API Bookstore プロジェクトの作成

このエンド ツー エンド チュートリアルの最初のステップが、CRUD 操作のすべてをサポートする Web API プロジェクトを作成するにはこのソリューションで Windows Phone アプリケーション プロジェクトを追加することに注意してください[手順 2.](#STEP2)このチュートリアルのです。

1. 開いている**Visual Studio 2013**です。
2. をクリックして**ファイル**、し**新しい**、し**プロジェクト**です。
3. ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**インストール**、し**テンプレート**、し**Visual c#**、し**Web**です。


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                画像をクリックすると、展開                                                                |


4. 強調表示**ASP.NET Web アプリケーション**、入力**BookStore**プロジェクト名、およびクリック **[ok]** です。
5. ときに、**新しい ASP.NET プロジェクト** ダイアログ ボックスが表示されたら、選択、 **Web API**テンプレート、およびクリック**OK**です。


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                画像をクリックすると、展開                                                                |


6. Web API プロジェクトを開くと、プロジェクトから、サンプル コント ローラーを削除します。

    1. 展開して、**コント ローラー**ソリューション エクスプ ローラーでフォルダーです。
    2. 右クリックし、 **ValuesController.cs**ファイルを開き、をクリックして**削除**です。
    3. をクリックして**OK**削除の確認を求められたらです。
7. XML データ ファイルを Web API プロジェクトに追加します。このファイルには、bookstore カタログの内容が含まれています。

   1. 右クリックし、**アプリ\_データ**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、順にクリック**新しい項目の**します。
   2. ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、強調表示、 **XML ファイル**テンプレート。
   3. ファイルの名前を付けます**Books.xml**、クリックして**追加**です。
   4. ときに、 **Books.xml**ファイルを開くと、ファイルのコード サンプルの XML に置き換えます**books.xml** MSDN 上のファイル。 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. 保存して XML ファイルを閉じます。

8. Web API プロジェクトに bookstore モデルを追加します。このモデルには、bookstore アプリケーションの作成、読み取り、更新、および削除 (CRUD) のロジックが含まれています。

   1. 右クリックし、**モデル**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、クリックして**クラス**です。
   2. ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、クラス ファイルの名前を付けます**BookDetails.cs**、クリックして**追加**です。
   3. ときに、 **BookDetails.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. 保存して閉じます、 **BookDetails.cs**ファイル。

9. Web API プロジェクトに bookstore コント ローラーを追加します。

   1. 右クリックし、**コント ローラー**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、クリックして**コント ローラー**です。
   2. ときに、**追加 Scaffold**  ダイアログ ボックスが表示されたら、強調表示**Web API 2 コント ローラー - 空**、順にクリック**追加**です。
   3. ときに、**コント ローラーの追加** ダイアログ ボックスが表示されたら、コント ローラーの名前を付けます**BooksController**、クリックして**追加**です。
   4. ときに、 **BooksController.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. 保存して閉じます、 **BooksController.cs**ファイル。

10. エラーをチェックする Web API アプリケーションをビルドします。

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>手順 2: Windows Phone 8 Bookstore カタログ プロジェクトの追加

このエンド ツー エンド シナリオの次の手順では、Windows Phone 8 用のカタログのアプリケーションを作成します。 このアプリケーションで使用、 *Windows Phone のデータ バインド アプリ*で作成した Web API アプリケーションをテンプレートでは、既定のユーザー インターフェイスが使用される[手順 1.](#STEP1)データ ソースとしては、このチュートリアルのです。

1. 右クリックし、 **BookStore**でソリューションは、ソリューション エクスプ ローラーで、をクリックして**追加**、し、**新しいプロジェクト**です。
2. ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**インストール**、し**Visual c#**、し**Windows Phone**です。
3. 強調表示**Windows Phone のデータ バインド アプリ**、入力**BookCatalog**  をクリックし、名前の**ok**です。
4. Json.NET の NuGet パッケージを追加、 **BookCatalog**プロジェクト。

    1. 右クリック**参照**の**BookCatalog**ソリューション エクスプ ローラーでプロジェクトをクリックして**NuGet パッケージの管理**です。
    2. ときに、 **NuGet パッケージの管理** ダイアログ ボックスが表示されたら、展開、**オンライン**セクションし、強調表示**nuget.org**です。
    3. 入力**Json.NET**検索フィールドし、検索アイコンをクリックします。
    4. 強調表示**Json.NET**をクリックして、検索結果**インストール**です。
    5. インストールが完了したらをクリックして**閉じる**です。
5. 追加、 **BookDetails**モデルを**BookCatalog**プロジェクト; bookstore クラスの汎用モデルが含まれます。

   1. 右クリックし、 **BookCatalog**プロジェクト、ソリューション エクスプ ローラーで、をクリックして**追加**、クリックして**新しいフォルダー**です。
   2. 新しいフォルダーの名前を付けます**モデル**です。
   3. 右クリックし、**モデル**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、クリックして**クラス**です。
   4. ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、クラス ファイルの名前を付けます**BookDetails.cs**、クリックして**追加**です。
   5. ときに、 **BookDetails.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. 保存して閉じます、 **BookDetails.cs**ファイル。

6. 更新プログラム、 **MainViewModel.cs**クラス BookStore Web API アプリケーションと通信する機能を含めます。

   1. 展開して、 **ViewModels**フォルダーをクリックしてソリューション エクスプ ローラーで、 **MainViewModel.cs**ファイル。
   2. ときに、 **MainViewModel.cs**ファイルを開く、ファイル内のコードを次に置き換えますの値を更新する必要があります、`apiUrl`定数と、Web API の実際の URL:。 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. 保存して閉じます、 **MainViewModel.cs**ファイル。

7. 更新プログラム、 **MainPage.xaml**アプリケーション名をカスタマイズするファイル。

   1. ダブルクリックして、 **MainPage.xaml**ソリューション エクスプ ローラーでファイル。
   2. ときに、 **MainPage.xaml**ファイルを開くと、次のコード行を探します。 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. これらの行を次のように置き換えます。 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. 保存して閉じます、 **MainPage.xaml**ファイル。

8. 更新プログラム、 **DetailsPage.xaml**表示されている項目をカスタマイズするファイル。

   1. ダブルクリックして、 **DetailsPage.xaml**ソリューション エクスプ ローラーでファイル。
   2. ときに、 **DetailsPage.xaml**ファイルを開くと、次のコード行を探します。 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. これらの行を次のように置き換えます。 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. 保存して閉じます、 **DetailsPage.xaml**ファイル。

9. エラーをチェックする Windows Phone アプリケーションをビルドします。

### <a name="step-3-testing-the-end-to-end-solution"></a>手順 3: エンド ツー エンド ソリューションをテストします。

説明したように、*の前提条件*このチュートリアルでは、Web API と Windows Phone 8 の間の接続をテストするときのセクションでは、ローカル システムでプロジェクトの場合はで手順に従う必要があります、  *[ローカル コンピューター上の Web API アプリケーションを Windows Phone 8 エミュレーターを接続する](https://go.microsoft.com/fwlink/?LinkId=324014)* アーティクルをテスト環境を設定します。

構成されているテスト環境を作成したら、Windows Phone アプリケーションをスタートアップ プロジェクトとして設定する必要があります。 これを行うには、強調表示、 **BookCatalog**をクリックして、ソリューション エクスプ ローラーで、アプリケーション**スタートアップ プロジェクトとして設定**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| 画像をクリックすると、展開 |

F5 キーを押すと、Visual Studio が起動両方 Windows Phone エミュレーターが表示されます、&quot;お待ちください&quot;Web API からアプリケーション データを取得中のメッセージします。

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| 画像をクリックすると、展開 |

すべてのものが成功した場合は、表示されるカタログが表示されます。

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| 画像をクリックすると、展開 |

場合は、書籍のタイトルをタップすると、アプリケーションにはブックの説明が表示されます。

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| 画像をクリックすると、展開 |

アプリケーションは、Web API と通信できません、エラー メッセージが表示されます。

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| 画像をクリックすると、展開 |

エラー メッセージでタップすると、エラーに関する詳細が表示されます。


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 画像をクリックすると、展開                                                                 |

