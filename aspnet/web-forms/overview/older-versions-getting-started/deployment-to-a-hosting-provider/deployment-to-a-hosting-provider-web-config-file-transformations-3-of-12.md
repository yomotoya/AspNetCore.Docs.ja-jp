---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: "SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します Web.Config ファイルの変換、3/12 |。Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: ed78b55d2b0315cf428f137c56ad85b29a95e1c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションを配置します Web.Config ファイルの変換、3/12。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルを変更するプロセスを自動化する方法を示します、 *Web.config*ファイルの別のインストール先の環境に展開するときにします。 ほとんどのアプリケーションに設定がある、 *Web.config*ファイルをアプリケーションの配置時に異なる必要があります。 これらの変更のプロセスを自動化する常に展開すると、これは面倒では、エラーが発生するたびに手動で行うことがなくなります。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web.config 変換ではなく Web 展開パラメーター

2 つの方法を変更するプロセスを自動化する*Web.config*ファイルの設定: [Web.config 変換](https://msdn.microsoft.com/library/dd465326.aspx)と[Web 展開パラメーター](https://msdn.microsoft.com/library/ff398068.aspx)です。 A *Web.config*変換ファイルには変更する方法を指定する XML マークアップが含まれています、 *Web.config*ファイルが展開されるとします。 特定のさまざまな変更がビルド構成と特定のプロファイルの発行を指定できます。 既定のビルド構成には、デバッグおよびリリースでは、およびカスタム ビルド構成を作成することができます。 通常、発行プロファイルは、送信先の環境に対応しています。 (でプロファイルの発行の詳細を学習、[テスト環境として IIS に展開する](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)チュートリアルです)。

Web デプロイのパラメーターは、内にある設定を含む、展開時に構成する必要がある設定の多くのさまざまな種類を指定するために使用できます*Web.config*ファイル。 指定するために使用時に*Web.config*ファイルの変更、Web デプロイ パラメーターをセットアップより複雑なを展開するまでに設定される値が認識していない場合に便利です。 など、エンタープライズ環境で作成、*展開パッケージ*、実稼働環境でインストールするには、IT 部門の担当者に付与し、そのユーザーが接続文字列またはそうしないとパスワードを入力できるように、知っています。

このチュートリアルをカバーするシナリオではわかってを実行することはすべて、 *Web.config*ファイル、Web Deploy のパラメーターを使用する必要はありません。 このオプションを使用すると、ビルド構成に応じて異なるし、使用された発行プロファイルによって異なるいくつかされる一部の変換を構成します。

## <a name="creating-transformation-files-for-publish-profiles"></a>発行プロファイルの変換ファイルの作成

**ソリューション エクスプ ローラー**、展開*Web.config*を表示する、 *web.debug.config となります*と*Web.Release.config*ファイルの変換既定では 2 つの既定のビルド構成ごとに作成されます。

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Web.config ファイルを右クリックしてカスタム ビルド構成の変換ファイルを作成することができます**構成変換の追加**コンテキスト メニューから、このチュートリアルを行うには不要です。

必要以上の 2 つの変換ファイル、ビルド構成ではなく、配置先に関連する変更を構成するためです。 設定のこの種の典型的な例は、実稼働環境とテストのさまざまなである WCF エンドポイントです。 作成後のチュートリアルで発行する必要がありますので、テストと実稼働環境でをという名前のプロファイル、 *Web.Test.config*ファイルと*Web.Production.config*ファイル。

発行プロファイルに関連付けられている変換ファイルを手動で作成する必要があります。 **ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、選択、 **Windows エクスプ ローラーでフォルダーを開く**です。

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

**Windows エクスプ ローラー**、select、 *Web.Release.config*ファイル、ファイルをコピーし、その 2 つのコピーを貼り付けます。 これらのコピーの名前を変更*Web.Production.config*と*Web.Test.config*、終値**Windows エクスプ ローラー**です。

**ソリューション エクスプ ローラー**をクリックして**更新**に新しいファイルを参照してください。

選択、新しいファイルを右クリックしをクリック**プロジェクトに含める**コンテキスト メニュー。

![プロジェクトにテストおよび運用の構成ファイルを含める](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

これらのファイルが展開されていることを防ぐために選択して **ソリューション エクスプ ローラー**、し、**プロパティ**ウィンドウの変更、**ビルド アクション**からプロパティ**コンテンツ**に**None**です。 (ビルド構成に基づく変換ファイルは自動的に回避から展開されます。)

入力する準備が整いました*Web.config*変換を*Web.config*変換ファイル。

## <a name="limiting-error-log-access-to-administrators"></a>管理者にエラー ログへのアクセスを制限します。

アプリケーションの実行中にエラーがある場合は、アプリケーションには、システムによって生成されたエラー ページの代わりに一般的なエラー ページが表示されます。 を使用して[Elmah NuGet パッケージ](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)エラー ログとレポート。 `customErrors`内の要素、 *Web.config*ファイルは、エラー ページを指定します。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

エラー ページを表示するには、一時的に変更、`mode`の属性、`customErrors`要素に「オン」には、"RemoteOnly"と Visual Studio からアプリケーションを実行します。 など、無効な URL を要求することでエラーが発生する*Studentsxxx.aspx*です。 IIS で生成された「ページが見つかりません」エラー ページの代わりに、 *GenericErrorPage.aspx*ページ。

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

エラー ログを表示するとポート番号の後、URL 内のすべてを置換*elmah.axd* (スクリーン ショットの例で`http://localhost:51130/elmah.axd`) し、Enter キーを押します。

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

忘れずに設定、`customErrors`要素を完了すると、"RemoteOnly"モードに戻します。

開発用コンピューターでは、エラー ログ ページには、セキュリティ リスクになる実稼働環境で自由にアクセスを許可すると便利です。 実稼働サイトで変換を構成することによって、管理者にエラー ログへのアクセスを制限する承認規則を追加することができます、 *Web.Production.config*ファイル。

開いている*Web.Production.config*し、新しい追加`location`要素の開始後すぐに`configuration`タグを次に示すようにします。 (だけ追加することを確認してください、`location`要素と周辺のいくつかのコンテキストを提供することのみここに表示されるマークアップされません)。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform` "Insert"の属性値と、この`location`既存に兄弟として追加する要素`location`内の要素、 *Web.config*ファイル。 (が既に 1 つ`location`承認を指定する要素の規則を**更新クレジット**ページです)。展開後に、実稼働サイトをテストするときに、この承認規則が有効であることを確認するテストをします。

このコードを追加する必要はありませんようにテスト環境でエラー ログへのアクセスを制限する必要はありません、 *Web.Test.config*ファイル。

> [!NOTE] 
> 
> **セキュリティに関する注意**ことはありません、実稼働アプリケーションで公開するエラーの詳細を表示または公共の場所にその情報を格納します。 攻撃者は、エラー情報を使用して、サイトの脆弱性を検出することができます。 独自のアプリケーションで ELMAH を使用する場合は、セキュリティ上のリスクを最小限に抑える ELMAH を構成する方法を調査することを確認してあります。 このチュートリアルの例では ELMAH は見なされません構成はお勧めします。 これは、アプリケーションはファイルを作成できる必要があるフォルダーを処理する方法を説明するために選択した例です。


## <a name="setting-an-environment-indicator"></a>インジケーターを環境の設定

一般的なシナリオは、こと*Web.config*ファイルの各環境に展開することで別にする必要がある設定をします。 たとえば、WCF サービスを呼び出すアプリケーションは、テスト環境や実稼働環境で別のエンドポイントを必要があります。 Contoso 大学アプリケーションには、この種類の設定が含まれます。 この設定は、開発、テスト、運用などは、in、環境を示すサイトのページに表示されるインジケーターを制御します。 設定値は、アプリケーションが"(Dev)"を追加するかどうかを決定するか、「(テスト)」のメインの見出しに、 *Site.Master*マスター ページ。

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

環境のインジケーターは、実稼働環境でアプリケーションが実行されている場合は省略されます。

Contoso 大学 web ページで設定されている値の読み取り`appSettings`で、 *Web.config*でアプリケーションが実行されているどのような環境を判断するためのファイル。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

値は、"Test"テスト環境では、"Prod"で、運用環境にする必要があります。

開いている*Web.Production.config*を追加し、`appSettings`の開始タグの直前の要素、`location`要素の前に追加します。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform`属性値"SetAttributes"では、この変換の目的で既存の要素の属性値を変更することを示します、 *Web.config*ファイル。 `xdt:Locator`属性"Match(key)"が、その要素を変更するのには、いずれかを示す値を`key`属性と一致する、`key`属性をここで指定します。 のみの他の属性の`add`要素は`value`はどのような変更は、展開済みで*Web.config*ファイル。 このコードにより、`value`の属性、 `Environment` `appSettings` "Prod"に設定する要素、 *Web.config*実稼働環境に展開されているファイル。

次に、同じ変更を適用*Web.Test.config*セット以外のファイル、 `value` "Prod"の代わりに"Test"にします。 完了したら、ときに、 `appSettings` 」の「 *Web.Test.config*は次の例のようになります。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>デバッグ モードを無効にします。

リリース ビルドでは、たくないデバッグを有効にする環境に関係なくを展開しています。 既定では、 *Web.Release.config*変換ファイルがコードを削除すると自動的に作成、`debug`属性から、`compilation`要素。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform`属性の原因、`debug`除外する属性から、デプロイされている*Web.config*リリース ビルドを配置するたびにファイルします。

この同じ変換は、テストや実稼働変換ファイル リリース変換ファイルをコピーして作成するためです。 重複しているファイルは必要ありません、開いているためそれらのすべてのファイル、削除する、**コンパイル**要素、および保存し、各ファイルを閉じます。

## <a name="setting-connection-strings"></a>接続文字列の設定

ほとんどの場合する必要はありません、接続文字列の変換を設定するため、発行プロファイルで接続文字列を指定することができます。 SQL Server Compact データベースを配置するときに例外があるし、Entity Framework Code First Migrations を使用して、移行先サーバー上のデータベースを更新します。 このシナリオでは、データベース スキーマを更新するため、サーバーで使用される追加の接続文字列を指定する必要があります。 この変換をセットアップするには追加、  **&lt;connectionStrings&gt;** 要素の開始後すぐに**&lt;構成&gt;**両方でタグ*Web.Test.config*と*Web.Production.config*ファイルに変換します。

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform`属性は、この接続文字列に追加することを指定します、 *connectionStrings* 、展開内の要素*Web.config*ファイル。 (発行プロセスこの追加の接続文字列が自動的に作成すると、それが存在しない場合の既定では、 **providerName**属性に設定を取得`System.Data.SqlClient`、SQL Server Compact は機能しません。 接続文字列を手動で追加すると、しておく、展開プロセスを間違ったプロバイダー名を持つ接続文字列の要素を作成します。)

指定したすべてのようになりましたが、 *Web.config*をテストする Contoso 大学アプリケーションおよび運用環境の配置に必要な変換です。 次のチュートリアルではありますを処理するプロジェクト プロパティの設定を必要とする展開のセットアップ タスク。

## <a name="more-information"></a>説明

このチュートリアルで説明されているトピックの詳細については、Web.config 変換シナリオを参照してください。 [ASP.NET 展開のコンテンツ マップ](https://msdn.microsoft.com/library/bb386521.aspx)です。

>[!div class="step-by-step"]
[前へ](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
[次へ](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
