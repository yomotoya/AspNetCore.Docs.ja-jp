---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: "パート 2: データ アクセス層 |Microsoft ドキュメント"
author: JoeStagner
description: "このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 2 部では、データ アクセス レイヤーの追加について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8b07b320640c1bb0074a4d3a04ca7c5b7e7bb6cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-2-data-access-layer"></a>パート 2: データ アクセス層
====================
によって[行える](https://github.com/JoeStagner)

> Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。 ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。
> 
> このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 2 部では、データ アクセス レイヤーの追加について説明します。


## <a id="_Toc260221668"></a>データ アクセス レイヤーの追加

電子商取引アプリケーションは、2 つのデータベースに左右されます。

顧客情報は、標準の ASP.NET メンバーシップ データベースを使用します。 弊社のショッピング カートや製品カタログのおを実装 SQL Express データベースには、次のようにします。

![](tailspin-spyworks-part-2/_static/image1.jpg)

データベース (Commerce.mdf) アプリケーションのアプリで作成\_データ フォルダーを続けていく中 .NET Entity Framework を使用して、データ アクセス レイヤーを作成することができます。

という名前のフォルダーを作成します"データ\_アクセス"し、そのフォルダーを右クリックし、「新しい項目の追加」を選択します。

「インストールされたテンプレート」の項目と選択し、「ADO.NET エンティティ データ モデル」入力 EDM\_Commerce.edmx 名として「追加」ボタンをクリックします。

![](tailspin-spyworks-part-2/_static/image2.jpg)

「データベースから生成」を選択します。

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

保存し、ビルドします。

これで、最初の機能: 製品カテゴリ メニューを追加する準備が整いました。

>[!div class="step-by-step"]
[前へ](tailspin-spyworks-part-1.md)
[次へ](tailspin-spyworks-part-3.md)
