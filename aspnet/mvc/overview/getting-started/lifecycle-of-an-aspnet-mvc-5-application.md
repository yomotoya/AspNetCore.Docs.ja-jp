---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: "ASP.NET MVC 5 アプリケーションのライフ サイクル |Microsoft ドキュメント"
author: cephalin
description: "ASP.NET MVC 5 アプリケーションのライフ サイクルをグラフ化する PDF ドキュメントをダウンロードします。 このライフ サイクルのドキュメントでは、MVC のライフ サイクルの概要を表示する."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5692c43168eb261c91f40e2046897a1e5d31a028
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>ASP.NET MVC 5 アプリケーションのライフ サイクル
====================
によって[Cephas Lin](https://github.com/cephalin)

[PDF ドキュメントをダウンロードします。](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

HTTP の受信からのすべての ASP.NET MVC 5 アプリケーションのライフ サイクルを HTTP 応答を送信することを要求するグラフがクライアントに返信する PDF ドキュメントをダウンロードできます。 ASP.NET MVC に追加された新しい管理者向けの教育用のツール、およびアプリケーションの特定の側面にドリル ダウンする必要がある方のための参照としてもされています。 PDF ドキュメントには、次の機能があります。

- 関連する[HttpApplication](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx)に MVC が統合されたを理解するための段階、 [ASP.NET アプリケーションのライフ サイクル](https://msdn.microsoft.com/en-us/library/bb470252.aspx)です。
- すべての MVC アプリケーションが要求処理パイプラインを通過する主なステージを理解することができます、MVC アプリケーション ライフ サイクルの概要を表示します。  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- 詳細の表示で、ダウン要求処理パイプラインの詳細にドリルを示しています。 高レベルのビューと詳細を表示する、さまざまな段階にライフ サイクルの詳細を収集する方法を比較できます。 [PDF のダウンロード](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)より大きなビューを表示します。
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- 配置およびオーバーライド可能なすべてのメソッドの目的は、[コント ローラー](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.aspx)要求処理パイプライン内のオブジェクト。 可能性がありますか、任意の 1 つのメソッドをオーバーライドする必要がある可能性がありますが注意する効果のライフ サイクルの適切な段階でコードを作成できるように、アプリケーションのライフ サイクルにおける各自の役割を理解するためです。
- フィルターの種類 (認証、承認、アクション、および結果) の各を呼び出す方法を示す飛んでアップ図。
- 詳細ビューで、目的の各ポイントから、役に立つ記事やブログにリンクします。


## <a name="next-steps"></a>次の手順

このドキュメントは、ニーズを満たしていますか。 お客様のフィードバックをお待ちしています。 ASP.NET MVC のライフ サイクルで、アプリケーションでどの質問がある場合[Stackoverflow](http://stackoverflow.com/help)と[ASP.NET MVC フォーラム](https://forums.asp.net/1146.aspx)最適な場所に確認してください。 次の[me](https://twitter.com/Cephas_MSFT)ツイッター 最新のチュートリアルで更新プログラムを取得できるようにします。
