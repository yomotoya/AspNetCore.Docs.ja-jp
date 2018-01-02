---
uid: whitepapers/aspnet-and-iis6
title: "IIS 6.0 と ASP.NET 1.1 を実行している |Microsoft ドキュメント"
author: rick-anderson
description: "Windows Server 2003 には、IIS 6.0 および ASP.NET 1.1 の両方が含まれているときに、これらのコンポーネントは、既定で無効にします。 このホワイト ペーパーでは、IIS 6.0 を有効にする方法を説明する."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a>IIS 6.0 と ASP.NET 1.1 を実行します。
====================
> Windows Server 2003 には、IIS 6.0 および ASP.NET 1.1 の両方が含まれているときに、これらのコンポーネントは、既定で無効にします。 このホワイト ペーパーでは、IIS 6.0 および ASP.NET 1.1 を有効にする方法について説明し、IIS と ASP.NET から最適なパフォーマンスを取得するいくつかの構成設定をお勧めします。
> 
> ASP.NET 1.1、および IIS 6.0 に適用されます。


ASP.NET 1.1 は、インターネット インフォメーション サーバー (IIS) バージョン 6.0 の最新のバージョンも含まれます Windows Server 2003 に付属します。 IIS 6.0 および ASP.NET 1.1 がシームレスに統合するように設計し、ASP.NET の今すぐ新しい IIS 6.0 ワーカー プロセス モデルを既定値です。

## <a name="aspnet-11-is-not-installed-by-default"></a>既定では ASP.NET 1.1 がインストールされていません

Microsoft のサーバー オペレーティング システムの以前のバージョンとは異なりインターネット インフォメーション サーバー (IIS) が既定では有効です。また、ASP.NET 1.1。 IIS を有効にするための 2 つのオプションがあります。

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>サーバーの構成ウィザード、オプション 1: IIS を有効にします。

Windows Server 2003 では、新しい ' サーバーの構成ウィザード '、目的のモードで、サーバーを適切に構成するためが同梱されています。

-ウィザードを起動する必要がありますとしてログインする管理者がウィザードを実行する、メモに移動します開始 |。プログラム |管理ツールと select '、サーバーの構成 ' です。

選択すると、' サーバーの構成ウィザード ' 開始画面が表示されます。

![](aspnet-and-iis6/_static/image1.jpg)

をクリックして ' 次&gt;'。

![](aspnet-and-iis6/_static/image2.jpg)

をクリックして ' 次&gt;'

![](aspnet-and-iis6/_static/image3.jpg)

この画面で選択する必要があります ' を構成するオプションとアプリケーション サーバー (IIS、ASP.NET)。

をクリックして '次&gt;' です。

![](aspnet-and-iis6/_static/image4.jpg)

選択したら、サーバー、アプリケーション サーバーとして構成した後、この画面が表示されるどのようなその他の機能をインストールするかを確認します。 どちらのオプションは、既定で選択されます。 ASP.NET を自動的に有効にするを選択する必要があります。 ' ASP を有効にします。NET' です。

をクリックして '次&gt;' です。

![](aspnet-and-iis6/_static/image5.jpg)

新機能をインストールするオプションは、この画面が表示されます。

をクリックして '次&gt;' です。

![](aspnet-and-iis6/_static/image6.jpg)

選択したオプションのインストール中に、この画面が表示されます。 通常をサービスがインストールされているようにボックスが表示されるその他のダイアログを参照してください。 Windows Server 2003 インストール CD の場所をさらに求められます。

をクリックして '次&gt;' を完了するとします。

![](aspnet-and-iis6/_static/image7.jpg)

IIS 6.0 および ASP.NET 1.1 をサポートするために、Windows Server 2003 が構成されました - [完了] をクリックします。

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>オプション 2: IIS と ASP.NET を手動で構成する、IIS を有効にします。

'サーバーの構成ウィザード' を使用したくない場合、コントロール パネルから IIS 6.0 およびプログラムの追加と削除 を使用して ASP.NET 1.1 をインストールすることができます必要に応じて。

まず、コントロール パネルを開きます。

![](aspnet-and-iis6/_static/image8.jpg)

次に、' 追加/削除 Windows Components' するが開きます。 [Windows コンポーネント ウィザード] をクリックします。

![](aspnet-and-iis6/_static/image9.jpg)

強調表示を「アプリケーション サーバーの」を確認して [詳細] を '?' ボタンをクリックします。

![](aspnet-and-iis6/_static/image10.jpg)

ASP.NET をインストールするには確認 ' ASP です。NET' です。

Windows コンポーネント ウィザードに戻るには、[OK] をクリックします。 をクリックして ' 次&gt;'、Windows コンポーネント ウィザードからインストールを開始します。

![](aspnet-and-iis6/_static/image11.jpg)

通常をサービスがインストールされているようにボックスが表示されるその他のダイアログを参照してください。 Windows Server 2003 インストール CD の場所をさらに求められます。

インストールが完了したら、Windows コンポーネント ウィザードの最後の画面が表示されます。

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 および ASP.NET 1.1 が構成および使用可能な。

## <a name="recommended-settings"></a>推奨設定

IIS 6.0 と ASP.NET 1.1 を実行している場合は、ASP.NET から最適なパフォーマンスを得るには、推奨されるいくつかの構成設定があります。

- 構成のワーカー プロセス メモリの制限
- ワーカー プロセスのリサイクルを構成します。

### <a name="configuring-worker-process-memory-limits"></a>構成のワーカー プロセス メモリの制限

既定では IIS 6.0 は、IIS が使用できるメモリの量に制限を設定しません。 ASP です。NET のキャッシュ機能は、ため、キャッシュは積極的にメモリから未使用の項目を削除、メモリの制限事項に依存します。

IIS 6.0 の機能をリサイクルするメモリを構成することをお勧めします。 この open のインターネット インフォメーション サービス マネージャーを構成する (開始 |プログラム |管理ツール |インターネット インフォメーション サービス)。 されたら、開く 'アプリケーション プール' フォルダーを展開します。

アプリケーション プールごとに。

![](aspnet-and-iis6/_static/image13.jpg)

1. 例: アプリケーション プールを右クリックします。'DefaultAppPool' および 'プロパティ' を選択:

![](aspnet-and-iis6/_static/image14.jpg)

2. 次に、メモリのリサイクルのいずれかをクリックして有効にする ' 使用された最大メモリ (メガバイト):' です。 値は、サーバー上の物理 (virtual ではない) のメモリ量を超えるを指定できませんが適切に近似つまり 512 MB の物理メモリを選択 310 でサーバーの物理メモリの 60% です。 最大値を超えていないことに 800 MB を 2 GB のアドレス空間を使用する場合をお勧めします。 サーバーのメモリ アドレス空間が 3 GB の場合は、ワーカー プロセスの最大メモリ制限は 1、800 MB 程度になります。

![](aspnet-and-iis6/_static/image15.jpg)

'Apply' および 'OK' のプロパティ ダイアログを終了する をクリックします。 すべての使用可能なアプリケーション プールには、これを繰り返します。

### <a name="configuring-worker-recycling"></a>ワーカーのリサイクルを構成します。

既定では、IIS 6.0 は 29 時間ごとのワーカー プロセスをリサイクルする構成します。 これは、ASP.NET を実行するアプリケーションの積極的なビットと自動のワーカー プロセスのリサイクルが無効になっていることをお勧めします。

自動のワーカー プロセスのリサイクルを無効にするには、まずインターネット インフォメーション サービス マネージャーを開きます (開始 |プログラム |管理ツール |インターネット インフォメーション サービス)。 されたら、開く 'アプリケーション プール' フォルダーを展開します。

![](aspnet-and-iis6/_static/image16.jpg)

アプリケーション プールごとに。

1. 例: アプリケーション プールを右クリックします。'DefaultAppPool' および 'プロパティ' を選択:

![](aspnet-and-iis6/_static/image17.jpg)

2. オフにして ' (分単位) をワーカー プロセスのリサイクル:'。

![](aspnet-and-iis6/_static/image18.jpg)

'Apply' および 'OK' のプロパティ ダイアログを終了する をクリックします。 すべての使用可能なアプリケーション プールには、これを繰り返します。

## <a name="granting-write-access-to-the-file-system"></a>ファイル システムへの書き込みアクセス権の付与

アプリケーションがファイル システムへの書き込みアクセスを必要とし、NTFS を使用している場合は、アクセス制御リスト (ACL)、フォルダーまたはファイルの ASP.NET アクセスを許可するを変更する必要があります。

たとえば、ASP.NET を許可する、c:\inetpub\wwwroot への書き込みアクセス最初エクスプ ローラーを開き、ディレクトリに移動します。

![](aspnet-and-iis6/_static/image19.jpg)

次に、ディレクトリ、たとえば 'wwwroot' し、プロパティの右クリックします。 プロパティ ダイアログ ボックスが開いたら、[セキュリティ] タブを選択します。

![](aspnet-and-iis6/_static/image20.jpg)

C:\inetpub\wwwroot\ ディレクトリに特別なディレクトリでは、特別な IIS 6.0 グループ ' IIS\_WPG' 読み取りが既に許可されて&amp;実行、フォルダー内容の一覧および読み取りのアクセスを許可します。 ただし、書き込みアクセス許可を付与するには、書き込みを許可するチェック ボックスをクリックする必要があります。

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0 で、このフォルダーに対する書き込みアクセス許可できるようになりました。 次の手順で、他のフォルダーに対する書き込みアクセス許可を付与するに注意してください、IIS を追加する必要があります\_WPG グループが既に存在しない場合。

> [!CAUTION]
> IIS への書き込みアクセス権の付与\_WPG がこのディレクトリに書き込むために、ASP.NET アプリケーションを許可します。

## <a name="supporting-integrated-authentication-with-sql-server"></a>SQL Server との統合認証をサポートします。

統合認証では、Windows NT の認証を利用する SQL Server の SQL Server ログオン アカウントを検証します。 これにより、ユーザーは、標準の SQL Server ログオン プロセスを回避できます。 この方法で、ネットワーク ユーザーは、SQL Server、Windows NT のネットワーク セキュリティの処理からユーザーとパスワードの情報を取得するために、個別のログイン id またはパスワードを指定せず、SQL Server データベースをアクセスできます。

資格情報が格納されることはないアプリケーションの接続文字列内であるために、適切な選択は ASP.NET アプリケーションの統合認証を選択します。 はなく SQL への接続に使用する接続文字列を示しています。

`"server=localhost; database=Northwind;Trusted_Connection=true"`

この接続文字列は、SQL Server にアクセスしようとすると、アプリケーションの Windows 資格情報を使用する SQL Server に指示します。 ASP.NET/IIS 6 の場合、IIS でアカウントになります\_WPG グループ。

SQL Server と ASP.NET 間の統合認証を有効にする必要がありますまず統合認証または混合モード認証] - [SQL Server が構成されていることを確認するかどうかを判断するようにデータベース管理者です。 SQL Server がこれら 2 つのモードのいずれかである場合は、統合認証を使用できます。

SQL Server Enterprise Manager を開き (スタート |プログラム |Microsoft SQL Server |Enterprise Manager) では、適切なサーバーを選択し、[セキュリティ] フォルダーを展開します。

![](aspnet-and-iis6/_static/image22.jpg)

場合 ' BUILTINT\IIS\_WPG' グループが表示されていない、ログインを右クリックし、[新しいログイン] を選択します。

![](aspnet-and-iis6/_static/image23.jpg)

'名前:' ボックスに入力するか' [サーバー/ドメイン名] \IIS\_WPG' するかを開くには、Windows NT ユーザー/グループの選択の省略記号ボタンをクリックします。

![](aspnet-and-iis6/_static/image24.jpg)

現在のコンピューターの IIS を選択して\_'Add' アクセサーとピッカーを閉じるには、OK WPG グループをクリックします。

既定のデータベースおよびデータベースにアクセスする権限も設定する必要があります。 設定するには、既定のデータベースは 下 Northwind などが選択されている ドロップダウン リストから選択します。

![](aspnet-and-iis6/_static/image25.jpg)

次に、データベースへのアクセス タブをクリックします。

![](aspnet-and-iis6/_static/image26.jpg)

アクセスを許可するすべてのデータベースに対して [許可] チェック ボックスをクリックします。 必要があります、データベースの役割の選択に db をチェック\_所有者がログインが管理し、選択したデータベースを使用するすべての必要なアクセス許可を確認してください。

プロパティ ダイアログ ボックスを終了するには、[ok] をクリックします。 SQL Server の統合認証をサポートするために、ASP.NET アプリケーションが構成されているようになりました。

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>IIS 6.0 ネイティブ モードで ASP.NET 1.0 を実行しません。

IIS 6.0 では ASP.NET 1.0 は、IIS 5 互換モードでのみサポートされます。

IIS 5.0 互換モードで実行する ASP.NET 1.0 を構成するのには、インターネット サービス マネージャを開く、Web サイトを右クリックして、プロパティを選択します。

![](aspnet-and-iis6/_static/image27.jpg)

[サービス] タブに切り替えるし、を確認しますか。IIS 5.0 プロセス分離モードで WWW サービスを実行しますか?。

![](aspnet-and-iis6/_static/image28.jpg)
