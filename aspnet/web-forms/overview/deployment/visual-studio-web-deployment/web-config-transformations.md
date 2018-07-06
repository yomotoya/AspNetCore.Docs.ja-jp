---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Visual Studio を使用して ASP.NET Web 展開: Web.config ファイルの変換 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 0a52530396efa3612d19817eeaa0498cffdac38f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842440"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Visual Studio を使用して ASP.NET Web 展開: Web.config ファイルの変換
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、変更するプロセスを自動化する方法、 *Web.config*ファイルの別の変換先の環境にデプロイするとき。 ほとんどのアプリケーション設定を持つ、 *Web.config*ファイルをアプリケーションの配置時に異なる必要があります。 このような変更のプロセスを自動化すると、展開すると面倒でエラーが発生するたびに手動で行うことがなくなります保持します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](troubleshooting.md)します。

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web デプロイのパラメーターと Web.config の変換

2 つの方法を変更するプロセスを自動化する*Web.config*ファイルの設定: [Web.config 変換](https://msdn.microsoft.com/library/dd465326.aspx)と[Web 配置パラメーター](https://msdn.microsoft.com/library/ff398068.aspx)します。 A *Web.config*変換ファイルには変更する方法を指定する XML マークアップが含まれています、 *Web.config*ファイルが展開されている場合にします。 特定のさまざまな変更がビルド構成と、特定の発行プロファイルを指定することができます。 既定のビルド構成はデバッグとリリースでは、およびカスタム ビルド構成を作成することができます。 発行プロファイルは、通常、インポート先の環境に対応します。 (でプロファイルの発行の詳細について説明します、[テスト環境として IIS への配置](deploying-to-iis.md)チュートリアルです)。

Web 配置パラメーターを使用して、さまざまな種類の設定に含まれる設定を含む、展開中に構成する必要がありますを指定することができます*Web.config*ファイル。 指定する際に*Web.config*ファイルの変更は、Web 配置パラメーターを設定するより複雑ですを展開するまでに設定する値がわからない場合に便利ですが。 など、エンタープライズ環境で作成、*展開パッケージ*と、運用環境でインストールするには、IT 部門で人にたってから、そのユーザーが、接続文字列やパスワードをしない」と入力します知っています。

このシナリオは、このチュートリアル シリーズでは、事前にわかってすべてを行う必要がありますが、 *Web.config*ファイル、Web 配置パラメーターを使用する必要はありません。 使用すると、ビルド構成によって異なりますが、使用された発行プロファイルによって異なるいくつかをいくつかの変換を構成します。

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Azure で Web.config の設定を指定します。

場合、 *Web.config*を変更するファイルの設定は、`<connectionStrings>`または`<appSettings>`要素中の変更を自動化するためのもう 1 つのオプションが Azure App Service で Web アプリを展開する場合展開します。 Azure で有効にするために必要な設定を入力することができます、**構成**web アプリの管理ポータル ページのタブ (下へスクロールして、**アプリ設定**と**接続文字列**セクション)。 プロジェクトを展開するときに Azure での変更が自動的に適用されます。 詳細については、次を参照してください。 [Windows Azure Web サイト: アプリケーション文字列と接続文字列の動作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)します。

## <a name="default-transformation-files"></a>既定の変換ファイル

**ソリューション エクスプ ローラー**、展開*Web.config*を参照してください、 *Web.Debug.config*と*Web.Release.config*ファイルの変換既定では 2 つの既定のビルド構成ごとに作成されます。

![Web.config_transform_files](web-config-transformations/_static/image1.png)

カスタム ビルド構成の変換ファイルを作成するには、Web.config ファイルを右クリックして**構成変換の追加**コンテキスト メニュー。 このチュートリアルでは、その必要はありませんし、任意のカスタム ビルド構成を作成していないために、メニュー オプションが無効にします。

後でを作成、テストでは、1 つずつ次の 3 つの複数変換ファイル ステージング、発行プロファイルを運用環境。 移行先の環境に依存するため、発行プロファイルの変換ファイルでの処理と設定の典型的な例は、テストと運用環境のさまざまなである WCF エンドポイントです。 これらに対応する発行プロファイルを作成した後に、発行に、後のチュートリアルでのプロファイルの変換ファイルを作成します。

## <a name="disable-debug-mode"></a>デバッグ モードを無効にします。

移行先の環境ではなく、ビルド構成に依存する設定の例、`debug`属性。 リリース ビルドでは、通常するデバッグが無効にデプロイする環境に関係なく。 そのため、既定では、Visual Studio プロジェクト テンプレート作成*Web.Release.config*を削除するコードを含むファイルの変換、`debug`属性を`compilation`要素。 ここでは、既定の*Web.Release.config*: でコードが含まれますがコメント アウトされているサンプル変換コード、だけでなく、`compilation`要素を削除する、`debug`属性。

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"`属性を指定する、`debug`から削除する属性、`system.web/compilation`要素で、デプロイされている*Web.config*ファイル。 これは、リリース ビルドをデプロイするたびに実行されます。

## <a name="limit-error-log-access-to-administrators"></a>管理者にエラー ログへのアクセスを制限します。

アプリケーションには、システムによって生成されたエラー ページの代わりに一般的なエラー ページが表示されますを使用して、アプリケーションの実行中にエラーがある場合、 [Elmah NuGet パッケージ](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)エラー ログとレポートします。 `customErrors`アプリケーション内の要素*Web.config*ファイルは、エラー ページを指定します。

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

エラー ページを表示するには、一時的に変更、`mode`の属性、`customErrors`要素から"RemoteOnly"を"オン"に、Visual Studio からアプリケーションを実行します。 エラーが発生するなど、無効な URL を要求することによって*Studentsxxx.aspx*します。 IIS で生成された「リソースが見つかりません」エラーではなくページの表示、 *GenericErrorPage.aspx*ページ。

![[エラー] ページ](web-config-transformations/_static/image2.png)

エラー ログを表示するとポート番号の後、URL 内のすべてを置換*elmah.axd* (たとえば、 `http://localhost:51130/elmah.axd`) し、Enter キーを押します。

![ELMAH ページ](web-config-transformations/_static/image3.png)

設定することを忘れないでください、`customErrors`要素を完了すると、"RemoteOnly"モードに戻ります。

開発用コンピューターには、エラー ログ ページには、セキュリティ リスクとなる運用環境での無料アクセスを許可すると便利です。 実稼働サイトでは、対象管理者にエラー ログへのアクセスを制限する承認規則を追加して、制限が機能することを確認するテスト環境やステージングもします。 これに属しているためと、リリース ビルドをデプロイするたびに実装するもう 1 つの変更は、そのため、 *Web.Release.config*ファイル。

オープン*Web.Release.config*し、新しい追加`location`終了直前の要素`configuration`タグを次に示します。

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform`属性値"Insert"の場合、この`location`に既存のすべての兄弟として追加する要素`location`内の要素、 *Web.config*ファイル。 (既に 1 つである`location`の承認を指定する要素の規則、**更新クレジット**ページです)。

これで正しくコーディングすることを確認する変換をプレビューできます。

**ソリューション エクスプ ローラー**、右クリック*Web.Release.config*  をクリック**プレビュー変換**します。

![変換 メニューをプレビューします。](web-config-transformations/_static/image4.png)

開発を示すページが開きます*Web.config*左と上のファイル、デプロイされている*Web.config*ファイルのようになります、右側の変更が強調表示されます。

![デバッグの変換のプレビュー](web-config-transformations/_static/image5.png)

![変換の場所のプレビュー](web-config-transformations/_static/image6.png)

(プレビューでは、いくつか追加の変更を記述していない変換を確認可能性があります通常の機能には影響しませんホワイト スペースの削除などがあります。)。

デプロイ後に、サイトをテストするときに、承認規則が有効であることを確認するテストも行います。

> [!NOTE] 
> 
> **セキュリティに関する注意**で実稼働アプリケーションでは、一般にエラーの詳細を表示または公共の場所にその情報を格納することはありません。 攻撃者は、エラー情報を使用して、サイトの脆弱性を検出することができます。 独自のアプリケーションで ELMAH を使用する場合は、セキュリティ上のリスクを最小限に抑える ELMAH を構成します。 このチュートリアルの例では ELMAH しない推奨される構成を検討してください。 アプリケーションはファイルを作成できる必要があるフォルダーを処理する方法を説明するために実装されている一例です。 詳細については、次を参照してください。 [ELMAH エンドポイントをセキュリティで保護する](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)します。


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>処理する設定プロファイル変換ファイルを発行します。

一般的なシナリオとして*Web.config*ファイルの設定を環境に展開するごとに変える必要があります。 たとえば、WCF サービスを呼び出すアプリケーションは、テストおよび運用環境で別のエンドポイントを必要があります。 Contoso University アプリケーションには、この種類の設定が含まれます。 この設定は、開発、テスト、運用などは、in、環境を示すサイトのページに表示されるインジケーターを制御します。 設定値は、アプリケーションが"(Dev)"を追加するかどうかを決定します。 または"(テスト)"メインの見出しに、 *Site.Master*マスター ページ。

![環境のインジケーター](web-config-transformations/_static/image7.png)

アプリケーションがステージング環境または運用環境で実行されているときに、環境のインジケーターは省略されます。

Contoso University の web ページで設定されている値の読み取り`appSettings`で、 *Web.config*ファイルで、アプリケーションが実行されているどのような環境を決定するには。

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

値は、"Test"をする、テスト環境で、ステージングと運用のためには、"Prod"する必要があります。

変換ファイルで次のコードでは、この変換を実装します。

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform`属性値"SetAttributes"では、この変換の目的で既存の要素の属性値を変更することを示します、 *Web.config*ファイル。 `xdt:Locator`属性値"Match(key)"では、変更する要素が、1 つであることを示しますが`key`属性と一致する、`key`ここで指定された属性。 のみその他の属性の`add`要素が`value`、変更される内容は、デプロイされているの*Web.config*ファイル。 エラーの原因を以下のコード、`value`の属性、 `Environment` `appSettings` "Test"に設定する要素、 *Web.config*が展開されているファイル。

この変換は、発行プロファイルの変換ファイルをまだ作成していないに属しています。 作成し、テスト、ステージング、運用環境の発行プロファイルを作成するときに、この変更を実装する変換ファイルを更新します。 行います、[を IIS に配置](deploying-to-iis.md)と[運用環境にデプロイ](deploying-to-production.md)チュートリアル。

> [!NOTE]
> この設定のため、 `<appSettings>` Azure アプリ サービスの「Web アプリにデプロイするときに、変換を指定するための別の方法がある要素、[を指定する Web.config 設定を Azure](#watransforms)で以前このトピックの「します。


## <a name="setting-connection-strings"></a>接続文字列の設定

既定の変換ファイルには、接続文字列を更新する方法を示す例が含まれますが、ほとんどの場合する必要はありません、接続文字列の変換を設定するため、発行プロファイルの接続文字列を指定することができます。 行います、[を IIS に配置](deploying-to-iis.md)と[運用環境にデプロイ](deploying-to-production.md)チュートリアル。

## <a name="summary"></a>まとめ

同じくらいお行ったようになりました*Web.config*変換前に、発行プロファイルを作成して、デプロイされた Web.config ファイルではどのプレビューを見てきました。

![変換の場所のプレビュー](web-config-transformations/_static/image8.png)

次のチュートリアルではするに代わってプロジェクト プロパティの設定を必要とする展開のセットアップ タスク。

## <a name="more-information"></a>説明

このチュートリアルで扱うトピックの詳細については、次を参照してください[デプロイ中に、対象の Web.config ファイルまたは app.config ファイルで設定を変更するのを使用して Web.config 変換](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms)での Web 配置コンテンツ マップ。Visual Studio および ASP.NET です。

> [!div class="step-by-step"]
> [前へ](preparing-databases.md)
> [次へ](project-properties.md)
