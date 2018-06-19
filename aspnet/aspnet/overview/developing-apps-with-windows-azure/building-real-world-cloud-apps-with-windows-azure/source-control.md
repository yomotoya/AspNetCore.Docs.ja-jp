---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: ソース管理 (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント
author: MikeWasson
description: Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 0022458fa89a3be7ee8303750ad0e072df3b1bab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875696"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>ソース管理 (Azure と実際のクラウド アプリのビルド)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


ソース管理は、すべてのクラウド開発プロジェクト、チームの環境だけでなくに不可欠です。 ソース コードの編集と思われるでしょうまたはでもは、Word 文書を元に戻す関数および自動バックアップは、ソース管理なしは、問題が発生すると、さらに時間を保存できないように、プロジェクト レベルでこれらの関数を提供します。 クラウド ソース管理サービス、不要になった複雑なセットアップは、心配する必要があるし、5 ユーザーまで無料の Visual Studio Online のソース管理を使用することができます。

この章の最初の部分では、注意する 3 つのキーのベスト プラクティスについて説明します。

- [自動化スクリプトをソース コードとして扱う](#scripts)およびバージョンと共に、アプリケーション コードにします。
- [機密情報を確認しない](#secrets)(資格情報などの機密データ) をソース コード リポジトリにします。
- [ソースの分岐をセットアップ](#devops)DevOps ワークフローを有効にします。

章の残りの部分では、Visual Studio、Azure、および Visual Studio Online でのこれらのパターンの一部のサンプル実装を提供します。

- [Visual Studio でのソース管理にスクリプトを追加します。](#vsscripts)
- [Azure で機密データを保存します。](#appsettings)
- [Visual Studio および Visual Studio Online で Git を使用します。](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>自動化スクリプトをソース コードとして扱う

クラウド プロジェクトで作業している場合は、点を頻繁に変更して、お客様によって報告された問題に迅速に対応できるたいとします。 すばやく応答を使用して自動化スクリプト」の説明に従って、[自動化すべて](automate-everything.md)章します。 すべてのお客様の環境の作成に使用するスクリプトはなど、アプリケーションのソース コードと同期する必要があります。 スケールに展開します。

スクリプトのコードとの同期を保つには、ソース管理システムに保存します。 変更をロールバックや開発コードからは実稼働コードをクイック修正を行う必要が生じた場合は、どの設定が変更されたか、チーム メンバーがある必要があるバージョンのコピーを追跡するために時間を無駄にする必要はありません。 スクリプトをする必要がありますが、必要に応じて、すべてのチーム メンバーを同じスクリプトを扱うことを保証しているコード ベースへと同期されているを保証しています。 修正を運用環境または新しい機能の開発のテストと展開を自動化する必要があるかどうか、更新する必要があるコードの適切なスクリプトが必要があります。

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>内の機密情報をチェックしないでください。

ソース コード リポジトリ通常ユーザーがアクセスが多すぎますのパスワードなどの機密データを適切にセキュリティで保護された場所であります。 スクリプトは、パスワードなどの機密情報に依存する場合は、ソース コードでは保存されません、別の場所に、機密情報が保存されるようにそれらの設定をパラメーター化します。

たとえば、Azure を含むファイルをダウンロードすることができますでは、発行プロファイルの作成を自動化するために設定を発行します。 これらのファイルには、ユーザー名と、Azure サービスを管理するが承認されているパスワードが含まれます。 これを使用する場合を作成する方法と、発行プロファイルとこれらのファイルをソース管理にチェックインする場合、リポジトリへのアクセスを持つユーザーことがわかりますそれらのユーザー名とパスワード。 安全に格納できます、パスワード、発行プロファイル自体が暗号化されていると、内にあるため、 *. pubxml.user*ファイルを既定ではソース管理に含まれません。

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>DevOps ワークフローを容易に構造体のソース分岐

リポジトリの分岐をどのように実装する新しい機能の開発し、実稼働環境での問題を修正の両方に影響します。 多数のメディア サイズ チームの使用を調整するパターンを次に示します。

![ソースの分岐構造](source-control/_static/image1.png)

マスター分岐には、実稼働環境では、コードが常と一致します。 マスターの下に分岐は、開発ライフ サイクルのさまざまな段階に対応します。 Development 分岐では、新しい機能を実装します。 小規模なチームのだけマスターと開発、いて多くの場合、ユーザーに開発とマスター ステージング分岐があることをお勧めします。 ステージングを使用して、最終的な統合テストの更新プログラムが実稼働環境に移動される前にすることができます。

大規模なチームである可能性があります。 新しい機能ごとに別の分岐小さなチームでは全員、development 分岐にチェックインするがあります。

機能 A は、準備ができてを結合したときに、各機能用に分岐がある場合、ソース コードの変更を開発に分岐上下の他の機能の分岐にします。 このソース コードをマージ処理時間がかかり、でき個別機能しながら、その作業を避けるためには、チームによって実装と呼ばれる代替*[機能を切り替えます](http://en.wikipedia.org/wiki/Feature_toggle)* (とも呼ばれます*機能フラグ*)。 これは、すべての機能のコードはすべて、同じツリーには、有効にするにまたは、コードでスイッチを使用して各機能を無効にすることを意味します。 たとえば、機能 A 修正アプリ タスクの新しいフィールドは、機能 B は、キャッシュの機能を追加します。 両方の機能のコードは、development 分岐がありますが、アプリのみが表示されます、変数が true の場合、およびそれに設定されているときに、新しいフィールドのみキャッシュを使用する、別の変数が設定されている場合に true に設定します。 機能 A に昇格する準備ができていませんが、機能 B は準備ができて、すべてのオフ機能 A スイッチを使用して実稼働環境にコードを昇格することができ機能 B を切り替えます。 機能 A を終了し、昇格しても、後でないソース コードの結合をすべてことができます。

機能の分岐または切り替えを使用するかどうか次のように分岐構造では、アジャイルかつ反復的な方法でコードを運用環境に開発からをフローすることができます。

この構造体では、お客様のフィードバックに迅速に対応することもできます。 実稼働環境にクイック修正する必要がある場合は、行うことができますもを効率的にアジャイルな手法でします。 Master またはステージングから分岐を作成することができ、マスターにマージすることと下向きの準備が開発と機能の分岐にします。

![Hotfix 分岐](source-control/_static/image2.png)

運用と開発の分岐には、その分離を使用した次のように分岐構造、せず運用環境の問題が生じる可能性する運用修正と共に新しい機能のコードを昇格することの位置にします。 この新しい機能のコードを徹底的にテストおよびの準備が整って運用できない可能性があり、準備が整っていない変更を取り消します作業の多くを実行する必要があります。 または、変更をテストし、その展開を準備するために、修正プログラムを遅延する必要があります。

次に Visual Studio、Azure、および Visual Studio Online にこれら 3 つのパターンを実装する方法の例が表示されます。 これらは、how に-は it 手順の詳細についてはではなく、例すべての必要なコンテキストを提供の詳細については、次を参照してください。、[リソース](#resources)章の末尾。

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Visual Studio でのソース管理にスクリプトを追加します。

Visual Studio でのソース管理にスクリプトを追加するには、Visual Studio ソリューション フォルダー (プロジェクトをソース管理されている場合) に含まれます。 1 つの方法を次に示します。

ソリューション フォルダーに、スクリプト用のフォルダーを作成 (と同じフォルダーには、 *.sln*ファイル)。

![オートメーション フォルダー](source-control/_static/image3.png)

スクリプト ファイルをフォルダーにコピーします。

![フォルダーの内容の自動化](source-control/_static/image4.png)

Visual Studio でプロジェクトをソリューション フォルダーを追加します。

![新しいソリューション フォルダーのメニューの選択](source-control/_static/image5.png)

スクリプト ファイルをソリューション フォルダーに追加します。

![既存の項目のメニュー項目を追加します。](source-control/_static/image6.png)

![[既存項目の追加] ダイアログ ボックス](source-control/_static/image7.png)

スクリプト ファイルがプロジェクトに含まれるようになりましたし、ソース管理が対応するソース コードの変更とそのバージョンの変更を追跡します。

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Azure で機密データを保存します。

Azure Web サイトでアプリケーションを実行する場合、ソース管理の資格情報を格納しないようにする方法の 1 つは代わりに Azure に保存してです。

たとえば、その Web.config ファイル 2 つの接続文字列を実稼働環境や、Azure ストレージ アカウントにアクセスできるようにキー内のパスワードを持つ修正アプリケーションが格納されます。

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

これらの設定の値を実際の運用環境を配置するかどうか、 *Web.config*ファイルに配置した場合、または、 *Web.Release.config*展開時に、それらを挿入する Web.config 変換の構成ファイルオブジェクトは、ソース リポジトリに格納されます。 発行プロファイル、実稼働環境へ、データベース接続文字列を入力する場合は、パスワードになります、 *.pubxml*ファイル。 (でしたを除外する、 *.pubxml* 、ソース管理からファイルしますが、他のすべての展開設定の共有の利点が失われます)。

Azure で使用するための代替、 **appSettings**および接続文字列のセクションでは、 *Web.config*ファイル。 ここで、関連の一部である、**構成**Azure 管理ポータルで web サイト タブ。

![appSettings、connectionStrings ポータル](source-control/_static/image8.png)

この web サイトとアプリケーションの実行時にプロジェクトを配置するときに Azure に格納しているどのような値は、どのような値は、Web.config ファイルを上書きします。

Azure でこれらの値を設定するには、管理ポータルまたはスクリプトを使用します。 示した、環境の作成の自動化スクリプト、[自動化すべて](automate-everything.md)章が Azure SQL データベースを作成し、ストレージと SQL データベースの接続文字列を取得および web サイトの設定でこれらのシークレットを格納します。

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

実際の値をソース リポジトリに永続化されないように、スクリプトがパラメーター化されることを確認します。

開発環境内でローカルに実行すると、アプリを読み取り、ローカルの Web.config ファイルとの接続文字列ポインターは、SQL Server の LocalDB データベースに、*アプリ\_データ*web プロジェクトのフォルダーです。 Azure でアプリを実行すると、アプリの Web.config ファイルからこれらの値を読み取るしようとする、バックアップを取得、確認し、使用は、格納されている新機能が実際に Web.config ファイルで、Web サイトの値です。

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Visual Studio および Visual Studio Online で Git を使用します。

任意のソース管理の環境を使用すると、前に示した DevOps 分岐構造を実装します。 分散チームの[分散バージョン コントロール システム](http://en.wikipedia.org/wiki/Distributed_revision_control)(DVCS) 最も効果的である; 他のチームを[システムを一元的な](http://en.wikipedia.org/wiki/Revision_control)うまく機能があります。

[Git](http://git-scm.com/)がある DVCS が非常に一般的なになります。 ソース管理に Git を使用する場合、ローカル コンピューター上のすべての履歴とリポジトリの完全なコピーがあります。 多くの人を簡単になっているため、ネットワークに接続していない--操作を行うときに作業を続行するをコミットしてロールバック、作成して、分岐を切り替えるなどです。 ネットワークに接続していることが容易かつ迅速に分岐を作成し、すべてのものがローカルの場合、分岐を切り替えます。 他の開発者に影響を与えず、ローカル コミットまたはロールバック時を実行することもできます。 サーバーに送信する前にコミットをバッチすることができます。

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO) は、Team Foundation サービスと呼ばれる両方の Git は、以前と[Team Foundation バージョン管理](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx)(TFVC; ソース管理を一元的な)。 ここでの Microsoft Azure のグループにチームによって一元的なソース管理を使用、配布、使用するものもと (一部のプロジェクトに集中型および他のプロジェクトの分散) の組み合わせがあります。 VSO サービスは、5 ユーザーまで無料です。 Free プランにサインアップすることができます[ここ](https://go.microsoft.com/fwlink/?LinkId=307137)です。

Visual Studio 2013 には、組み込みファースト クラスにはが含まれています。 [Git サポート](https://msdn.microsoft.com/library/hh850437.aspx); ここでは、簡単に、その動作のデモします。

プロジェクトを Visual Studio 2013 で開き、ソリューションを右クリックして**ソリューション エクスプ ローラー**を選択して**ソリューションをソース管理に追加**です。

![ソース管理にソリューションを追加します。](source-control/_static/image9.png)

Visual Studio では、TFVC (一元化されたバージョン管理) または Git を使用するように求めます。

![ソース管理を選択します。](source-control/_static/image10.png)

Git を選択し、をクリックして**OK**、Visual Studio は、ソリューション フォルダーに新しいローカルの Git リポジトリを作成します。 新しいリポジトリはまだありませんファイルです。Git コミットの手順を実行して、リポジトリに追加する必要があります。 ソリューションを右クリックして**ソリューション エクスプ ローラー**、クリックして**コミット**です。

![コミット](source-control/_static/image11.png)

Visual Studio は自動的にすべてのコミットのプロジェクト ファイルをステージングしでそれらを一覧表示**チーム エクスプ ローラー**で、**含まれる変更**ウィンドウです。 (発生した場合、コミットを含めるしたくないいくつか、選択、右クリックしをクリックして**除外**)。

![チーム エクスプローラー](source-control/_static/image12.png)

コミットのコメントを入力し、クリックして**コミット**、および Visual Studio は、コミットが実行され、コミット ID を表示します

![チーム エクスプ ローラーの変更](source-control/_static/image13.png)

今すぐ内容と異なるリポジトリになるようにいくつかのコードを変更する場合は、相違点を簡単に確認できます。 ファイルを切り替えたことを選択して右クリック**Unmodified と比較**、コミットされていない変更内容を示す比較表示を取得します。

![変更されていないと比較します。](source-control/_static/image14.png)

![相違の表示の変更](source-control/_static/image15.png)

どのような変更を加えようとして確認するには簡単に確認できます。

– 分岐する必要があると仮定すぎる Visual Studio でことができます。 **チーム エクスプ ローラー**をクリックして**新しい分岐**です。

![チーム エクスプ ローラーの新しい分岐](source-control/_static/image16.png)

ブランチ名を入力し、をクリックして**分岐の作成**、選択した場合と**チェック アウト ブランチ**、Visual Studio が自動的にチェック アウト、新しい分岐します。

![チーム エクスプ ローラーの新しい分岐](source-control/_static/image17.png)

ファイルに変更を加えるをこの分岐にチェックインできます。 することができますを簡単に切り替える分岐と Visual Studio に自動的に同期する方が分岐するのには、ファイルのチェック アウトしました。この例では、web ページのタイトルで *\_Layout.cshtml* 「修正 1」HotFix1 で分岐に変更されました。

![Hotfix1 分岐](source-control/_static/image18.png)

マスターに戻る場合は、分岐の内容、  *\_Layout.cshtml*ファイルをマスター分岐には何かを自動的に元に戻します。

![マスター ブランチ](source-control/_static/image19.png)

この方法を素早く分岐を作成し、できます分岐間で前後の上下を反転の簡単な例です。 この機能により、分岐構造を使用して高アジャイルのワークフローと自動化スクリプトに表示される、[自動化すべて](automate-everything.md)章します。 たとえば、ことができます、Development 分岐で作業しているマスターから修正分岐を作成、新しい分岐に切り替えます、変更内容がありますと、それらをコミットし、Development 分岐に戻りますおよび行っていた操作を続行します。

ここで確認した新機能は、Visual Studio でのローカルの Git リポジトリを操作する方法です。 チームの環境で、通常もに変更をプッシュ共通のリポジトリ。 Visual Studio のツールを使用して、リモート Git リポジトリを指すこともできます。 そのような目的の GitHub.com を使用するか、使用することができます[Visual Studio Online での Git](https://msdn.microsoft.com/library/hh850437.aspx)作業項目とバグの追跡などの他のすべての Visual Studio Online 機能と統合します。

これは、当然のことながら、アジャイルの分岐方法を実装することができます、唯一の方法がありません。 一元的なソース管理リポジトリを使用して同じアジャイルのワークフローを有効にすることができます。

## <a name="summary"></a>まとめ

変更を加えるし、セーフと予測可能な方法でライブを取得するどの程度の速度に基づいて、ソース管理システムの成功率を測定します。 ならなくなった場合、1 日にまたは 2 つの手動テストの実行する必要があるために、変更を加える心配と思うかもしれません自分で何を行う必要がある process-wise または test-wise できるように、分または 1 時間よりも不要になった最悪では、その変更を行うことができます。 これを行う方法の 1 つは、継続的インテグレーションと継続的な配信は、ここを実装する、[次のチャプター](continuous-integration-and-continuous-delivery.md)です。

<a id="resources"></a>
## <a name="resources"></a>リソース

[Visual Studio Online](https://www.visualstudio.com/)ポータルは、ドキュメントとサポートのサービスを提供し、アカウントにサインアップすることができます。 Visual Studio 2012 をしたに Git を使用する場合は、次を参照してください。 [Visual Studio Tools for Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c)です。

TFVC (一元化されたバージョン管理) および Git (分散型バージョン管理) の詳細については、次のリソースを参照してください。

- [バージョン コントロール システムを使用する: TFVC または Git しますか?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) MSDN のドキュメントには、TFVC および Git の違いの概要を作成するテーブルが含まれています。
- [Team Foundation Server に満足し、Git、ような場合にする方がよいですか。](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Git と TFVC の比較できます。

分岐方法の詳細については、次のリソースを参照してください。

- [Team Foundation Server 2012 でリリース パイプラインの構築](https://msdn.microsoft.com/library/dn449957.aspx)です。 Microsoft Patterns and Practices ドキュメントです。 分岐方法についての第 6 章を参照してください。 支持者の機能では、機能の分岐に切り替えますおよび、分岐機能を使用している場合に維持する短時間 (時間や日数に、多くてを支援します。
- [バージョン コントロールのガイド](https://aka.ms/vsarsolutions)です。 ALM Rangers によって分岐方法を説明します。 [ダウンロード] タブでは、分岐 Strategies.pdf を参照してください。
- [機能の切り替えを使用したソフトウェア開発](https://msdn.microsoft.com/magazine/dn683796.aspx)です。 MSDN マガジンの記事です。
- [機能のオン/オフ](http://martinfowler.com/bliki/FeatureToggle.html)です。 機能の概要を切り替えます/Martin ファウラーのブログで機能フラグを設定します。
- [機能を切り替えます vs 機能分岐](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx)です。 機能の切り替え、Dylan Smith によっては別のブログの投稿。

ソース管理リポジトリ内に収めることがない機密情報を処理する方法の詳細については、次のリソースを参照してください。

- [ASP.NET と Azure App Service へのパスワードなどの機密データの展開のベスト プラクティス](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)です。
- [Azure の Web サイト: どのようにアプリケーション文字列と接続文字列の動作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)です。 オーバーライドする Azure の機能についての説明`appSettings`と`connectionStrings`内のデータ、 *Web.config*ファイル。
- [カスタム構成とアプリケーション設定の Azure Web サイトを使用して、Stefan Schackow に](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/)です。

その他のソース管理から機密情報を保持しておく方法の詳細については、次を参照してください。 [ASP.NET MVC: ソース管理の設定を秘密保持](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)です。

> [!div class="step-by-step"]
> [前へ](automate-everything.md)
> [次へ](continuous-integration-and-continuous-delivery.md)
