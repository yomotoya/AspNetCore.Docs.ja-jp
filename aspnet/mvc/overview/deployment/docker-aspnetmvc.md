---
uid: mvc/overview/deployment/docker
title: Windows コンテナーへの ASP.NET MVC アプリケーションの移行
description: 既存の ASP.NET MVC アプリケーションを取得し、Windows Docker コンテナーで実行する方法について説明します。
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.prod: .net-framework
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 7a580c6c6236b375ea54ef4e9978fff6993d885a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2018
ms.locfileid: "29143190"
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a>Windows コンテナーへの ASP.NET MVC アプリケーションの移行

Windows コンテナーで既存の .NET Framework ベース アプリケーションを実行するとき、アプリに変更を加える必要はありません。 Windows コンテナーでアプリを実行するには、アプリを含む Docker イメージを作成し、コンテナーを起動します。 このトピックでは、既存の [ASP.NET MVC アプリケーション](http://www.asp.net/mvc)を取得し、Windows コンテナーに展開する方法について説明します。

既存の ASP.NET MVC アプリから開始し、Visual Studio を使用して発行した資産をビルドします。 Docker を使用し、アプリを含み、これを実行するイメージを作成します。 Windows コンテナーで実行されているサイトに移動し、アプリが動作していることを確認します。

この記事を読むには、Docker の基本的な知識があることが前提となります。 Docker の詳細については、「[Docker Overview](https://docs.docker.com/engine/understanding-docker/)」 (Docker の概要) を参照してください。

コンテナーで実行するアプリは、質問にランダムに回答する単純な Web サイトです。 このアプリは基本的な MVC アプリケーションであり、認証やデータベース ストレージはないため Web 層をコンテナーに移動することに集中できます。 今後のトピックで、コンテナー化されたアプリケーションに永続的な記憶域を移動して管理する方法を説明します。

アプリケーションを移行する手順は次のとおりです。

1. [イメージの資産をビルドする発行タスクを作成します。](#publish-script)
1. [アプリケーションを実行する Docker イメージをビルドします。](#build-the-image)
1. [イメージを実行する Docker コンテナーを開始します。](#start-a-container)
1. [ブラウザーを使用してアプリケーションを検証します。](#verify-in-the-browser)

[完成したアプリケーション](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator)が GitHub にあります。

## <a name="prerequisites"></a>必須コンポーネント

開発用コンピューターを実行している必要があります

- [Windows 10 Anniversary Update](https://www.microsoft.com/software-download/windows10/) (以降) または [Windows Server 2016](https://www.microsoft.com/cloud-platform/windows-server) (以降)。
- [Docker for Windows](https://docs.docker.com/docker-for-windows/) - バージョン Stable 1.13.0 または 1.12 Beta 26 (以降のバージョン)
- [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx)です。

> [!IMPORTANT]
> Windows Server 2016 を使用している場合は、「[コンテナー ホストの展開 - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment)」の指示に従ってください。

Docker をインストールして開始した後、トレイ アイコンを右クリックし、**[Switch to Windows containers]** (Windows コンテナーに切り替え) を選択します。 これは Windows をベースに Docker イメージを実行するために必要です。 このコマンドの実行には数秒かかります。

![Windows コンテナー][windows-container]

## <a name="publish-script"></a>発行スクリプト

Docker イメージに読み込む必要があるすべての資産を 1 か所に集めます。 Visual Studio の **[発行]** コマンドを使用してアプリの発行プロファイルを作成できます。 このプロファイルですべての資産を 1 つのディレクトリ ツリーに集めます。このチュートリアルの後半で、そのディレクトリ ツリーをターゲット イメージにコピーします。

**発行の手順**

1. Visual Studio で Web プロジェクトを右クリックし、**[発行]** を選択します。
1. **[カスタム プロファイル]** ボタンをクリックし、方法として **[ファイル システム]** を選択します。
1. ディレクトリを選択します。 規則により、ダウンロードしたサンプルには `bin\Release\PublishOutput` が使用されます。

![接続の発行][publish-connection]

**[設定]** タブの **[ファイル発行オプション]** のセクションを開きます。**[発行中にプリコンパイルする]** を選択します。 この最適化は、Docker コンテナー内のビューをコンパイルすることを意味し、プリコンパイル済みのビューをコピーすることになります。

![発行の設定][publish-settings]

**[発行]** をクリックすると、Visual Studio で必要なすべての資産がコピー先フォルダーにコピーされます。

## <a name="build-the-image"></a>イメージのビルド

Dockerfile に Docker イメージを定義します。 Dockerfile は、基本イメージ、追加のコンポーネント、実行するアプリ、その他の構成イメージに関する指示を含みます。  Dockerfile は、イメージを作成する `docker build` コマンドの入力です。

[Docker Hub](https://hub.docker.com/r/microsoft/aspnet/) にある `microsoft/aspnet` イメージに基づいてイメージをビルドします。
基本イメージである `microsoft/aspnet` は、Windows Server イメージです。 Windows Server Core、IIS および ASP.NET 4.6.2 が含まれています。 このイメージをコンテナー内で実行すると、IIS とインストールされている Web サイトが自動的に起動します。

イメージを作成する Dockerfile は、次のようになります。

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

この Dockfile に `ENTRYPOINT` コマンドは使用されていません。 使用する必要はありません。 IIS と Windows Server を実行するときに、IIS プロセスは aspnet の基本イメージで起動するように構成がエントリ ポイントです。

Docker ビルド コマンドを実行し、ASP.NET アプリを実行するイメージを作成します。 これを行うには、プロジェクトのディレクトリに、PowerShell ウィンドウを開くし、ソリューション ディレクトリに次のコマンドを入力します。

```console
docker build -t mvcrandomanswers .
```

このコマンドは、Dockerfile の手順に従って、新しいイメージを構築名前付け (・ t がタグ付け) mvcrandomanswers としてイメージ。 その手順には、[Docker Hub](http://hub.docker.com) から基本イメージをプルし、それからそのイメージにアプリを追加する作業が含まれることがあります。

そのコマンドの完了後、`docker images` コマンドを実行して新しいイメージの情報を参照できます。

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

IMAGE ID はコンピューターによって異なります。 では、アプリを実行しましょう。

## <a name="start-a-container"></a>コンテナーの開始

コンテナーを開始するには、次の `docker run` コマンドを実行します。

```console
docker run -d --name randomanswers mvcrandomanswers
```

`-d` 引数は、デタッチ モードでイメージを開始するよう Docker に指示します。 つまり、Docker イメージは現在のシェルから切断された状態で実行されます。

Docker の多くの例では、コンテナーとホストのポートにマップする-p を参照してください可能性があります。 既定の aspnet イメージでは、既にポート 80 でリッスンし、それを公開するコンテナーを構成します。 

`--name randomanswers` は、実行するコンテナーに名前を付けます。 この名前は、ほとんどのコマンドでコンテナー ID の代わりに使用できます。

`mvcrandomanswers` は、開始するイメージの名前です。

## <a name="verify-in-the-browser"></a>ブラウザーでの確認

> [!NOTE]
> 現在の Windows コンテナーのリリースを参照することはできません`http://localhost`です。
> これは WinNAT での既知の動作によるものであり、将来のリリースで修正される予定です。 解決されるまでは、コンテナーの IP アドレスを使用する必要があります。

コンテナーの開始後、実行中のコンテナーにブラウザーから接続できるように、コンテナーの IP アドレスを見つけます。

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

IPv4 アドレスを使用して、実行中のコンテナーに接続`http://172.31.194.61`に示す例です。 その URL をブラウザーに入力すると、実行中のサイトが表示されます。

> [!NOTE]
> 一部の VPN またはプロキシ ソフトウェアが原因でサイトに移動できない場合があります。
> コンテナーが動作していることを確認するために、それらを一時的に無効にできます。

GitHub のサンプル ディレクトリに含まれる [PowerShell スクリプト](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1)は、これらのコマンドを実行します。 PowerShell ウィンドウを開き、ソリューション ディレクトリに移動して、次のように入力します。

```console
./run.ps1
```

上のコマンドによりイメージをビルドし、コンピューターにイメージの一覧を表示して、コンテナーを開始した後、そのコンテナーの IP アドレスを表示します。

コンテナーを停止するには、`docker
stop` コマンドを実行します。

```console
docker stop randomanswers
```

コンテナーを削除するには、`docker rm` コマンドを実行します。

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Windows コンテナーへの切り替え"
[publish-connection]: media/aspnetmvc/PublishConnection.png "ファイル システムへの発行"
[publish-settings]: media/aspnetmvc/PublishSettings.png "発行の設定"
