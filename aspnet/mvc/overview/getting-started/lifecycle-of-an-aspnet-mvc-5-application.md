---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: ASP.NET MVC 5 アプリケーションのライフ サイクル |Microsoft Docs
author: cephalin
description: ASP.NET MVC 5 アプリケーションのライフ サイクルをグラフ化する PDF ドキュメントをダウンロードします。 このライフ サイクルのドキュメントは、MVC のライフ サイクルの概要を確認する.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 98735f2e04bdd0f2fec19524e59f6272dbc4ca57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393916"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>ASP.NET MVC 5 アプリケーションのライフ サイクル
====================
によって[Cephas Lin](https://github.com/cephalin)

[PDF ドキュメントをダウンロードします。](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

ここで、HTTP の受信からのすべての ASP.NET MVC 5 アプリケーションのライフ サイクルは、HTTP 応答を送信する要求のグラフは、クライアントにバックしている PDF ドキュメントをダウンロードできます。 新しい ASP.NET MVC には、教育ツール、およびアプリケーションの特定の側面にドリル ダウンする必要がある方のための参照としても設計されています。 PDF ドキュメントには、次の機能があります。

- 関連する[HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) MVC が統合を理解するためのステージ、 [ASP.NET アプリケーションのライフ サイクル](https://msdn.microsoft.com/library/bb470252.aspx)します。
- 要求処理パイプラインですべての MVC アプリケーションを通過する主なステージを理解することができます、MVC アプリケーション ライフ サイクルの概要を確認します。  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- 要求処理パイプラインの詳細にダウン ドリルを表示する詳細ビュー。 概要ビューと詳細を表示するさまざまな段階のライフ サイクルの詳細を収集する方法を比較することができます。 [PDF をダウンロード](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)にすると拡大表示を参照してください。
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- 配置およびオーバーライド可能なすべてのメソッドの目的、[コント ローラー](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx)要求処理パイプライン内のオブジェクト。 あるか、任意の 1 つのメソッドをオーバーライドする必要がある可能性がありますが、する効果の適切なライフ サイクルの段階でコードを記述するには、アプリケーションのライフ サイクルでのロールを理解するため重要です。
- 驚くほど衝撃的アップ コンテンツの各フィルターの種類 (認証、承認、アクション、および結果) が呼び出される方法を示す図。
- 詳細ビューで、関心のある各ポイントから役に立つ記事やブログにリンクします。


## <a name="next-steps"></a>次の手順

このドキュメントは、ニーズを満たしているか。 ご意見ご提案があります。 アプリケーションでは、ASP.NET MVC のライフ サイクルでの質問がある場合[Stackoverflow](http://stackoverflow.com/help)と[ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)は質問に最適な場所。 次の[me](https://twitter.com/Cephas_MSFT) twitter で最新のチュートリアルでは、更新プログラムを取得できるようにします。
