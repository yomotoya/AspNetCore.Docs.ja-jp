---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: "Visual Studio を使用した ASP.NET Web 展開: データベースの配置の準備 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: caa79725ede320c4bd3e87ac246966c57175eb8e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Visual Studio を使用した ASP.NET Web 展開: データベースの配置の準備
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルは、データベースの配置の準備ができて、プロジェクトを取得する方法を示します。 データベースの構造と一部 (すべて)、アプリケーションの 2 つのデータのデータベースは、テスト、ステージング、および実稼働環境に配置する必要があります。

通常、アプリケーションを開発するには、実際のサイトに展開したくない、データベースにテスト データを入力します。 ただし、展開する一部の実稼働データを設定することもできます。 このチュートリアルでを Contoso 大学プロジェクトを構成し、展開するときに、正しいデータが含まれるように、SQL スクリプトを準備します。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](troubleshooting.md)です。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

サンプル アプリケーションでは、SQL Server Express LocalDB を使用します。 SQL Server Express は、SQL Server の無償のエディションです。 完全バージョンの SQL Server と同じデータベース エンジンに基づいているための開発中に通常使用されます。 SQL Server Express を使ってテストし、アプリケーションが動作する SQL Server のエディション間で異なる場合の機能の一部の例外を除き、実稼働環境で同じ確実に実行します。

LocalDB は SQL Server Express データベースを対象として使用することができますの特殊な実行モード*.mdf*ファイル。 LocalDB のデータベース ファイルを保持する、通常、*アプリ\_データ*web プロジェクトのフォルダーです。 SQL Server express ユーザー インスタンスの機能でを操作することもできます*.mdf*は、ファイル、ユーザー インスタンス機能は推奨されません。 そのため、LocalDB を使用するため推奨*.mdf*ファイル。

通常実稼働 web アプリケーションの SQL Server Express は使用されません。 LocalDB 具体的には、ために推奨されませんを実稼働用 web アプリケーションと IIS を使用するものはありません。

Visual Studio 2012、Visual Studio で既定で LocalDB はインストールされます。 Visual Studio 2010 以前のバージョンで (LocalDB) なしの SQL Server Express がインストールされている既定では、Visual Studio です。されている理由をインストールしたことで前提条件の 1 つとして[このシリーズの最初のチュートリアル](introduction.md)です。

SQL Server のエディションの詳細については、LocalDB などについて、次のリソース[SQL Server データベースを扱う](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver)です。

## <a name="entity-framework-and-universal-providers"></a>Entity Framework とユニバーサル プロバイダー

データベースへのアクセスには、Contoso 大学アプリケーションには、.NET Framework に含まれていないために、アプリケーションと共に配置する必要があります、次のソフトウェアが必要です。

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (Azure SQL データベースを使用する ASP.NET メンバーシップ システムを有効に)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

このソフトウェアが NuGet パッケージに含まれているため、プロジェクトは既に設定、必要なアセンブリは、プロジェクトと共に配置されるようにします。 (リンクは、このチュートリアル用にダウンロードしたスタート プロジェクトにインストールされている内容よりも新しい可能性のあるこれらのパッケージの現在のバージョン をポイント)。

Azure ではなく、サード パーティのホスティング プロバイダーに配置する場合は、Entity Framework 5.0 以降を使用することを確認します。 完全信頼では、以前のバージョンの Code First Migrations を必要として、ほとんどのホスティング プロバイダーは、中程度の信頼でアプリケーションを実行します。 中程度の信頼の詳細については、次を参照してください。、[テスト環境として IIS へのデプロイ](deploying-to-iis.md)チュートリアルです。

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>アプリケーション データベースの配置の Code First Migrations を構成します。

Contoso 大学アプリケーション データベースが Code First で管理され、Code First Migrations を使用して配置します。 Code First Migrations を使用して、データベースの配置の概要については、次を参照してください。[このシリーズの最初のチュートリアル](introduction.md)です。

アプリケーション データベースを展開するときに通常しないだけで、開発用データベースにすべてのデータのサーバーに配置する、テスト目的でのみで、データの多くはおそらく存在するためです。 たとえば、テスト データベースの受講者名は、架空は。 その一方で、多くの場合、配置できませんにそのデータのないデータベース構造だけですべての。 テスト データベース内のデータの一部は実際のデータがありますであり、ユーザーがアプリケーションの使用を開始時に存在する必要があります。 たとえば、データベースには、有効グレードの値または実際の部署名を含むテーブルがあります。

この一般的なシナリオをシミュレートするには、Code First Migrations を構成します`Seed`に実稼働環境で存在データのみをデータベースに挿入するメソッド。 これは、 `Seed` Code First 実稼働環境で、データベースが作成された後に運用環境で実行するためにメソッドがテスト データを挿入しないでください。

以前のバージョンの Code First Migrations がリリースされる前に、が一般的でした`Seed`を挿入するメソッドをテスト データも、開発時にモデル変更のたびに、データベースは完全に削除され、最初から再作成する必要があるためです。 テスト データを保持するデータベースの変更後に、Code First Migrations にテスト データを含むため、`Seed`メソッドが必要ではありません。 ダウンロードしたプロジェクト内のすべてのデータのメソッドを使用して、`Seed`初期化子クラスのメソッドです。 このチュートリアルでが、その初期化子のクラスを無効にし、`enable Migrations. Then you'll update the `シード ' メソッドで移行構成クラスのデータのみを実稼働環境で挿入するを挿入できるようにします。

次の図は、アプリケーション データベースのスキーマを示しています。

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

これらのチュートリアルについては、していることを前提、`Student`と`Enrollment`サイトが最初に展開されたときに、テーブルが空にする必要があります。 その他のテーブルには、アプリケーションがライブをプリロードする必要があるデータが含まれます。

### <a name="disable-the-initializer"></a>初期化子を無効にします。

Code First Migrations を使用する場合は、ためにを使用する必要はありません、 `DropCreateDatabaseIfModelChanges` Code First の初期化子。 この初期化子のコードは、 *SchoolInitializer.cs* ContosoUniversity.DAL プロジェクト内のファイルです。 設定、`appSettings`の要素、 *Web.config*ファイルにより、アプリケーションが最初にデータベースにアクセスしようとするたびに実行するため、この初期化子。

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

アプリケーションを開く*Web.config*ファイルを削除するかコメント アウト、 `add` Code First の初期化子クラスを指定する要素。 `appSettings`要素は次のようになります。

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> 初期化子クラスを指定する別の方法は、呼び出すことによって行います`Database.SetInitializer`で、`Application_Start`メソッドで、 *Global.asax*ファイル。 そのメソッドを使用して、初期化子を指定するプロジェクトでの移行を有効にする場合は、そのコード行を削除します。


> [!NOTE]
> Visual Studio 2013 を使用している場合は、手順 2 および 3 の間で、次の手順を追加: (a) で PMC 入力"更新プログラム パッケージ entityframework-バージョン 6.1.1"EF の現在のバージョンを取得します。 (B) がビルド エラーの一覧を取得し、それらを修正するプロジェクトをビルドします。 不要になったが存在しを右クリックし、ステートメントを使用して、ここで必要なとき、追加の解決 をクリックする名前空間用に using ステートメントを削除し、System.Data.EntityState の出現を System.Data.Entity.EntityState に変更します。


### <a name="enable-code-first-migrations"></a>Code First Migrations を有効にします。

1. (いない ContosoUniversity.DAL) ContosoUniversity プロジェクトがスタートアップ プロジェクトとして設定されていることを確認します。 **ソリューション エクスプ ローラー**ContosoUniversity プロジェクトを右クリックし、選択、**スタートアップ プロジェクトとして設定**です。 Code First Migrations は、データベース接続文字列を検索するスタートアップ プロジェクトになります。
2. **ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー** (または**NuGet Package Manager**) し、 **Package Manager Console**です。

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. 上部にある、**パッケージ マネージャー コンソール**ウィンドウ êaƒ ContosoUniversity.DAL、既定のプロジェクトとし、at、`PM>`プロンプトは「有効な移行」を入力します。

    ![enable-migrations コマンド](preparing-databases/_static/image4.png)

    (というエラーが発生した場合、*有効な移行*コマンドが認識されていない、コマンドを入力して*更新プログラム パッケージ EntityFramework-再インストール*もう一度やり直してください)。

    このコマンドを作成、*移行*ContosoUniversity.DAL プロジェクト内のフォルダーでは、2 つのファイルをそのフォルダー内に、:*される Configuration.cs*移行、および、構成に使用できるファイル*InitialCreate.cs*データベースを作成する最初の移行用のファイルです。

    ![Migrations フォルダー](preparing-databases/_static/image5.png)

    DAL プロジェクトを選択した、**既定のプロジェクト**のドロップダウン リスト、 **Package Manager Console**ため、 `enable-migrations` Code First を含むプロジェクトには、コマンドを実行する必要がありますコンテキスト クラスです。 そのクラスは、クラス ライブラリ プロジェクトでは、Code First Migrations はソリューションのスタートアップ プロジェクトでデータベース接続文字列が検索されます。 ContosoUniversity ソリューションでは、web プロジェクトがスタートアップ プロジェクトとして設定されてです。 Visual Studio のスタートアップ プロジェクトとして接続文字列を含むプロジェクトを指定しない場合は、PowerShell コマンドでスタートアップ プロジェクトを指定できます。 コマンドの構文を表示するには、コマンドを入力`get-help enable-migrations`です。

    `enable-migrations`データベースが既に存在するためにコマンドが最初の移行に自動的に作成されました。 代わりには、移行、データベースを作成するのにです。 そのために使用**サーバー エクスプ ローラー**または**SQL Server オブジェクト エクスプ ローラー**移行を有効にする前に、ContosoUniversity データベースを削除します。 移行を有効にした後、"追加移行 InitialCreate"のコマンドを入力して最初の移行を手動で作成します。 コマンド [更新プログラム-データベース] を入力して、データベースを作成できます。

### <a name="set-up-the-seed-method"></a>Seed メソッドを設定します。

このチュートリアルでは、コードを追加して固定データを追加します、`Seed`の Code First Migrations メソッド`Configuration`クラスです。 Code First Migrations 呼び出し、`Seed`メソッドすべての移行した後にします。

以降、`Seed`メソッドがすべて移行後に実行される、データが既にテーブル内の最初の移行後にします。 使用してこの状況に対処する、`AddOrUpdate`に既に挿入されている、またはまだ存在しない場合に挿入される行を更新する方法です。 `AddOrUpdate`メソッドをシナリオに最適な選択肢できない可能性があります。 詳細については、次を参照してください。 [EF 4.3 AddOrUpdate メソッドを使用して注意](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman ブログ。

1. 開く、*される Configuration.cs*ファイルし、内のコメントを置換、`Seed`メソッドを次のコード。

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. 参照`List`その下に赤い波線がある必要がないため、`using`まだその名前空間のステートメント。 インスタンスの 1 つを右クリックして`List` をクリック**解決**、順にクリック**using System.Collections.Generic**です。

    ![ステートメントを使用すると解決するには](preparing-databases/_static/image6.png)

    このメニュー項目が次のコードを追加、`using`ステートメント ファイルの上部にあります。

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. プロジェクトをビルドするには CTRL-shift キーを押し-B を押します。

プロジェクトは、展開する準備ができました、 *ContosoUniversity*データベース。 実行し、データベースにアクセスするページに移動最初に、アプリケーションを配置した後 Code First はデータベースを作成し、これを実行`Seed`メソッドです。

> [!NOTE]
> コードを追加する、`Seed`メソッドは、固定データがデータベースに挿入できるさまざまな方法の 1 つです。 代わりに、コードを追加し、`Up`と`Down`各移行クラスのメソッドです。 `Up`と`Down`メソッドには、データベースの変更を実装するコードが含まれています。 それらの例が表示されます、[データベース更新の展開](deploying-a-database-update.md)チュートリアルです。
> 
> 使用して SQL ステートメントを実行するコードを記述することも、`Sql`メソッドです。 たとえば、Department テーブルを予算列を追加して、移行の一環として $1,000.00 をすべての部門予算を初期化するために必要な場合は、次のコード行を追加でした、`Up`その移行方法。
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>メンバーシップ データベースの展開スクリプトを作成します。

Contoso 大学アプリケーションでは、ASP.NET メンバーシップ システムとフォーム認証を使用して、ユーザー認証および承認します。 **更新クレジット**ページには、管理者ロールのユーザーにのみにアクセスします。

アプリケーションを実行し、をクリックして**コース**、クリックして**更新クレジット**です。

![[更新プログラムのクレジット] をクリックします。](preparing-databases/_static/image7.png)

**ログイン**いるために、ページが表示されます、**更新クレジット**ページには、管理者特権が必要です。

入力*管理者*ユーザー名と*devpwd*をクリックして、パスワードとして**ログイン**です。

![ログイン ページ](preparing-databases/_static/image8.png)

**更新クレジット**ページが表示されます。

![クレジットのページを更新します。](preparing-databases/_static/image9.png)

ユーザーおよびロールの情報は、 *aspnet ContosoUniversity*によって指定されているデータベース、 **DefaultConnection**内の接続文字列、 *Web.config*ファイル。

このデータベースは、移行を使用すると、それを配置することはできませんので Entity Framework Code First で管理されていません。 DbDacFx プロバイダーが、データベース スキーマを配置に使用して、データベース テーブルに初期データを挿入するスクリプトを実行する、発行プロファイルを構成します。

> [!NOTE]
> (という名前になり ASP.NET Identity)、新しい ASP.NET メンバーシップ システムは、Visual Studio 2013 で導入されました。 新しいシステムでは、アプリケーションとメンバーシップ テーブルの両方を同じデータベースに保持でき、Code First Migrations を使用すると、両方を配置することができます。 サンプル アプリケーションでは、Code First Migrations を使用して展開することはできません以前 ASP.NET メンバーシップ システムを使用します。 このメンバーシップ データベースを配置する手順は、Entity Framework Code First で作成されていない SQL Server データベースを配置する、アプリケーションが必要なその他のシナリオにも適用されます。


ここで、通常たくない開発内にある実稼働環境で同じデータです。 最初にサイトを展開するときは、ほとんどまたはすべてのテストを作成するユーザー アカウントを除外する一般的なです。 ため、ダウンロードしたプロジェクトに、2 つのメンバーシップ データベース: *aspnet ContosoUniversity.mdf*開発ユーザーでと*aspnet ContosoUniversity-Prod.mdf*運用環境のユーザーにします。 このチュートリアルでは、ユーザー名が両方のデータベースで同じ: *admin*と*nonadmin*です。 両方のユーザー パスワードを使用して*devpwd*開発用データベースと*prodpwd*実稼働データベースでします。

開発ユーザー テスト環境とステージングと運用環境に運用環境のユーザーを展開します。 行うにはこのチュートリアルでの開発と実稼働環境での 2 つの SQL スクリプトを作成しを実行する、発行プロセスの構成後のチュートリアルでします。

> [!NOTE]
> メンバーシップ データベースには、アカウントのパスワードのハッシュが格納されます。 1 台のコンピューターから別のアカウントをデプロイするのには、ソース コンピューターの方がよりに、ハッシュのルーチンが、移行先サーバー上の別のハッシュを生成しないことを確認しての操作を行う必要があります。 生成されます、同じハッシュ ASP.NET ユニバーサル プロバイダーを使用すると、既定のアルゴリズムを変更しない限り、します。 既定のアルゴリズムはで指定された、HMACSHA256、**検証**の属性、  **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)**  Web.config ファイル内の要素。


SQL Server Management Studio (SSMS) を使用して、またはサード パーティ製ツールを使用して、データの配置スクリプトを手動で作成することができます。 このチュートリアルの残りの部分が SSMS では、それを実行する方法を示しますをインストールして SSMS を使用しない場合は、プロジェクトの完成版からスクリプトを取得し、ソリューション フォルダーに格納する場所のセクションにスキップします。

SSMS をインストールするには、インストールから[ダウンロード センター: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062)  をクリックして[ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe)または[ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe)です。 問題の 1 つを選択した場合、システムのインストールに失敗し、もう 1 つを試みることができます。

(600 メガバイト ダウンロードは、このことに注意してください。 時間がかかる場合がありますをインストールし、コンピューターの再起動が必要になります)。

SQL Server インストール センターの最初のページで、をクリックして**SQL Server の新規スタンドアロン インストールまたは既存のインストールに機能の追加**デフォルトの選択を受け入れ、指示に従います。

### <a name="create-the-development-database-script"></a>開発データベース スクリプトを作成します。

1. SSMS を実行します。
2. **サーバーへの接続** ダイアログ ボックスで、入力*(localdb) \v11.0*として、**サーバー名**のままにして**認証** 'éý'**Windows 認証**、クリックして**接続**です。

    ![SSMS は、サーバーに接続します。](preparing-databases/_static/image10.png)
3. **オブジェクト エクスプ ローラー**ウィンドウで、展開**データベース**を右クリックして**aspnet ContosoUniversity**、 をクリックして**タスク**をクリックし、**スクリプトの生成**です。

    ![SSMS スクリプトを生成します。](preparing-databases/_static/image11.png)
4. **公開スクリプトの生成と**ダイアログ ボックスで、をクリックして**スクリプト作成オプションの設定**です。

    スキップすることができます、**オブジェクトの選択**既定値はためにのステップ**データベース全体とすべてのデータベース オブジェクトのスクリプトを作成**対象であるとします。
5. **[詳細設定]** をクリックします。

    ![SSMS スクリプト作成オプション](preparing-databases/_static/image12.png)
6. **スクリプト作成オプションの高度な** ダイアログ ボックスを下にスクロール**スクリプトへのデータ型**、 をクリックし、**データのみ**ドロップダウン リストでオプションです。
7. 変更**を使用してデータベースのスクリプトを**に**False**です。 ステートメントを使用する Azure SQL Database の有効ながなく、テスト環境で SQL Server Express への展開の不要な。

    ![SSMS スクリプトのデータのみ、使用するステートメントのないです。](preparing-databases/_static/image13.png)
8. **[OK]**をクリックします。
9. **公開スクリプトの生成と** ダイアログ ボックスで、**ファイル名**ボックスは、スクリプトを作成する場所を指定します。 パスをソリューション フォルダー (、ContosoUniversity.sln ファイルがあるフォルダー) とファイルの名前に変更*aspnet データ-dev.sql*です。
10. をクリックして**次へ**に移動する、**概要** タブをクリックして**次へ**スクリプトを作成するには、もう一度です。

    ![SSMS スクリプトの作成](preparing-databases/_static/image14.png)
11. **[完了]**をクリックします。

### <a name="create-the-production-database-script"></a>実稼働データベース スクリプトを作成します。

を、実稼働データベースを、プロジェクトを実行していないため、LocalDB インスタンスにまだに添付されません。 したがって、最初にデータベースをアタッチする必要があります。

1. SSMS で**オブジェクト エクスプ ローラー**を右クリックして**データベース** をクリック**アタッチ**です。

    ![SSMS をアタッチします。](preparing-databases/_static/image15.png)
- **データベースのアタッチ**ダイアログ ボックスで、をクリックして**追加**に移動し、 *aspnet ContosoUniversity-Prod.mdf*ファイルを*アプリ\_データ*フォルダーです。

    ![アタッチする .mdf ファイルを SSMS の追加](preparing-databases/_static/image16.png)
- **[OK]**をクリックします。
- 以前に使用した、実稼働ファイルのスクリプトを作成する同じ手順に従います。 スクリプト ファイルの名前を付けます*aspnet データ-prod.sql*です。

## <a name="summary"></a>まとめ

両方のデータベースを展開する準備が整いましたされ、2 つのデータ配置スクリプトで、ソリューション フォルダーです。

![データ配置スクリプト](preparing-databases/_static/image17.png)

次のチュートリアルでは、展開に影響するプロジェクト設定を構成して、自動に設定した*Web.config*ファイルの配置のアプリケーションで別にする必要がある設定の変換。

## <a name="more-information"></a>説明

NuGet の詳細については、次を参照してください。 [NuGet のプロジェクトのライブラリを管理](https://msdn.microsoft.com/magazine/hh547106.aspx)と[NuGet のドキュメント](http://docs.nuget.org/docs/start-here/overview)です。 NuGet を使用しない場合は、インストールされている場合に、新機能を決定する NuGet パッケージを分析する方法を学習する必要があります。 (たとえば、構成が*Web.config*変換がビルド時などに実行する PowerShell スクリプトを構成します)。NuGet のしくみの詳細については、次を参照してください。[を作成すると、パッケージを発行する](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package)と[構成ファイルとソース コードの変換](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations)です。

>[!div class="step-by-step"]
[前へ](introduction.md)
[次へ](web-config-transformations.md)
