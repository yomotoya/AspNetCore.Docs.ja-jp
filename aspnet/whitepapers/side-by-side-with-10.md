---
uid: whitepapers/side-by-side-with-10
title: .NET Framework 1.0 および 1.1 の ASP.NET で並列実行 |Microsoft Docs
author: rick-anderson
description: このホワイト ペーパーでは、ASP.NET Web アプリケーションをフレームのいずれかのバージョンで実行することにより、コンピューターに .NET 1.0 と .NET 1.1 の両方をインストールする方法について説明しています.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: b5457a62e127ba555674fbe3b9f75552cad041c7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832272"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>.NET Framework 1.0 および 1.1 の ASP.NET で並列実行
====================
> このホワイト ペーパーでは、ASP.NET Web アプリケーションをフレームワークのいずれかのバージョンで実行することにより、コンピューターに .NET 1.0 と .NET 1.1 の両方をインストールする方法について説明します。
> 
> ASP.NET 1.0 と ASP.NET 1.1 に適用されます。


ASP.NET では、アプリケーションが同じコンピューターでは、インストールされているものの、異なるバージョンの .NET Framework を使用して、場合、並行して実行するといいます。 次のトピックでは、サイド バイ サイドで実行するための ASP.NET アプリケーションを構成する方法について説明しする詳細な手順を提供します。

- [インストール中に .NET Framework version 1.0 への Web アプリケーションのマッピングを管理します。](#1)
- [.NET Framework の特定のバージョンへの Web アプリケーションをマップします。](#2)
- [Web サイトを使用している .NET Framework のバージョンを確認します。](#3)

これまでは、コンポーネントまたはアプリケーションがコンピューターに更新されると、以前のバージョンが削除され、新しいバージョンに置き換えられます。 新しいバージョンが以前のバージョンと互換性がない場合これは、通常、コンポーネントやアプリケーションを使用するその他のアプリケーションを中断します。 .NET Framework では、これにより、アセンブリまたは同時に同じコンピューターにインストールするアプリケーションの複数のバージョン、サイド バイ サイドで実行するためのサポートを提供します。 複数のバージョンを同時にインストールできるため、マネージ アプリケーションは、別のバージョンを使用するアプリケーションに影響を与えずに使用するバージョンを選択できます。

既定では、.NET Framework version 1.1 のインストール中に既存のすべての ASP.NET アプリケーションが自動的に再構成、.NET Framework の最新バージョンを使用します。 既定の .NET Framework 1.1 ASP.NET アプリケーションにしたくない場合はクリックして[ここ](#1)のインストール時にこれを回避する方法について説明します。

.NET Framework 1.1 に、Web サーバーを更新し、1 つまたは複数の Web アプリケーションを .NET Framework 1.0 を実行すると場合、は、インターネット インフォメーション サービス (IIS) のスクリプト マップを更新する必要があります。 スクリプトのマッピングは、.NET Framework のバージョンを特定の Web アプリケーションの .aspx ファイルの拡張子をマップするメカニズムです。 クリックして[ここ](#2)に .NET Framework の特定のバージョンへの Web アプリケーションにマップする方法について説明します。

インターネットの情報マネージャーか、ASP.NET IIS 登録ツールを使用することができます (Aspnet\_regiis.exe) を特定の Web アプリケーションを実行して .NET Framework バージョンを検索します。 クリックして[ここ](#3)に Web サイトを使用している .NET Framework のバージョンを確認する方法について説明します。

.NET Framework 1.1 への移行は、.NET Framework の各バージョンが、独自の Machine.config ファイルを使用するときに考慮を 1 つのインポート: その結果、Web 管理者が Machine.config ファイルに変更を加えた場合これらの変更は、.NET Framework 1.1 の Machine.config ファイルに移行する必要があります。

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>インストール中に .NET Framework 1.0 に Web アプリケーションのマッピングの保持

既定では、既存のすべての ASP.NET アプリケーションは、.NET Framework の新しいバージョンを使用して、インストール時に自動的に再構成されます。 .NET Framework の新しいバージョンを使用して、アプリケーションできますフル活用および新しいリリースに含まれる新しい機能強化。 同時に、Web 管理者は、どのアプリケーションをきめ細かく制御を望む場合がありますが更新、自動再マップのすべての既存の ASP.NET アプリケーションの .NET Framework のインストール中に防ぐことができます。

.NET Framework の新しいバージョンへの ASP.NET アプリケーション全体の自動再割り当てを防ぐためには、Web 管理者は、Dotnetfx.exe のセットアップ プログラムで/noaspupgrade コマンド ライン オプションを使用できます。

**新しいバージョンに ASP.NET アプリケーションの合計の再割り当てを回避するには**

1. 移動して**開始**します。
2. をクリックして**実行**します。
3. 「**cmd**」と入力します。
4. **[OK]** をクリックします。  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. コマンド プロンプトから、.NET Framework のインストールを開始する次の行を入力します: **Dotnetfx.exe/c:"/noaspupgrade をインストールしますか?** します。  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. クリックして**はい**Microsoft .NET Framework 1.1 のセットアップ時にします。 これにより、.NET Framework 1.1 のセットアップ プロセスが開始されます。  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>.NET Framework の特定のバージョンへの Web アプリケーションをマップします。

.NET Framework のバージョンごとに、ASP.NET IIS 登録ツールのバージョンが含まれています (Aspnet\_regiis.exe)。 このツールには、管理者 Web アプリケーションが .NET Framework の特定のバージョンで実行することを指定することができます。 これは Web アプリケーションを .NET Framework のバージョンのマッピングと呼ばれます。 管理者は、Aspnet を選択する必要があります\_regiis.exe、Web アプリケーションと関連付けられる .NET Framework のバージョンに対応します。 たとえば、Web サイトが .NET Framework 1.1 を使用するように指定する必要のある管理者は、Aspnet を使用する必要があります\_regiis.exe .NET Framework 1.1 に付属しています。

Aspnet\_regiis.exe バージョン 1.0 には。

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis

Aspnet\_regiis.exe バージョン 1, 1 にあります。

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis

Aspnet\_regiis.exe スクリプト、Web アプリケーションのマッピングの 2 つのオプションを提供します。

- **-s**マップを設定、スクリプトのパスとその子ディレクトリ。
- **-sn**パスだけでスクリプト マップを設定します。

パスは、W3SVC/ルートの形式で定義されている Web アプリケーション IIS メタデータ パスの定義/{WebSiteNumber}/{アプリケーション\_名}。 たとえば、既定の Web サイトの下にあるポータルと呼ばれる Web アプリケーション、メタベースのパスは W3SVC/1/ルート/ポータル。

![](side-by-side-with-10/_static/image4.gif)

メタベース パスを取得するメタベース エディターと呼ばれるツールを使用することもできます。 注意してください。 このツールをダウンロードするには、Microsoft サポート サイトから[ https://support.microsoft.com/default.aspx?scid=kb; en-232068; 米国。](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- 実行 Aspnet\_マップとその subapplication regiis.exe-s W3SVC/1/ルート/ポータル IIS ポータルを更新するスクリプトを作成します。  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Aspnet の実行\_regiis.exe sn-たとえば、W3SVC/1/ルート/ポータル ポータルの IIS スクリプトを更新するマップのポータルでアプリケーションに影響を与えずに? s サブディレクトリ。  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Web アプリケーションを使用している .NET Framework バージョンを確認します。

管理者は、インターネット サービス マネージャーを使用して、Web サイトで実行される .NET Framework のバージョンを検索します。 オペレーティング システムのバージョンでは、異なる方法で、インターネット サービス マネージャーを起動します。 サービス マネージャーを起動するには、次の手順に従います。

**インターネット サービス マネージャーを起動するには**

1. 移動して**開始**します。
2. をクリックして**実行**します。
3. 型**inetmgr**します。  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. インターネット サービス マネージャからでは、Web アプリケーションを知りたい .NET Framework のバージョンを選択します。  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Web アプリケーションを右クリックし、をクリックして**プロパティ。**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. プロパティ ウィンドウで、次のように選択します。**構成します。**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. アプリケーションのマッピング テーブルから選択 **.aspx**、 をクリック**編集**します。  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. **実行可能ファイル**テキスト ボックス、スクロールしてバージョンのディレクトリを参照してください。 バージョンのディレクトリが v.1.1.4322 の場合は、アプリケーションは、.NET Framework 1.1 にマップされます。 逆に、バージョンのディレクトリが v1.0.3705 の場合は、アプリケーションが .NET Framework 1.0 にマップされます。  
  
    ![](side-by-side-with-10/_static/image12.gif)
