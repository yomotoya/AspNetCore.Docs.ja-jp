---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: ソース管理 (Azure で現実世界のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 5df863762523b62759bb4f7849ca2635e5241b0a
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577796"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>ソース管理 (Azure で現実世界のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson]((https://twitter.com/RickAndMSFT))、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。

ソース管理は、すべてのクラウド開発プロジェクト、チーム環境だけでなくに不可欠です。 ソース コードの編集を検討するでしょうかも、Word ドキュメントを元に戻す関数と自動バックアップ、およびソース管理なしでは、問題が発生したときにさらに多くの時間を乗り越えるのにプロジェクト レベルでこれらの関数。 クラウド ソースの管理サービスでされなく複雑のセットアップについて心配する必要があるし、最大 5 ユーザーまで無料の Azure リポジトリ ソース管理を使用することができます。

この章の最初の部分では、留意する 3 つのキーのベスト プラクティスについて説明します。

- [自動化スクリプトをソース コードとして扱う](#scripts)とバージョン、アプリケーション コードと共ににします。
- [機密情報を確認しない](#secrets)(資格情報などの機密データ) のソース コード リポジトリに格納します。
- [ソース ブランチを設定](#devops)DevOps ワークフローを有効にします。

」の章の残りの部分では、Visual Studio、Azure、および Azure リポジトリでこれらのパターンの一部のサンプル実装を示します。

- [スクリプトを Visual Studio でソース管理に追加します。](#vsscripts)
- [Azure での機密データを格納します。](#appsettings)
- [Visual Studio と Azure リポジトリに Git を使用して、](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>自動化スクリプトをソース コードとして扱う

クラウド プロジェクトでの作業と処理を頻繁に変更しているお客様によって報告された問題に迅速に対応できるようにします。 説明したように、自動化スクリプトを使用してでは迅速な対応、[自動化すべて](automate-everything.md)」の章。 すべてのスクリプトを使用して、環境を作成するはなど、アプリケーションのソース コードと同期する必要があります。 スケールをデプロイします。

スクリプトのコードでの同期を保つには、ソース管理システムに保存します。 後に変更をロールバックまたは実稼働コードのコード開発とは別にクイック修正を加える必要が生じた場合は、追跡設定が変更されたか、どのチーム メンバーが必要なバージョンのコピーを保持している時間を浪費する必要はありません。 必要なスクリプトは、必要なすべてのチーム メンバーを同じスクリプトを操作することを保証しているコード ベースとの同期を保証しています。 テストおよび運用環境または新しい機能の開発に修正プログラムのデプロイを自動化する必要があるかどうか、更新する必要があるコードの適切なスクリプトがあります。

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>シークレットをチェックインしません。

ソース コードのリポジトリが通常パスワードなどの機密データのための場所が適切にセキュリティで保護するには人が多すぎますアクセスできます。 スクリプトは、パスワードなどの機密情報に依存する場合は、これらの設定、ソース コードでは保存されません、および他の場所、シークレットを格納するためパラメーター化します。

などが含まれているファイルをダウンロードする Azure では、発行プロファイルの作成を自動化するために設定を発行します。 これらのファイルは、ユーザー名とパスワードを Azure サービスを管理する権限が含まれます。 これを使用する場合を作成する方法と、発行プロファイル、およびをソース管理にこれらのファイルをチェックインする場合は、それらのユーザー名とパスワードを表示をリポジトリにアクセスできる人物によることができます。 暗号化されているので、発行プロファイル自体で、パスワードに格納することに安全にことができます、 *. >.pubxml.user*ファイルを既定では、ソース管理には含まれていません。

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>構造のソース ブランチを DevOps ワークフローを容易にする

リポジトリで分岐を実装する方法、新機能の開発し、運用環境での問題を修正する能力に影響します。 大量のメディア サイズのチームの使用を調整するパターンを次に示します。

![ソース ブランチ構造](source-control/_static/image1.png)

マスター ブランチには、実稼働環境では、コードが常と一致します。 マスターの下にある分岐は、開発ライフ サイクルのさまざまな段階に対応します。 Development 分岐には新機能を実装します。 小規模なチームの可能性がありますだけマスターと開発が多くの場合、ユーザーが開発とマスター間でのステージング環境の分岐があることをお勧めします。 ステージングを使用して、最終的な統合が更新プログラムが運用環境に移動する前にテストすることができます。

大きなチームのある可能性があります。 新しい機能ごとに別のブランチ小規模なチーム everyone development 分岐にチェックインするがあります。

機能 A が準備完了のするマージ時に、各機能の分岐がある場合、ソース コードの変更を開発に分岐および他の機能の分岐にします。 このソース コードのマージ プロセスは時間がかかり、できるし、個別機能しながら、その作業を避けるためには、一部のチームと呼ばれる別の方法を実装*[フィーチャー トグル](http://en.wikipedia.org/wiki/Feature_toggle)* (ともいう*機能フラグ*)。 つまり、同じツリーには、すべての機能のすべてのコードが有効にするか、コードでスイッチを使用して各機能を無効にします。 たとえば、機能 A は Fix It アプリ タスクの新しいフィールドで、機能 B は、キャッシュ機能を追加します。 両方の機能のコードは、development 分岐であることができますが、アプリが表示、新しいフィールドの変数が true に設定されている場合のみキャッシュを使用する、別の変数が設定されている場合は true にします。 機能 A に昇格する準備ができていない場合、機能 B の準備がすべての機能 A スイッチを使用して実稼働環境にコードを昇格することができ、機能 B をオンにします。 機能 A を終了し、昇格して後で、すべてでないソース コードをマージします。

機能の分岐または切り替えを使用するかどうかこのような分岐構造では、アジャイルかつ反復的な方法で開発運用環境にコードをフローすることができます。

この構造体では、お客様のフィードバックに迅速に対応することもできます。 運用環境にクイック修正を加える必要がある場合も行えますを効率的にアジャイルな手法で。 Master またはステージング環境から分岐を作成して、マスターにマージする準備ができ次第印および下矢開発と機能の分岐にします。

![修正プログラムの分岐](source-control/_static/image2.png)

なしの運用と開発分岐には、その分離を使用したこのような分岐構造、運用環境の問題に配置する、運用環境の修正と共に新しい機能のコードを昇格することの位置にします。 新しい機能のコードを徹底的にテストと準備の運用できない可能性があり、多くの準備ができていない変更をバックアップする作業を実行する必要があります。 または、変更をテストし、それらをデプロイする準備が取得するには、修正プログラムを遅延する必要があります。

次に、Visual Studio、Azure、および Azure リポジトリにこれら 3 つのパターンを実装する方法の例を確認します。 これらは、詳細な手順の方法を操作を行います it について; ではなく、例すべての必要なコンテキストを提供の詳細については、次を参照してください。、[リソース](#resources)章の最後のセクション。

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>スクリプトを Visual Studio でソース管理に追加します。

(プロジェクトがソース管理にある場合)、Visual Studio ソリューション フォルダーに含めることで、Visual Studio でソース管理にスクリプトを追加できます。 これを行う 1 つの方法を次に示します。

ソリューション フォルダー内のスクリプトのフォルダーを作成 (がある同じフォルダー、 *.sln*ファイル)。

![Automation フォルダー](source-control/_static/image3.png)

スクリプト ファイルをフォルダーにコピーします。

![フォルダーの内容の自動化](source-control/_static/image4.png)

Visual Studio でプロジェクトをソリューション フォルダーを追加します。

![新しいソリューション フォルダーのメニューの選択](source-control/_static/image5.png)

スクリプト ファイルをソリューション フォルダーに追加します。

![既存の項目 メニューの選択を追加します。](source-control/_static/image6.png)

![[既存項目の追加] ダイアログ ボックス](source-control/_static/image7.png)

スクリプト ファイルがプロジェクトに含まれるようになりましたし、ソース管理に対応するソース コードの変更とそのバージョンの変更が追跡します。

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Azure での機密データを格納します。

Azure Web サイトでアプリケーションを実行する場合でソース管理の資格情報を格納しないようにする方法の 1 つが、代わりに Azure に格納します。

たとえば、Fix It アプリケーションは、Web.config ファイル 2 つの接続文字列を運用環境と Azure ストレージ アカウントにアクセスできるように、キーのパスワードを持つに格納します。

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

これらの設定の実際の運用時の値を配置するかどうか、 *Web.config*ファイルに配置する場合や、 *Web.Release.config*デプロイ時に、それらを挿入する Web.config の変換を構成するファイルこれらは、ソース リポジトリに格納します。 発行プロファイルを運用環境にデータベースの接続文字列を入力する場合で、パスワードになります、 *.pubxml*ファイル。 (除外する可能性があります、 *.pubxml* 、ソース管理からファイルが、他のすべての展開設定を共有する利点が失われます)。

ための代替を提供、 **appSettings**および接続文字列のセクションでは、 *Web.config*ファイル。 関連部分を次に示します、**構成**Azure 管理ポータルでの web サイト タブ。

![appSettings、connectionStrings ポータル](source-control/_static/image8.png)

この web サイトとアプリケーションの実行時にプロジェクトを配置するときに Azure に保管されている任意の値は、Web.config ファイルには、どのような値をオーバーライドします。

管理ポータルまたはスクリプトを使用して、Azure でこれらの値を設定できます。 環境の作成の自動化スクリプトで見た、[自動化すべて](automate-everything.md)章、Azure SQL Database を作成、ストレージと SQL データベースの接続文字列を取得および web サイトの設定にこれらのシークレットを格納します。

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

実際の値をソース リポジトリに永続化されないように、スクリプトがパラメーター化されることを確認します。

開発環境でローカルに実行すると、アプリを読み取り、ローカルの Web.config ファイルとの接続文字列で SQL Server の LocalDB データベースを指して、*アプリ\_データ*web プロジェクトのフォルダー。 Azure でアプリを実行すると、アプリが Web.config ファイルからこれらの値を読み取ろうと、失意こと確認し、使用は、格納されているとは限りません実際に Web.config ファイルで、Web サイトの値です。

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-azure-devops"></a>Visual Studio および Azure DevOps で Git を使用して、

前に示した DevOps 分岐構造を実装するのに任意のソース管理の環境を使用できます。 分散チームの[分散バージョン コントロール システム](http://en.wikipedia.org/wiki/Distributed_revision_control)(DVCS) が最も適切に機能があります。 他のチームの、[システムを集中管理](http://en.wikipedia.org/wiki/Revision_control)より適切に動作可能性があります。

[Git](http://git-scm.com/)人気のある分散型バージョン コントロール システムです。 ソース管理に Git を使用するときに、ローカル コンピューターに、そのすべての履歴を含むリポジトリの完全なコピーを用意します。 多くの人は、こと簡単だから、ネットワークに接続していない--を続行できるときの作業を続行するには、コミット、ロールバック、作成し、分岐を切り替えるしなどを使用します。 ネットワークに接続している場合でもブランチを作成し、すべてのものがローカル分岐を切り替えた方が手軽な容易になります。 他の開発者に影響を与えず、ローカル コミットとロールバックを実行することもできます。 サーバーに送信する前にコミットをバッチ処理できます。

[Azure リポジトリ](/azure/devops/repos/index?view=vsts)両方を提供[Git](/azure/devops/repos/git/?view=vsts)と[Team Foundation バージョン管理](/azure/devops/repos/tfvc/index?view=vsts)(TFVC; ソース管理を一元化する)。 Azure DevOps の概要[ここ](https://app.vsaex.visualstudio.com/signup)します。

Visual Studio 2017 には、組み込みファースト クラスにはが含まれています。 [Git サポート](https://msdn.microsoft.com/library/hh850437.aspx)します。 ここでは、簡単に、その動作のデモです。

プロジェクトを Visual Studio で開き、ソリューションを右クリックして**ソリューション エクスプ ローラー**を選び、**ソリューションをソース管理に追加**します。

![ソリューションをソース管理に追加します。](source-control/_static/image9.png)

Visual Studio では、TFVC (集中型バージョン コントロール) または Git を使用するかどうかを要求します。

![ソース管理を選択します。](source-control/_static/image10.png)

Git を選択し、をクリックして**OK**、Visual Studio は、ソリューション フォルダーに新しいローカル Git リポジトリを作成します。 新しいリポジトリにはファイルがありません。 まだGit コミットの手順を実行して、リポジトリに追加することがあります。 ソリューションを右クリックして**ソリューション エクスプ ローラー**、 をクリックし、**コミット**します。

![コミット](source-control/_static/image11.png)

Visual Studio が自動的にすべてのコミットのプロジェクト ファイルのステージングし、でそれらが一覧表示**チーム エクスプ ローラー**で、**含まれる変更**ウィンドウ。 (いくつかのコミットに含める必要がなかったなら、それを選択できますし、右クリックおよびクリック**除外**)。

![チーム エクスプローラー](source-control/_static/image12.png)

コミットのコメントを入力し、クリックして**コミット**、および Visual Studio は、コミットを実行し、コミット ID を表示します

![チーム エクスプ ローラーの変更](source-control/_static/image13.png)

今すぐ内容と異なるリポジトリにあるように、いくつかのコードを変更する場合は、相違点を簡単に確認できます。 右クリックして、変更したファイルを選択します**Unmodified と比較**、コミットされていない変更内容を比較表示を取得します。

![未変更のものと比較します。](source-control/_static/image14.png)

![差分の表示の変更](source-control/_static/image15.png)

どのような変更を行っているし、チェックインを簡単に確認できます。

分岐を作成する必要があります – Visual Studio ですぎるを実行できるとします。 **チーム エクスプ ローラー**、 をクリックして**新しいブランチ**します。

![チーム エクスプ ローラーの新しいブランチ](source-control/_static/image16.png)

ブランチ名を入力し、をクリックして**分岐の作成**、選択した場合と**ブランチのチェック アウト**、Visual Studio が自動的に新しいブランチを確認します。

![チーム エクスプ ローラーの新しいブランチ](source-control/_static/image17.png)

ファイルに変更を加えるし、この分岐にチェックインできるようになりました。 切り替えることができます簡単に分岐と Visual Studio に自動的に同期したブランチにファイルがチェック アウトします。この例では、web ページのタイトルで *\_Layout.cshtml*が"1"に修正 HotFix1 ブランチに変更されました。

![Hotfix1 ブランチ](source-control/_static/image18.png)

マスターに戻る場合は、分岐の内容、  *\_Layout.cshtml*を master ブランチでは何か、ファイルが自動的に元に戻します。

![マスター ブランチ](source-control/_static/image19.png)

このことができますすばやくの作成方法、ブランチとブランチ間を行き来反転の簡単な例です。 この機能により、分岐構造を使用して、高度なアジャイル ワークフローと自動化スクリプトを示した、[自動化すべて](automate-everything.md)」の章。 たとえばに Development 分岐で作業しているマスターから修正プログラムの分岐を作成、新しいブランチを切り替える、そこに変更を行うと、それらをコミットし Development 分岐に戻りますおよび元の作業を続行します。

Visual Studio でローカル Git リポジトリを使用する方法は、ここで説明しました。 チーム環境で通常も変更をプッシュする共通のリポジトリ。 Visual Studio tools では、リモート Git リポジトリを指すこともできます。 GitHub.com を使用するにはそのため、または使用することができます[Git と Azure リポジトリ](/azure/devops/repos/git/overview?view=vsts)作業項目とバグの追跡など他のすべての Azure DevOps 機能と統合します。

そうでは、唯一の方法はもちろん、アジャイルな分岐戦略を実装することができます。 一元的なソース管理リポジトリを使用して、同じアジャイル ワークフローを有効にすることができます。

## <a name="summary"></a>まとめ

変更を加えるし、安全性と予測可能な方法でライブを取得速度に基づいて、ソース管理システムの成功を測定します。 場合は自分で両方を変更するには、1 日または 2 つの手動テストを行う必要があるため、多少、疑問に思うかもしれません何を行う必要がある process-wise または test-wise を分単位または 1 時間よりも不要になった最悪では、その変更を行うことができます。 これを行う方法の 1 つは、継続的インテグレーションと継続的デリバリーは、ここを実装する、 [[次へ] の章](continuous-integration-and-continuous-delivery.md)します。

<a id="resources"></a>
## <a name="resources"></a>リソース

分岐戦略の詳細については、次のリソースを参照してください。

- [Team Foundation Server 2012 によるリリース パイプラインを構築](https://msdn.microsoft.com/library/dn449957.aspx)します。 Microsoft Patterns and Practices ドキュメントです。 分岐戦略の詳細については第 6 章を参照してください。 擁護者機能は、機能の分岐を切り替えるし、機能の分岐を使用している場合に短時間 (時間または日、最大) を維持することを主張します。
- [バージョン管理ガイド](https://aka.ms/vsarsolutions)します。 ALM Rangers によって分岐戦略を説明します。 [ダウンロード] タブでは、分岐 Strategies.pdf を参照してください。
- [機能の切り替えを使ったソフトウェア開発](https://msdn.microsoft.com/magazine/dn683796.aspx)します。 MSDN Magazine の記事。
- [機能の切り替え](http://martinfowler.com/bliki/FeatureToggle.html)します。 機能の概要を切り替えます/Martin Fowler のブログで機能フラグを設定します。
- [機能の切り替えと機能の分岐](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx)します。 Dylan smith の機能の切り替えに関するブログ記事をもう 1 つ。

ソース管理リポジトリに保持する必要があります機密情報を処理する方法の詳細については、次のリソースを参照してください。

- [ASP.NET と Azure App Service にパスワードやその他の機密データを展開するためのベスト プラクティス](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)します。
- [Azure の Web サイト: どのアプリケーション文字列と接続文字列の動作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)します。 オーバーライドする Azure の機能について説明します`appSettings`と`connectionStrings`内のデータ、 *Web.config*ファイル。
- [カスタム構成とアプリケーションの設定の Azure Web サイトを使用して-Stefan schackow 共演](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/)します。

ソース管理から機密情報を管理することは、その他の方法については、次を参照してください。 [ASP.NET MVC: ソース管理のプライベートの設定を保持](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)します。

> [!div class="step-by-step"]
> [前へ](automate-everything.md)
> [次へ](continuous-integration-and-continuous-delivery.md)
