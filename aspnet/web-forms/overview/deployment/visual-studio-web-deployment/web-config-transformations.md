---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Visual Studio を使用した ASP.NET Web 展開: Web.config ファイルの変換 |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 77ed0d8b2fe85adb009a3f4759030b7fba8fb9d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880506"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Visual Studio を使用した ASP.NET Web 展開: Web.config ファイルの変換
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルを変更するプロセスを自動化する方法を示します、 *Web.config*ファイルの別のインストール先の環境に展開するときにします。 ほとんどのアプリケーションに設定がある、 *Web.config*ファイルをアプリケーションの配置時に異なる必要があります。 これらの変更のプロセスを自動化する常に展開すると、これは面倒では、エラーが発生するたびに手動で行うことがなくなります。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](troubleshooting.md)です。

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web Deploy のパラメーターと Web.config 変換

2 つの方法を変更するプロセスを自動化する*Web.config*ファイルの設定: [Web.config 変換](https://msdn.microsoft.com/library/dd465326.aspx)と[Web 展開パラメーター](https://msdn.microsoft.com/library/ff398068.aspx)です。 A *Web.config*変換ファイルには変更する方法を指定する XML マークアップが含まれています、 *Web.config*ファイルが展開されるとします。 特定のさまざまな変更がビルド構成と特定のプロファイルの発行を指定できます。 既定のビルド構成には、デバッグおよびリリースでは、およびカスタム ビルド構成を作成することができます。 通常、発行プロファイルは、送信先の環境に対応しています。 (でプロファイルの発行の詳細を学習、[テスト環境として IIS に展開する](deploying-to-iis.md)チュートリアルです)。

Web デプロイのパラメーターは、内にある設定を含む、展開時に構成する必要がある設定の多くのさまざまな種類を指定するために使用できます*Web.config*ファイル。 指定するために使用時に*Web.config*ファイルの変更、Web デプロイ パラメーターをセットアップより複雑なを展開するまでに設定される値が認識していない場合に便利です。 など、エンタープライズ環境で作成、*展開パッケージ*、実稼働環境でインストールするには、IT 部門の担当者に付与し、そのユーザーが接続文字列またはそうしないとパスワードを入力できるように、知っています。

このチュートリアルのシリーズをカバーするシナリオでは、事前にわかってを実行することはすべて、 *Web.config*ファイル、Web Deploy のパラメーターを使用する必要はありません。 このオプションを使用すると、ビルド構成に応じて異なるし、使用された発行プロファイルによって異なるいくつかされる一部の変換を構成します。

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Azure で Web.config 設定の指定

場合、 *Web.config*を変更するファイルの設定が、`<connectionStrings>`または`<appSettings>`要素中の変更を自動化するための別のオプションがある Azure App service Web アプリに配置する場合展開します。 Azure で有効にするために使用する設定を入力することができます、**構成**web アプリの管理ポータル ページのタブ (まで下にスクロール、**アプリ設定**と**接続文字列**セクション)。 プロジェクトを展開するときに Azure では、変更が自動的に適用します。 詳細については、次を参照してください。 [Windows Azure Web サイト: どのようにアプリケーション文字列と接続文字列作業](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)です。

## <a name="default-transformation-files"></a>既定の変換ファイル

**ソリューション エクスプ ローラー**、展開*Web.config*を表示する、 *web.debug.config となります*と*Web.Release.config*ファイルの変換既定では 2 つの既定のビルド構成ごとに作成されます。

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Web.config ファイルを右クリックしてカスタム ビルド構成の変換ファイルを作成することができます**構成変換の追加**コンテキスト メニュー。 このチュートリアルを実行する必要はありませんし、任意のカスタム ビルド構成を作成していないために、メニュー オプションが無効にします。

後でを作成 3 つ以上変換ファイル、テストは、1 つずつ、ステージングし、発行プロファイルを実稼働します。 展開先の環境に依存しているため、発行プロファイルの変換ファイルでの処理と、設定の典型的な例は運用環境とテストの異なる WCF エンドポイントです。 これらに対応する発行プロファイルを作成した後に、発行に、後のチュートリアルでのプロファイル変換ファイルを作成します。

## <a name="disable-debug-mode"></a>デバッグ モードを無効にします。

送信先の環境ではなく、ビルド構成に依存している設定の例、`debug`属性。 リリース ビルドでは、通常するデバッグが無効に配置する環境に関係なく。 したがって、既定では、Visual Studio プロジェクト テンプレートを作成*Web.Release.config*ファイルを削除するコードを変換、`debug`属性から、`compilation`要素。 既定値を次に示します*Web.Release.config*: でコードが含まれますがコメント アウトされているサンプル変換コード、に加えて、`compilation`要素を削除する、`debug`属性。

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"`属性を指定する、`debug`から削除する属性、 `system.web/compilation` 、展開内の要素*Web.config*ファイル。 これは、リリース ビルドを配置するたびに実行されます。

## <a name="limit-error-log-access-to-administrators"></a>管理者にエラー ログへのアクセスを制限します。

アプリケーションの実行中にエラーがある場合は、アプリケーションには、システムによって生成されたエラー ページの代わりに一般的なエラー ページが表示されます。 を使用して、 [Elmah NuGet パッケージ](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)エラー ログとレポート。 `customErrors`アプリケーション内の要素*Web.config*ファイルは、エラー ページを指定します。

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

エラー ページを表示するには、一時的に変更、`mode`の属性、`customErrors`要素に「オン」には、"RemoteOnly"と Visual Studio からアプリケーションを実行します。 など、無効な URL を要求することでエラーが発生する*Studentsxxx.aspx*です。 IIS で生成された「リソースが見つかりません」エラーではなくページで、表示、 *GenericErrorPage.aspx*ページ。

![[エラー] ページ](web-config-transformations/_static/image2.png)

エラー ログを表示するとポート番号の後、URL 内のすべてを置換*elmah.axd* (たとえば、 `http://localhost:51130/elmah.axd`) し、Enter キーを押します。

![ELMAH ページ](web-config-transformations/_static/image3.png)

忘れずに設定、`customErrors`要素を完了すると、"RemoteOnly"モードに戻します。

開発用コンピューターでは、エラー ログ ページには、セキュリティ リスクになる実稼働環境で自由にアクセスを許可すると便利です。 運用サイトの対象を管理者にエラー ログへのアクセスを制限する承認規則を追加し、制限が機能することを確認するテストともステージング環境にします。 これに属しているためと、リリース ビルドを配置するたびに実装するその他の変更は、そのため、 *Web.Release.config*ファイル。

開いている*Web.Release.config*し、追加、新しい`location`終了の直前の要素`configuration`タグを次に示すようにします。

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` "Insert"の属性値と、この`location`既存に兄弟として追加する要素`location`内の要素、 *Web.config*ファイル。 (が既に 1 つ`location`承認を指定する要素の規則を**更新クレジット**ページです)。

これで正しくコーディングすることを確認する変換をプレビューできます。

**ソリューション エクスプ ローラー**を右クリックして*Web.Release.config*  をクリック**プレビュー変換**です。

![[プレビュー] メニューの変換](web-config-transformations/_static/image4.png)

開発を示すページが表示*Web.config*左と上のファイル、配置された*Web.config*ファイルのようになります、右側の変更が強調表示されます。

![デバッグのトランス フォームのプレビュー](web-config-transformations/_static/image5.png)

![場所のトランス フォームのプレビュー](web-config-transformations/_static/image6.png)

(プレビューで見つかる場合がありますを記述したいくつか追加の変更を変換しますこれらの機能には影響しません空白文字を削除する場合がよく。)。

展開後に、サイトをテストするときに、承認規則が有効であることを確認するテストも行います。

> [!NOTE] 
> 
> **セキュリティに関する注意**ことはありません、実稼働アプリケーションで公開するエラーの詳細を表示または公共の場所にその情報を格納します。 攻撃者は、エラー情報を使用して、サイトの脆弱性を検出することができます。 ELMAH 独自のアプリケーションを使用する場合は、セキュリティ上のリスクを最小限に抑える ELMAH を構成します。 このチュートリアルの例では ELMAH は見なされません構成はお勧めします。 これは、アプリケーションはファイルを作成できる必要があるフォルダーを処理する方法を説明するために選択した例です。 詳細については、次を参照してください。 [ELMAH エンドポイントをセキュリティで保護する](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)です。


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>処理されます設定プロファイル変換ファイルを発行します。

一般的なシナリオは、こと*Web.config*ファイルの各環境に展開することで別にする必要がある設定をします。 たとえば、WCF サービスを呼び出すアプリケーションは、テスト環境や実稼働環境で別のエンドポイントを必要があります。 Contoso 大学アプリケーションには、この種類の設定が含まれます。 この設定は、開発、テスト、運用などは、in、環境を示すサイトのページに表示されるインジケーターを制御します。 設定値は、アプリケーションが"(Dev)"を追加するかどうかを決定するか、「(テスト)」のメインの見出しに、 *Site.Master*マスター ページ。

![環境のインジケーター](web-config-transformations/_static/image7.png)

環境のインジケーターは、アプリケーションがステージングまたは運用環境で実行されている場合は省略されます。

Contoso 大学 web ページで設定されている値の読み取り`appSettings`で、 *Web.config*でアプリケーションが実行されているどのような環境を判断するためのファイル。

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

値がステージングと運用環境には、"Prod"して、テスト環境で"Test"をする必要があります。

変換ファイルに次のコードでは、この変換を実装します。

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform`属性値"SetAttributes"では、この変換の目的で既存の要素の属性値を変更することを示します、 *Web.config*ファイル。 `xdt:Locator`属性"Match(key)"が、その要素を変更するのには、いずれかを示す値を`key`属性と一致する、`key`属性をここで指定します。 のみの他の属性の`add`要素は`value`はどのような変更は、展開済みで*Web.config*ファイル。 原因を次のコード、`value`の属性、 `Environment` `appSettings` "Test"に設定する要素、 *Web.config*に展開されているファイル。

この変換は、まだ作成されている発行プロファイル変換ファイルに属しています。 作成し、テスト、ステージング、および実稼働環境の発行プロファイルを作成するときに、この変更を実装する変換ファイルを更新します。 実行される、 [IIS に配置](deploying-to-iis.md)と[実稼働環境に展開](deploying-to-production.md)チュートリアルです。

> [!NOTE]
> この設定がであるため、`<appSettings>`要素がある Azure App Service 参照では Web アプリに配置するときに、変換を指定するための別の方法として[Azure で指定する Web.config 設定](#watransforms)で以前このトピックでです。


## <a name="setting-connection-strings"></a>接続文字列の設定

既定の変換ファイルには、接続文字列を更新する方法を示す例が含まれている、ほとんどの場合、不要、接続文字列の変換を設定する、発行プロファイルで接続文字列を指定できるためです。 実行される、 [IIS に配置](deploying-to-iis.md)と[実稼働環境に展開](deploying-to-production.md)チュートリアルです。

## <a name="summary"></a>まとめ

今すぐ行うようになると*Web.config*が配置された Web.config ファイルで対象のプレビューを確認した発行プロファイルを作成する前に変換します。

![場所のトランス フォームのプレビュー](web-config-transformations/_static/image8.png)

次のチュートリアルではありますを処理するプロジェクト プロパティの設定を必要とする展開のセットアップ タスク。

## <a name="more-information"></a>説明

このチュートリアルで説明されているトピックの詳細については、次を参照してください[設定を変更する対象の Web.config ファイルまたは app.config ファイルでの展開時に使用して Web.config 変換](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms)で、Web コンテンツの展開マップ。Visual Studio と ASP.NET です。

> [!div class="step-by-step"]
> [前へ](preparing-databases.md)
> [次へ](project-properties.md)
