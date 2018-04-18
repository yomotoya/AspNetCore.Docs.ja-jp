---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: プロジェクト ファイルを理解する |Microsoft ドキュメント
author: jrjlee
description: Microsoft Build Engine (MSBuild) プロジェクト ファイルは、ビルドおよび配置プロセスの中核にあります。 このトピックは、MSBuild の概要を概念的に開始しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 49d1d4fbe48cd4f073e774d8a9c6c0c011bd3319
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="understanding-the-project-file"></a>プロジェクト ファイルを理解します。
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Microsoft Build Engine (MSBuild) プロジェクト ファイルは、ビルドおよび配置プロセスの中核にあります。 このトピックでは、MSBuild とプロジェクト ファイルの概念的な概要を開始します。 これには、プロジェクト ファイルを使用して、プロジェクト ファイルを使用して、実際のアプリケーションを展開する方法の例を使用して動作時に遭遇したが、主要なコンポーネントについて説明します。
> 
> 学習する内容。
> 
> - どの MSBuild は MSBuild プロジェクト ファイルを使用して、プロジェクトをビルドします。
> - MSBuild と統合する方法 (Web 配置) インターネット インフォメーション サービス (IIS) Web 配置ツールと同様に、展開テクノロジです。
> - プロジェクト ファイルの主要なコンポーネントを理解する方法です。
> - 方法をビルドして、複雑なアプリケーションを配置するプロジェクト ファイルを使用することができます。


## <a name="msbuild-and-the-project-file"></a>MSBuild とプロジェクト ファイル

作成して、Visual Studio でソリューションをビルドしたときに Visual Studio は MSBuild を使用して、ソリューション内の各プロジェクトをビルドします。 すべての Visual Studio プロジェクトには、プロジェクトの種類を反映するファイル拡張子を持つ、MSBuild プロジェクト ファイルが含まれています。&#x2014;など、c# プロジェクト (.csproj)、Visual Basic.NET プロジェクト (.vbproj) またはデータベース プロジェクト (.dbproj)。 プロジェクトをビルドするために MSBuild がプロジェクトに関連付けられているプロジェクト ファイルを処理する必要があります。 プロジェクト ファイルは、すべての情報と MSBuild は、コンテンツを含めるには、プラットフォームの要件、バージョン管理情報、web サーバーまたはデータベース サーバーの設定と同様に、プロジェクトをビルドするために必要がある命令を含む XML ドキュメントとタスクを実行する必要があります。

MSBuild プロジェクト ファイルがに基づいて、 [MSBuild XML スキーマ](https://msdn.microsoft.com/library/5dy88c2e.aspx)、その結果、ビルド プロセスは完全にオープンで透過的なとします。 MSBuild エンジンを使用するために Visual Studio をインストールする必要はありませんさらに、&#x2014;MSBuild.exe の実行可能ファイルは、.NET Framework の一部であり、コマンド プロンプトから実行することができます。 開発者には、プロジェクトをビルドおよび配置する方法を高度な粒度の細かい制御を強制する MSBuild XML スキーマを使用して、自分の MSBuild プロジェクト ファイル作成できます。 これらのカスタム プロジェクト ファイルは、Visual Studio を自動的に生成されるプロジェクト ファイルとまったく同じ方法で動作します。

> [!NOTE]
> チーム ビルド サービスでは、Team Foundation Server (TFS) と MSBuild プロジェクト ファイルを使用することもできます。 たとえば、継続的インテグレーション (CI) シナリオでのプロジェクト ファイルを使用して、新しいコードをチェックインするときに、テスト環境に展開を自動化することができます。 詳細については、次を参照してください。 [Web 配置の自動化の Team Foundation Server を構成する](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md)です。


### <a name="project-file-naming-conventions"></a>プロジェクト ファイルの名前付け規則

プロジェクト ファイルを作成するときに、あらゆるファイル拡張子を使用できます。 ただし、ソリューションを他のユーザーが理解しやすくするには、これらの共通の規則を使用する必要があります。

- プロジェクトをビルドするプロジェクト ファイルを作成するときは、.proj 拡張機能を使用します。
- 他のプロジェクト ファイルにインポートする再利用可能なプロジェクト ファイルを作成するときは、.targets 拡張機能を使用します。 .Targets 拡張子がファイルを通常しないビルド何も自体、単に、.proj ファイルにインポートする手順が含まれます。

### <a name="integration-with-deployment-technologies"></a>展開のテクノロジとの統合

ASP.NET web アプリケーションと ASP.NET MVC web アプリケーションと同様に、Visual Studio 2010 で web アプリケーション プロジェクトで作業した場合、これらのプロジェクトがパッケージ化と配置をターゲット環境に web アプリケーションの組み込みサポートを含めることがわかります。 **プロパティ**ページをこれらのプロジェクトに含まれている**パッケージ化/発行 Web**と**パッケージ化/発行 SQL**タブを構成するのに使用できる方法のコンポーネント、アプリケーションをパッケージ化され、展開します。 これを示しています、**パッケージ化/発行 Web**  タブ。

![](understanding-the-project-file/_static/image1.png)

これらの機能の背後にある、基になるテクノロジは、Web 発行パイプライン (WPP) と呼ばれます。 WPP は基本的に MSBuild を表示および[Web Deploy](https://go.microsoft.com/?linkid=9805122) web アプリケーションの完全なビルド、パッケージ、および展開プロセスを提供します。

良いニュースは、web プロジェクト用のカスタム プロジェクト ファイルを作成するときに、WPP が提供するための統合ポイントを活用を行うことができます。 プロジェクトをビルド、web 配置パッケージを作成し、1 つのプロジェクト ファイルと MSBuild に 1 回の呼び出しで、リモート サーバーでこれらのパッケージをインストールすることができるプロジェクト ファイルで展開する手順を含めることができます。 ビルド プロセスの一部として、その他の実行可能ファイルを呼び出すこともできます。 たとえば、スキーマ ファイルからデータベースを配置する VSDBCMD.exe コマンド ライン ツールを実行できます。 このトピックの過程で、これらの機能をエンタープライズ展開シナリオの要件を満たすために行える方法が表示されます。

> [!NOTE]
> Web アプリケーションの展開プロセスのしくみの詳細については、次を参照してください。 [ASP.NET Web アプリケーション プロジェクトの展開の概要](https://msdn.microsoft.com/library/dd394698.aspx)です。


## <a name="the-anatomy-of-a-project-file"></a>プロジェクト ファイルの構造は、

ビルド プロセスの詳細を見ると、前に、MSBuild プロジェクト ファイルの基本的な構造を理解するには数分をかけて価値です。 このセクションでは、確認、編集、またはプロジェクト ファイルを作成するときに発生する一般的な要素の概要を示します。 具体的には、学習します。

- 使用する*プロパティ*のビルド プロセスの変数を管理します。
- 使用する*項目*など、ビルド処理への入力を識別するためのコード ファイル。
- 使用する方法*ターゲット*と*タスク*指示を提供する実行、msbuild を使用して*プロパティ*と*項目*で定義されています。プロジェクト ファイルです。

これは、MSBuild プロジェクト ファイルの主な要素間の関係を示しています。

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Project 要素

[プロジェクト](https://msdn.microsoft.com/library/bcxfsh87.aspx)要素は、すべてのプロジェクト ファイルのルート要素です。 プロジェクト ファイルの XML スキーマを識別するだけでなく、**プロジェクト**要素は、ビルド プロセスのエントリ ポイントを指定する属性を含めることができます。 たとえば、[連絡先のマネージャーのサンプル ソリューション](the-contact-manager-solution.md)、 *Publish.proj*という名前のターゲットを呼び出すことにより、ビルドを開始するファイルを指定**FullPublish**です。


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>プロパティと条件

通常、プロジェクト ファイルは、多数のさまざまなが正常にビルドし、プロジェクトを配置するために情報を提供する必要があります。 これらの種類の情報には、サーバー名、接続文字列、資格情報、ビルド構成、ソースと変換先ファイルのパス、およびカスタマイズをサポートするために追加するその他の情報を含めることができます。 内では、プロジェクト ファイルのプロパティを定義する必要があります、 [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx)要素。 MSBuild プロパティは、キーと値のペアで構成されます。 内で、 **PropertyGroup**要素、要素名プロパティのキーと、要素の内容がプロパティの値を定義します。 たとえば、という名前のプロパティを定義する**ServerName**と**ConnectionString**静的サーバー名と接続文字列を保存します。


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


プロパティ値を取得するには、形式が使用します<strong>$(</strong><em>PropertyName</em><strong>)</strong><em>。</em> 。たとえばの値を取得するため、 <strong>ServerName</strong>プロパティを入力します。


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> プロパティ値を使用して、このトピックで後述する方法の例とが表示されます。


プロジェクト ファイルで静的なプロパティとして情報の埋め込みは常に、ビルド プロセスを管理するための最適なアプローチです。 多数のシナリオでは、他のソースから情報を取得するか、コマンド プロンプトからの情報を提供するユーザーを支援するします。 MSBuild では、コマンド ライン パラメーターとしてのプロパティの値を指定することができます。 たとえば、ユーザーがの値を指定できます**ServerName**ときに実行する MSBuild.exe コマンドラインからです。


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> MSBuild.exe で使用できるスイッチと引数の詳細については、次を参照してください。 [MSBuild コマンド ライン リファレンス](https://msdn.microsoft.com/library/ms164311.aspx)です。


環境変数と組み込みのプロジェクトのプロパティの値を取得するのにプロパティと同じ構文を使用することができます。 多数のよく使用されるプロパティが定義した、およびできますして使用する、プロジェクト ファイルに関連するパラメーター名を含むです。 たとえば、現在のプロジェクト プラットフォームを取得する&#x2014;など **x86** または **AnyCpu**&#x2014;含めることができます、 **$(Platform)** プロパティ参照でプロジェクト ファイルです。 詳細については、次を参照してください。[のビルドのコマンドとプロパティのマクロ](https://msdn.microsoft.com/library/c02as0cs.aspx)、 [MSBuild プロジェクトの共通プロパティ](https://msdn.microsoft.com/library/bb629394.aspx)、および[予約済みプロパティ](https://msdn.microsoft.com/library/ms164309.aspx)です。

プロパティはと共によく使用*条件*です。 MSBuild のほとんどの要素をサポート、**条件**属性は、MSBuild 基になる要素を評価する条件を指定することができます。 たとえば、このプロパティの定義があるとします。


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


MSBuild では、このプロパティの定義を処理するとき、まず確認するかどうか、 **$(OutputRoot)** プロパティの値が使用可能な。 プロパティの値が空白の場合&#x2014;つまり、ユーザーは、このプロパティの値を指定していない&#x2014;条件の評価が **true** にプロパティの値を設定および **.\Publish\Out** です。条件の評価が場合は、ユーザーは、このプロパティの値を指定するが、 **false**静的プロパティの値が使用されていません。

条件を指定するさまざまな方法の詳細については、次を参照してください。 [MSBuild の条件](https://msdn.microsoft.com/library/7szfhaft.aspx)です。

### <a name="items-and-item-groups"></a>アイテムとアイテム グループ

プロジェクト ファイルの重要な役割の 1 つは、ビルド処理への入力を定義します。 これらの入力ファイルは、通常、&#x2014;コード ファイル、構成ファイル、コマンド ファイルは、およびその他のファイルとしてコピーまたは処理する必要のあるビルド プロセスの一部です。 MSBuild プロジェクト スキーマでこれらの入力がによって表される[項目](https://msdn.microsoft.com/library/ms164283.aspx)要素。 内で、プロジェクト ファイルの項目を定義する必要があります、 [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx)要素。 同じように**プロパティ**要素、名前を指定できます、**項目**要素などのただしです。 ただし、指定する必要があります、 **Include**属性をファイルまたは項目を表すワイルドカードを指定します。


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


倍数を指定することによって**項目**と同じ名前の要素、効果的に作成するリソースの名前付きリスト。 Visual Studio によって作成されるプロジェクト ファイルの 1 つの内部情報を確認するは、この動作を確認することをお勧めします。 たとえば、 *ContactManager.Mvc.csproj*サンプル ソリューション内のファイルには、それぞれに同じ名前の複数の項目グループの多くが含まれる**項目**要素。


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


この方法で、プロジェクト ファイルが同じ方法で処理する必要のあるファイルのリストを構築するために MSBuild を指示が&#x2014;、**参照**一覧には、ビルドの成功のためにする必要があるアセンブリが含まれています、 **コンパイル**一覧には、コンパイルする必要があるコード ファイルが含まれています。 および**コンテンツ**一覧には、変更せずにコピーする必要があるリソースが含まれます。 ビルド プロセスの参照し、これらの項目を使用して、このトピックの後半に紹介します。

項目の要素を含めることも[ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx)子要素です。 これらはユーザー定義のキー/値ペアであり、本質的にそのアイテムに固有のプロパティを表します。 たとえば、多数の**コンパイル**項目要素をプロジェクト ファイルには、 **DependentUpon**子要素です。


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> ユーザーが作成した項目のメタデータの他のすべての項目は作成時にさまざまな一般的なメタデータに割り当てられます。 詳細については、「[既知の項目メタデータ](https://msdn.microsoft.com/library/ms164313.aspx)」を参照してください。


作成することができます**ItemGroup**ルート レベル内の要素**プロジェクト**要素または固有の仕様内にある**ターゲット**要素。 **ItemGroup**要素もサポート**条件**属性で、プロジェクトの構成やプラットフォームのような条件に従って、ビルド処理への入力を調整することができます。

### <a name="targets-and-tasks"></a>ターゲットとタスク

MSBuild スキーマで、[タスク](https://msdn.microsoft.com/library/77f2hx1s.aspx)要素は、個々 のビルド命令 (またはタスク) を表します。 MSBuild には、さまざまな定義済みのタスクが含まれています。 例えば:

- **コピー**タスクは、新しい場所にファイルをコピーします。
- **Csc**タスクは、Visual c# コンパイラを起動します。
- **Vbc**タスクは、Visual Basic コンパイラを起動します。
- **Exec**タスクが、指定したプログラムを実行します。
- **メッセージ**タスクは、ロガーにメッセージを書き込みます。

> [!NOTE]
> すぐに使用できますが、タスクの詳細については、次を参照してください。 [MSBuild タスク リファレンス](https://msdn.microsoft.com/library/7z253716.aspx)です。 独自のカスタム タスクを作成する方法などのタスクの詳細については、次を参照してください。 [MSBuild タスク](https://msdn.microsoft.com/library/ms171466.aspx)です。


内のタスクを含めることが常に必要があります[ターゲット](https://msdn.microsoft.com/library/t50z2hka.aspx)要素。 A**ターゲット**要素は、順番に実行される 1 つまたは複数のタスクのセットと、プロジェクト ファイルは、複数のターゲットを含めることができます。 タスク、または一連のタスクを実行する場合は、それらを含むターゲットを呼び出します。 たとえば、メッセージ ログに記録する単純なプロジェクト ファイルがあるとします。


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


使用して、コマンドラインからターゲットを呼び出すことができます、 **/t**ターゲットを指定するスイッチです。


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


また、追加することができます、 **DefaultTargets**属性を**プロジェクト**要素は、呼び出し先のターゲットを指定します。


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


この場合、コマンドラインからターゲットを指定する必要はありません。 プロジェクト ファイルは、単を指定して MSBuild を呼び出す、 **FullPublish**するターゲット。


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


ターゲットとタスクの両方を含めることができます**条件**属性。 そのため、特定の条件が満たされた場合に、ターゲットの全体または個々 のタスクを省略することもできます。

一般に、便利なタスクとターゲットを作成するときに、プロパティと、プロジェクト ファイルに他の場所で定義した項目を参照する必要があります。

- プロパティ値を使用する入力<strong>$(</strong><em>PropertyName</em><strong>)</strong>ここで、 <em>PropertyName</em>の名前を指定、 <strong>プロパティ</strong>要素またはパラメーターの名前。
- 項目を使用する入力<strong>@(</strong><em>ItemName</em><strong>)</strong>ここで、 <em>ItemName</em>の名前を指定、<strong>項目</strong>要素。

> [!NOTE]
> 同じ名前の複数の項目を作成する場合、リストをビルドすることに注意してください。 これに対し、同じ名前の複数のプロパティを作成する場合は、最後のプロパティ値を指定する必要がプロパティが上書きされます任意前と同じ名前&#x2014;プロパティは、単一の値のみを含めることができます。


たとえば、 *Publish.proj*サンプル ソリューションのファイルを見て、 **BuildProjects**ターゲットです。


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


このサンプルでは、これらの要点を確認することができます。

- 場合、 **BuildingInTeamBuild**パラメーターが指定されの値を持つ**true**、このターゲット内のタスクのいずれも実行されます。
- ターゲットには 1 つのインスタンスが含まれています、 [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx)タスク。 このタスクでは、MSBuild の他のプロジェクトをビルドすることができます。
- **ProjectsToBuild**項目は、タスクに渡されます。 この項目はプロジェクトまたはソリューション ファイルで定義されたすべての一覧を表すことが**ProjectsToBuild**項目の項目グループ内の要素。 ここで、 **ProjectsToBuild**項目が 1 つのソリューション ファイルを参照します。

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- 渡されるプロパティの値、 **MSBuild**タスクに名前付きパラメーターを含める**OutputRoot**と**構成**です。 これらは、されていない場合、パラメーター値が指定される場合または静的プロパティの値に設定されます。

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

表示することも、 **MSBuild**タスクを呼び出すという名前のターゲット**ビルド**です。 これは、広く使用されている Visual Studio プロジェクト ファイルでは、カスタムのプロジェクト ファイルで使用するようないくつかの組み込みのターゲットの 1 つ**ビルド**、**クリーン**、 **を再構築**、および**発行**です。 ターゲットとタスクをビルド処理を制御しの使用に関する詳細情報を学びます、 **MSBuild**具体的には、タスクのこのトピックで後述します。

> [!NOTE]
> ターゲットの詳細については、次を参照してください。 [MSBuild ターゲット](https://msdn.microsoft.com/library/ms171462.aspx)です。


## <a name="splitting-project-files-to-support-multiple-environments"></a>複数の環境をサポートするためにプロジェクト ファイルの分割

テスト サーバー、ステージングのプラットフォームでは、運用環境など、複数の環境にソリューションを展開したいとします。 構成は、これらの環境間で大幅に異なる場合があります&#x2014;だけでなくサーバー名、接続文字列、およびの観点からも資格情報、セキュリティ設定、およびその他の要因の多く観点から可能性があります。 これを定期的に実行する必要がある場合は、対象の環境を切り替えるたびに、プロジェクト ファイルで複数のプロパティを編集する非常に便利なことはできません。 最適なソリューションをビルド プロセスに提供するプロパティの値の無限のリストを必要とするでもありません。

幸運にもこれとは別です。 MSBuild では、ビルド構成を複数のプロジェクト ファイルに分割することができます。 動作を理解するこの、サンプルのソリューションでは、2 つのカスタム プロジェクト ファイルがあることに注意してください。

- *Publish.proj*プロパティ、項目が含まれています、およびターゲットはすべての環境に共通します。
- *Env Dev.proj*、開発環境に固有のプロパティが含まれています。

今すぐことに注意して、 *Publish.proj*ファイルが含まれています、[インポート](https://msdn.microsoft.com/library/92x05xfs.aspx)、タグのすぐ下にある要素、**プロジェクト**タグ。


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


**インポート**要素を使用して、現在の MSBuild プロジェクト ファイルを別の MSBuild プロジェクト ファイルの内容をインポートします。 ここで、 **TargetEnvPropsFile**パラメーターは、インポートするプロジェクト ファイルのファイル名を提供します。 MSBuild を実行するときに、このパラメーターの値を指定することができます。


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


実質的に、これは、1 つのプロジェクト ファイルに 2 つのファイルの内容をマージします。 このアプローチを使用して、ユニバーサル ビルド構成を含む 1 つのプロジェクト ファイルと環境固有のプロパティを含む複数の補助プロジェクト ファイルを作成できます。 その結果、異なるパラメーター値を持つコマンドを実行することができます、別の環境にソリューションを展開します。

![](understanding-the-project-file/_static/image3.png)

この方法で、プロジェクト ファイルの分割に従うことをお勧めします。 複数のプロジェクト ファイル ユニバーサル ビルド プロパティの重複を回避しながら、1 つのコマンドを実行して、複数の環境に展開できます。

> [!NOTE]
> サーバー環境の環境に固有のプロジェクト ファイルをカスタマイズする方法のガイダンスについては、次を参照してください。[ターゲット環境の配置プロパティを構成する](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)です。


## <a name="conclusion"></a>まとめ

このトピックでは、MSBuild プロジェクト ファイルの概要を提供し、ビルド プロセスを制御する独自のカスタム プロジェクト ファイルを作成する方法を説明します。 プロジェクト ファイルを簡単にビルドおよび複数の送信先にプロジェクトを配置するには、ユニバーサル ビルド手順と環境固有のビルド プロパティに分割の概念も導入されました。

次のトピックでは、[ビルド プロセスの理解](understanding-the-build-process.md)複雑さのレベルが現実的なソリューションの配置をここで、コントロールのビルドおよび配置するプロジェクト ファイルを使用する方法により多くの洞察を提供します。

## <a name="further-reading"></a>関連項目

プロジェクト ファイルと、WPP をより詳細な概要については、次を参照してください。[内の Microsoft Build Engine: MSBuild を使用して、Team Foundation ビルド](http://amzn.com/0735645248)Sayed Ibrahim Hashimi して William Bartholomew、ISBN: 978-0-7356-4524-0 です。

> [!div class="step-by-step"]
> [前へ](setting-up-the-contact-manager-solution.md)
> [次へ](understanding-the-build-process.md)
