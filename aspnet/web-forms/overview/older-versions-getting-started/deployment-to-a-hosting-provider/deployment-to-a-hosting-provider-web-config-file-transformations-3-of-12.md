---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: Web.Config ファイルの変換 - 3/12 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9673887fb39874e1b4f063fa9cfe3470e4ccde00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829931"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: Web.Config ファイルの変換 - 3/12
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。 シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、変更するプロセスを自動化する方法、 *Web.config*ファイルの別の変換先の環境にデプロイするとき。 ほとんどのアプリケーション設定を持つ、 *Web.config*ファイルをアプリケーションの配置時に異なる必要があります。 このような変更のプロセスを自動化すると、展開すると面倒でエラーが発生するたびに手動で行うことがなくなります保持します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web.config 変換と Web デプロイのパラメーター

2 つの方法を変更するプロセスを自動化する*Web.config*ファイルの設定: [Web.config 変換](https://msdn.microsoft.com/library/dd465326.aspx)と[Web 配置パラメーター](https://msdn.microsoft.com/library/ff398068.aspx)します。 A *Web.config*変換ファイルには変更する方法を指定する XML マークアップが含まれています、 *Web.config*ファイルが展開されている場合にします。 特定のさまざまな変更がビルド構成と、特定の発行プロファイルを指定することができます。 既定のビルド構成はデバッグとリリースでは、およびカスタム ビルド構成を作成することができます。 発行プロファイルは、通常、インポート先の環境に対応します。 (でプロファイルの発行の詳細について説明します、[テスト環境として IIS への配置](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアルです)。

Web 配置パラメーターを使用して、さまざまな種類の設定に含まれる設定を含む、展開中に構成する必要がありますを指定することができます*Web.config*ファイル。 指定する際に*Web.config*ファイルの変更は、Web 配置パラメーターを設定するより複雑ですを展開するまでに設定する値がわからない場合に便利ですが。 など、エンタープライズ環境で作成、*展開パッケージ*と、運用環境でインストールするには、IT 部門で人にたってから、そのユーザーが、接続文字列やパスワードをしない」と入力します知っています。

このシナリオは、このチュートリアルの内容を把握しておくすべてのことを行う必要があります、 *Web.config*ファイル、Web 配置パラメーターを使用する必要はありません。 使用すると、ビルド構成によって異なりますが、使用された発行プロファイルによって異なるいくつかをいくつかの変換を構成します。

## <a name="creating-transformation-files-for-publish-profiles"></a>発行プロファイルの変換ファイルの作成

**ソリューション エクスプ ローラー**、展開*Web.config*を参照してください、 *Web.Debug.config*と*Web.Release.config*ファイルの変換既定では 2 つの既定のビルド構成ごとに作成されます。

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

カスタム ビルド構成の変換ファイルを作成するには、Web.config ファイルを右クリックして**構成変換の追加**コンテキスト メニューから、このチュートリアルでは、その必要はありません。

ビルド構成ではなく、配置先に関連する変更を構成するため、他の 2 つの変換ファイルの必要がありますの操作を行います。 この種の設定の典型的な例は、実稼働とテストのさまざまなである WCF エンドポイントです。 作成後のチュートリアルで発行する必要があるために、テスト、運用をという名前のプロファイル、 *Web.Test.config*ファイルと*Web.Production.config*ファイル。

発行プロファイルに関連付けられている変換ファイルを手動で作成する必要があります。 **ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、選択、 **Windows エクスプ ローラーでフォルダーを開く**します。

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

**Windows エクスプ ローラー**を選択、 *Web.Release.config*ファイル、ファイルをコピーし、その 2 つのコピーを貼り付けます。 これらのコピーの名前を変更*Web.Production.config*と*Web.Test.config*終値、 **Windows エクスプ ローラー**します。

**ソリューション エクスプ ローラー**、 をクリックして**更新**に新しいファイルを参照してください。

新しいファイル、右クリックし、およびクリック選択**プロジェクトに含める**のコンテキスト メニュー。

![プロジェクトのテストと運用環境の構成ファイルを含む](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

これらのファイルが展開されていることを防ぐために選択して **ソリューション エクスプ ローラー**、し、**プロパティ**ウィンドウの変更、**ビルド アクション**からプロパティ**コンテンツ**に**None**します。 (ビルド構成に基づいて変換ファイルを配置から自動的にできません)。

入力する準備が整いました*Web.config*変換を*Web.config*変換ファイル。

## <a name="limiting-error-log-access-to-administrators"></a>管理者にエラー ログへのアクセスを制限します。

アプリケーションの実行中にエラーがある場合は、アプリケーションには、システムによって生成されたエラー ページの代わりに一般的なエラー ページが表示されますを使用して[Elmah NuGet パッケージ](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)エラー ログとレポートします。 `customErrors`内の要素、 *Web.config*ファイルは、エラー ページを指定します。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

エラー ページを表示するには、一時的に変更、`mode`の属性、`customErrors`要素から"RemoteOnly"を"オン"に、Visual Studio からアプリケーションを実行します。 エラーが発生するなど、無効な URL を要求することによって*Studentsxxx.aspx*します。 IIS で生成された「ページが見つかりません」エラー ページの代わりに、 *GenericErrorPage.aspx*ページ。

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

エラー ログを表示するとポート番号の後、URL 内のすべてを置換*elmah.axd* (、スクリーン ショットの例で`http://localhost:51130/elmah.axd`) し、Enter キーを押します。

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

設定することを忘れないでください、`customErrors`要素を完了すると、"RemoteOnly"モードに戻ります。

開発用コンピューターには、エラー ログ ページには、セキュリティ リスクとなる運用環境での無料アクセスを許可すると便利です。 運用サイトで変換を構成することで、管理者にエラー ログへのアクセスを制限する承認規則を追加することができます、 *Web.Production.config*ファイル。

開いている*Web.Production.config*し、新しい追加`location`要素の開始後すぐに`configuration`タグを次に示します。 (だけに追加することを確認、`location`要素とは、ここでいくつかのコンテキストを提供することのみ表示されている周辺のマークアップされません)。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform`属性値"Insert"の場合、この`location`に既存のすべての兄弟として追加する要素`location`内の要素、 *Web.config*ファイル。 (既に 1 つである`location`の承認を指定する要素の規則、**更新クレジット**ページです)。デプロイ後に、実稼働サイトをテストする場合は、この承認規則が有効であることを確認するテストします。

このコードを追加する必要はありませんので、テスト環境でエラー ログへのアクセスを制限する必要はありません、 *Web.Test.config*ファイル。

> [!NOTE] 
> 
> **セキュリティに関する注意**で実稼働アプリケーションでは、一般にエラーの詳細を表示または公共の場所にその情報を格納することはありません。 攻撃者は、エラー情報を使用して、サイトの脆弱性を検出することができます。 独自のアプリケーションで ELMAH を使用する場合は、セキュリティ上のリスクを最小限に抑える ELMAH を構成する方法を調査することを確認します。 このチュートリアルの例では ELMAH しない推奨される構成を検討してください。 アプリケーションはファイルを作成できる必要があるフォルダーを処理する方法を説明するために実装されている一例です。


## <a name="setting-an-environment-indicator"></a>環境の評価指標を設定

一般的なシナリオとして*Web.config*ファイルの設定を環境に展開するごとに変える必要があります。 たとえば、WCF サービスを呼び出すアプリケーションは、テストおよび運用環境で別のエンドポイントを必要があります。 Contoso University アプリケーションには、この種類の設定が含まれます。 この設定は、開発、テスト、運用などは、in、環境を示すサイトのページに表示されるインジケーターを制御します。 設定値は、アプリケーションが"(Dev)"を追加するかどうかを決定します。 または"(テスト)"メインの見出しに、 *Site.Master*マスター ページ。

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

アプリケーションが運用環境で実行されている場合、環境のインジケーターは省略されます。

Contoso University の web ページで設定されている値の読み取り`appSettings`で、 *Web.config*ファイルで、アプリケーションが実行されているどのような環境を決定するには。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

値は、""テスト環境では、"Prod"で、運用環境でテストする必要があります。

開いている*Web.Production.config*を追加し、`appSettings`の開始タグの直前の要素、`location`要素の前に追加します。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform`属性値"SetAttributes"では、この変換の目的で既存の要素の属性値を変更することを示します、 *Web.config*ファイル。 `xdt:Locator`属性値"Match(key)"では、変更する要素が、1 つであることを示しますが`key`属性と一致する、`key`ここで指定された属性。 のみその他の属性の`add`要素が`value`、変更される内容は、デプロイされているの*Web.config*ファイル。 このコードにより、`value`の属性、 `Environment` `appSettings` "Prod"に設定する要素、 *Web.config*を運用環境に展開されているファイル。

次に、同じ変更を適用*Web.Test.config*セット以外のファイル、 `value` "Prod"の代わりに"Test"にします。 済んだら、`appSettings`セクション*Web.Test.config*は次の例のようになります。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>デバッグ モードを無効にします。

リリース ビルドでは、たくないデバッグを有効にデプロイする環境に関係なく。 既定では、 *Web.Release.config*変換ファイルを削除するコードを自動作成、`debug`属性を`compilation`要素。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform`原因の属性、`debug`属性が省略、デプロイされているから*Web.config*リリース ビルドをデプロイするたびにファイルします。

この同じ変換は、テストと運用環境に変換ファイル リリース変換ファイルをコピーすることによってそれらを作成しているためです。 不要になったが重複している、ファイルそれらのすべての開いているため、削除、**コンパイル**要素、および保存し、各ファイルを閉じます。

## <a name="setting-connection-strings"></a>接続文字列の設定

ほとんどの場合にする必要はありません接続文字列の変換を設定するため、発行プロファイルの接続文字列を指定することができます。 SQL Server Compact データベースをデプロイするときに例外があるし、Entity Framework Code First Migrations を使用して、移行先サーバー上のデータベースを更新します。 このシナリオでは、データベース スキーマを更新するため、サーバーで使用される追加の接続文字列を指定する必要があります。 この変換を設定するには、追加、 **&lt;connectionStrings&gt;** 要素の開始後すぐに**&lt;構成&gt;** 両方のタグ*Web.Test.config*と*Web.Production.config*ファイルに変換します。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform`属性は、この接続文字列に追加することを指定します、 *connectionStrings*要素で、デプロイされている*Web.config*ファイル。 (発行プロセスこの追加の接続文字列が自動的に作成すると、存在しない場合は既定では、 **providerName**属性に設定を取得`System.Data.SqlClient`、SQL Server Compact は機能しません。 接続文字列を手動で追加すると、保持、展開プロセスから誤ったプロバイダーの名前を持つ接続文字列の要素を作成します。)

指定したすべてのようになりましたが、 *Web.config* Contoso University アプリケーションのテストと運用環境に展開するために必要な変換です。 次のチュートリアルではするに代わってプロジェクト プロパティの設定を必要とする展開のセットアップ タスク。

## <a name="more-information"></a>説明

このチュートリアルで扱うトピックの詳細については、Web.config 変換のシナリオを参照してください。 [ASP.NET 配置コンテンツ マップ](https://msdn.microsoft.com/library/bb386521.aspx)します。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [次へ](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
