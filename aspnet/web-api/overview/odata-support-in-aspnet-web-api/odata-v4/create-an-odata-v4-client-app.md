---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: "OData v4 クライアント アプリ (c#) を作成する | Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="create-an-odata-v4-client-app-c"></a>OData v4 クライアント アプリ (c#) を作成する
====================
[Mike Wasson](https://github.com/MikeWasson) によって

前のチュートリアルでは、CRUD 操作をサポートする基本的な OData サービスを作成しました。 次はサービスのクライアントを作成しましょう。

まず、Visual Studio の新しいインスタンスを開始し、新しいコンソール アプリケーション プロジェクトを作成します。 **新しいプロジェクト** ダイアログでは **インストール済み** &gt; **テンプレート** &gt; **Visual C#** &gt; **Windows デスクトップ**を選択し、**コンソール アプリケーション**テンプレートを選択します。 プロジェクトには &quot;ProductsApp&quot; と名前をつけます。

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> OData サービスを含んでいる同じ Visual Studio ソリューションにコンソール アプリを追加することもできます。


## <a name="install-the-odata-client-code-generator"></a>OData Client Code Generator をインストールする

次に **ツール** のメニューから **拡張機能と更新プログラム** を選択します。 **オンライン** &gt; **Visual Studio Gallery** を選択して、 [検索] ボックスから &quot;OData Client Code Generator&quot; を検索してください。 **ダウンロード** をクリックして VSIX をインストールします。 ここでは　Visual Studio の再起動を求められることがあります。

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>OData サービスをローカルで実行する

Visual Studio から ProductService プロジェクトを実行します。 既定では、Visual Studio はアプリケーション ルートに対してブラウザーを起動します。次の手順で必要になるので、ここでの URI を書き留めておいてください。 アプリケーションは実行されている状態のままにします。

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> 同じソリューションに両方のプロジェクトを配置する場合は ProductService プロジェクトがデバッグをせずに実行されていることを確認してください。 次の手順では、コンソール アプリケーション プロジェクトを変更している間、サービスが実行され続けている必要があります。


## <a name="generate-the-service-proxy"></a>サービス プロキシを生成します。

サービス プロキシは、OData サービスにアクセスするためのメソッドを定義する .NET クラスです。 プロキシは、HTTP 要求にメソッド呼び出しを変換します。 実行して、プロキシ クラスを作成するが、 [T4 テンプレート](https://msdn.microsoft.com/library/bb126445.aspx)です。

プロジェクトを右クリックします。 選択**追加** &gt; **新しい項目の**します。

![](create-an-odata-v4-client-app/_static/image5.png)

**新しい項目の追加**ダイアログで、 **Visual c# アイテム** &gt; **コード** &gt; **OData クライアント**です。 テンプレートに名前を&quot;ProductClient.tt&quot;です。 をクリックして**追加**でセキュリティの警告 をクリックします。

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

この時点では、無視してかまいませんが、エラーが表示されます。 Visual Studio は、テンプレートを自動的に実行されますが、テンプレートには、一部の構成設定が必要があります最初。

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

ProductClient.odata.config ファイルを開きます。`Parameter` ProductService プロジェクト (前の手順) 元の URI での貼り付けの要素。 例:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

テンプレートをもう一度実行します。 ソリューション エクスプ ローラーで、ProductClient.tt ファイルを右クリックし、選択**カスタム ツールの実行**です。

テンプレートでは、プロキシを定義する ProductClient.cs をという名前のコード ファイルを作成します。 OData エンドポイントを変更する場合に、アプリを開発するときは、プロキシを更新するには、再度テンプレートを実行します。

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>サービス プロキシを使用して、OData サービスの呼び出し

Program.cs ファイルを開き、次のように定型コードを置き換えます。

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

値を置き換える*serviceUri*以前からサービス URI とします。

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

アプリを実行するときに、次と出力されます。

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
