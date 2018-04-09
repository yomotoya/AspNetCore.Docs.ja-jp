---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: この章のビルドのダウンロードを EF 5 MVC 4 のチュートリアル |Microsoft ドキュメント
author: Rick-Anderson
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: bda7effabd715e4658d2472e1f0a66d7bba18cab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>この章のビルドのダウンロードを EF 5 MVC 4 のチュートリアル
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)をご覧ください。


## <a name="building-the-chapter-downloads"></a>章のダウンロードの構築

1. ダウンロードし、プロジェクトのサンプルの zip ファイルを解凍します。 解凍したダウンロード パッケージでは、各章の完了のいずれかの追加の zip ファイルが見つかります。
2. 目的の zip ファイルを右クリックし、をクリックして**プロパティ**、 をクリックし、**ブロックを解除する**ボタンをクリックします。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. ファイルを解凍します。
4. ダブルクリックして、 *CUx.sln*ファイルを Visual Studio を起動します。
5. **ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**、し**Package Manager Console**です。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. パッケージ マネージャー コンソール (PMC) をクリックして**復元**です。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Visual Studio を終了します。
8. 上記の手順で閉じられたか、ソリューション ファイルを開いて、Visual Studio を再起動します。
9. パッケージ マネージャー コンソール (PMC) を入力してください、`Update-Database`コマンド。  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > : 次のエラーが発生した場合  
    >   
    >  *用語 'データベースの更新' は、コマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前としては認識されません。名のスペルを確認するかパスが含まれている場合、パスが正しいことを確認し、もう一度やり直してください。*  
    > 終了し、Visual Studio を再起動します。

    各移行を実行してから、シード メソッドが実行されます。 今すぐアプリを実行することができます。

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [前へ](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
