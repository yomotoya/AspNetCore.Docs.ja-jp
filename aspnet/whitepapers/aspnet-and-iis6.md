---
uid: whitepapers/aspnet-and-iis6
title: IIS 6.0 と ASP.NET 1.1 を実行している |Microsoft Docs
author: rick-anderson
description: Windows Server 2003 には、IIS 6.0 と ASP.NET 1.1 の両方が含まれているときに、これらのコンポーネントは、既定で無効にします。 このホワイト ペーパーでは、IIS 6.0 を有効にする方法を説明する.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 38cd0abc1e9133b9b86cff6dd2759ce98ac5a115
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837741"
---
<a name="running-aspnet-11-with-iis-60"></a>IIS 6.0 と ASP.NET 1.1 を実行します。
====================
> Windows Server 2003 には、IIS 6.0 と ASP.NET 1.1 の両方が含まれているときに、これらのコンポーネントは、既定で無効にします。 このホワイト ペーパーでは、IIS 6.0 と ASP.NET 1.1 を有効にする方法について説明し、IIS と ASP.NET から最適なパフォーマンスを取得するいくつかの構成設定を推奨します。
> 
> ASP.NET 1.1 および IIS 6.0 に適用されます。


ASP.NET 1.1 に付属 Windows Server 2003 は、インターネット インフォメーション サーバー (IIS) バージョン 6.0 の最新のバージョンも含まれます。 IIS 6.0 および ASP.NET 1.1 がシームレスに統合するように設計および ASP.NET の新しい IIS 6.0 ワーカー プロセス モデルを今すぐ既定値します。

## <a name="aspnet-11-is-not-installed-by-default"></a>既定では ASP.NET 1.1 がインストールされていません

Microsoft のサーバー オペレーティング システムの以前のバージョンとは異なりインターネット インフォメーション サーバー (IIS) が有効でない既定ではまた ASP.NET 1.1 ではありません。 IIS を有効にするための 2 つのオプションがあります。

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>サーバーの構成ウィザード、オプション 1: IIS を有効にします。

Windows Server 2003 には、新しい ' サーバーの構成ウィザード '、目的のモードで、サーバーを適切に構成するためが付属しています。

移動する - 管理者としてログインする必要がありますウィザードを実行するに注意してください - ウィザードを起動します開始 |。プログラム |管理ツールと select '、サーバーの構成 '。

1 回選択した 'サーバーの構成ウィザード' の最初の画面を表示する必要があります。

![](aspnet-and-iis6/_static/image1.jpg)

クリックして ' [次へ] &gt;'。

![](aspnet-and-iis6/_static/image2.jpg)

クリックして ' [次へ] &gt;'

![](aspnet-and-iis6/_static/image3.jpg)

この画面で選択する必要があります ' を構成するオプションとしてのアプリケーション サーバー (IIS、ASP.NET)。

クリックして ' [次へ] &gt;'。

![](aspnet-and-iis6/_static/image4.jpg)

アプリケーション サーバーとして、サーバーの構成を選択すると、この画面が表示されますどのような追加機能をインストールするかを確認します。 どちらのオプションは、既定で選択されます。 ASP.NET を自動的に有効にするを選択する必要があります。 ' ASP を有効にします。NET'。

クリックして ' [次へ] &gt;'。

![](aspnet-and-iis6/_static/image5.jpg)

この画面がインストールのオプションが表示されます。

クリックして ' [次へ] &gt;'。

![](aspnet-and-iis6/_static/image6.jpg)

選択したオプションのインストール中に、この画面が表示されます。 通常、サービスがインストールされているボックスが表示されるその他のダイアログ ボックスが表示されます。 さらに、Windows Server 2003 インストール CD の場所を求められます。

クリックして '[次へ] &gt;' を完了するとします。

![](aspnet-and-iis6/_static/image7.jpg)

IIS 6.0 および ASP.NET 1.1 をサポートするためには、Windows Server 2003 が構成されました - [完了] をクリックします。

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>オプション #2 - 手動で IIS と ASP.NET を構成する、IIS を有効にします。

'サーバーの構成ウィザード' を使用したくない場合、コントロール パネルから IIS 6.0 およびプログラムの追加と削除' を使用して ASP.NET 1.1 をインストールすることができます必要に応じて。

最初に、コントロール パネルを開きます。

![](aspnet-and-iis6/_static/image8.jpg)

次に、' 追加/削除 Windows コンポーネント、Windows コンポーネント ウィザード が開きますをクリックします。

![](aspnet-and-iis6/_static/image9.jpg)

強調表示し、「アプリケーション サーバー」を確認し、'詳細' でしょうか。 ボタンをクリックします。

![](aspnet-and-iis6/_static/image10.jpg)

ASP.NET をインストールするには、次のように確認します。 ' ASP します。NET'。

Windows コンポーネント ウィザードに戻るには、[OK] をクリックします。 クリックして '[次へ] &gt;' からのインストールを開始する Windows コンポーネント ウィザード。

![](aspnet-and-iis6/_static/image11.jpg)

通常、サービスがインストールされているボックスが表示されるその他のダイアログ ボックスが表示されます。 さらに、Windows Server 2003 インストール CD の場所を求められます。

インストールが完了したら、Windows コンポーネント ウィザードの最後の画面が表示されます。

![](aspnet-and-iis6/_static/image12.jpg)

IIS 6.0 および ASP.NET 1.1 が構成および使用可能な。

## <a name="recommended-settings"></a>推奨設定

IIS 6.0 と ASP.NET 1.1 を実行している場合は、ASP.NET から最適なパフォーマンスを得るに推奨されるいくつかの構成設定があります。

- ワーカー プロセス メモリ制限の構成
- ワーカー プロセスのリサイクルを構成します。

### <a name="configuring-worker-process-memory-limits"></a>ワーカー プロセス メモリ制限の構成

既定で IIS 6.0 は、IIS が使用できるメモリの量に制限を設定しません。 ASP します。NET のキャッシュ機能は、キャッシュは積極的にメモリから未使用の項目を削除するために、メモリの制限事項に依存します。

IIS 6.0 の機能をリサイクルするメモリを構成することをお勧めします。 この開いているインターネット インフォメーション サービス マネージャーを構成する (開始 |プログラム |管理ツール |インターネット インフォメーション サービス)。 開いている場合は、' アプリケーション プール ' フォルダーを展開します。

各アプリケーション プール。

![](aspnet-and-iis6/_static/image13.jpg)

1. 例: アプリケーション プールを右クリックします。'DefaultAppPool' と 'プロパティ' を選択します。

![](aspnet-and-iis6/_static/image14.jpg)

2. 次に、いずれかをクリックしてメモリのリサイクルを有効にする ' 使用された最大メモリ (メガバイト単位)。 '。 値することはできません、サーバー上の物理 (仮想) のメモリ量以上で、十分な概算は 512 mb の物理メモリの選択 310 サーバーなどの物理メモリの 60% です。 最大値を超えていないこと 800 MB 2 GB のアドレス空間を使用する場合もお勧めします。 サーバーのメモリ アドレス空間が 3 GB の場合、ワーカー プロセスのメモリの上限は 1、800 MB の最高になります。

![](aspnet-and-iis6/_static/image15.jpg)

プロパティ ダイアログを終了するには、適用 と OK をクリックします。 すべての利用可能なアプリケーション プールには、これを繰り返します。

### <a name="configuring-worker-recycling"></a>ワーカーのリサイクルを構成します。

既定では、IIS 6.0 は 29 時間ごとのワーカー プロセスをリサイクルする構成します。 これは、ASP.NET を実行するアプリケーションの少しアグレッシブなと自動のワーカー プロセスのリサイクルが無効になっていることをお勧めします。

自動のワーカー プロセスのリサイクルを無効にするには、まずインターネット インフォメーション サービス マネージャーを開きます (開始 |プログラム |管理ツール |インターネット インフォメーション サービス)。 開いている場合は、' アプリケーション プール ' フォルダーを展開します。

![](aspnet-and-iis6/_static/image16.jpg)

各アプリケーション プール。

1. 例: アプリケーション プールを右クリックします。'DefaultAppPool' と 'プロパティ' を選択します。

![](aspnet-and-iis6/_static/image17.jpg)

2. オフに ' (分単位) をワーカー プロセスのリサイクル:'。

![](aspnet-and-iis6/_static/image18.jpg)

プロパティ ダイアログを終了するには、適用 と OK をクリックします。 すべての利用可能なアプリケーション プールには、これを繰り返します。

## <a name="granting-write-access-to-the-file-system"></a>ファイル システムへの書き込みアクセス許可

アプリケーションがファイル システムへの書き込みアクセスを必要とし、NTFS を使用している場合は、変更、アクセス制御リスト (ACL)、フォルダーまたはファイルの ASP.NET のアクセス権を付与する必要があります。

たとえば、ASP.NET を許可する、c:\inetpub\wwwroot への書き込みアクセス最初エクスプ ローラーを開きし、ディレクトリに移動します。

![](aspnet-and-iis6/_static/image19.jpg)

次に、ディレクトリ、たとえば 'wwwroot' し、プロパティの右クリックします。 プロパティ ダイアログ ボックスが開いたら、[セキュリティ] タブを選択します。

![](aspnet-and-iis6/_static/image20.jpg)

C:\inetpub\wwwroot\ ディレクトリに特別なディレクトリでは、特別な IIS 6.0 グループ ' IIS\_WPG' は既に読み取りが付与されて&amp;実行、フォルダーの内容の一覧と読み取りアクセス許可。 ただし、書き込みアクセス許可を付与する書き込みを許可するチェック ボックスをクリックする必要があります。

![](aspnet-and-iis6/_static/image21.jpg)

IIS 6.0 で、このフォルダーに対する書き込みアクセス許可できるようになりました。 -以下の手順に従って、他のフォルダーに対する書き込みアクセス許可を付与するに注意してください、IIS を追加する必要があります\_WPG グループが既に存在しない場合。

> [!CAUTION]
> IIS への書き込みアクセス権の付与\_WPG が ASP.NET アプリケーションをこのディレクトリへの書き込みを許可します。

## <a name="supporting-integrated-authentication-with-sql-server"></a>SQL Server による統合認証をサポートしています。

統合認証は、Windows NT 認証を利用する SQL Server の SQL Server ログオン アカウントを検証できます。 これにより、ユーザーは、標準の SQL Server ログオン プロセスを回避できます。 この方法により、ネットワーク ユーザーは、SQL Server、Windows NT のネットワーク セキュリティの処理から、ユーザーとパスワードの情報を取得するために、個別のログインの id またはパスワードを指定せず、SQL Server データベースをアクセスできます。

アプリケーションの接続文字列内でこれまで格納されている資格情報はないためには、ASP.NET アプリケーションの統合認証を選択することをお勧めします。 はなく SQL への接続に使用する接続文字列はようになります。

`"server=localhost; database=Northwind;Trusted_Connection=true"`

この接続文字列は、SQL Server にアクセスしようとすると、アプリケーションの Windows 資格情報を使用する SQL Server に指示します。 Iis のアカウントであるこの ASP.NET/IIS 6 の場合は\_WPG グループ。

SQL Server と ASP.NET 間で統合認証を有効にする最初の統合認証または混合モード認証 - SQL Server が構成されていることを確認する必要がありますこれを確認するようにデータベース管理者に確認します。 SQL Server がこれら 2 つのモードのいずれかである場合は、統合認証を使用できます。

SQL Server Enterprise Manager を開きます (開始 |プログラム |Microsoft SQL Server |Enterprise Manager) は、適切なサーバーを選択し、セキュリティ フォルダーを展開します。

![](aspnet-and-iis6/_static/image22.jpg)

場合 ' BUILTINT\IIS\_WPG' グループが表示されていない、ログインを右クリックし、[新しいログイン] を選択します。

![](aspnet-and-iis6/_static/image23.jpg)

'名:' テキスト ボックスに入力するか' [サーバー/ドメイン名] \IIS\_WPG' Windows NT ユーザー/グループの選択を開く省略記号ボタンをクリックしてまたは。

![](aspnet-and-iis6/_static/image24.jpg)

現在のコンピューターの IIS 選択\_'Add' アクセサーとピッカーを閉じてよい WPG グループをクリックします。

既定のデータベースおよびデータベースにアクセスするアクセス許可も設定する必要があります。 設定するには、既定のデータベースは 例: 次の Northwind が選択されている ドロップダウン リストから選択します。

![](aspnet-and-iis6/_static/image25.jpg)

次に、データベースへのアクセス タブをクリックします。

![](aspnet-and-iis6/_static/image26.jpg)

アクセスを許可するすべてのデータベースの [許可] チェック ボックスをクリックします。 必要になります、データベースの役割の選択に db をチェック\_所有者は、ログインを管理し、選択したデータベースを使用して、すべての必要なアクセス許可を持つことを確認します。

プロパティ ダイアログ ボックスを終了するには、[ok] をクリックします。 SQL Server の統合認証をサポートするために、ASP.NET アプリケーションが構成されているようになりました。

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>ネイティブ モードの IIS 6.0 で ASP.NET 1.0 を実行しないでください。

IIS 6.0 で ASP.NET 1.0 は、IIS 5 互換モードでのみサポートされます。

IIS 5.0 互換モードで実行する ASP.NET 1.0 を構成するには、インターネット サービス マネージャを開く Web サイトを右クリックし、プロパティを選択します。

![](aspnet-and-iis6/_static/image27.jpg)

サービス タブに切り替えて、チェックでしょうか。IIS 5.0 分離モードで WWW サービスを実行します。

![](aspnet-and-iis6/_static/image28.jpg)
