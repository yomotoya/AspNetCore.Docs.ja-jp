---
title: "Visual Studio Tools for Docker と ASP.NET Core"
description: "この記事では、Visual Studio 2017 ツールと Docker for Windows を使用して、ASP.NET Core アプリケーションをコンテナー化する方法を説明します。"
keywords: "Docker, ASP.NET Core, Visual Studio, コンテナー"
author: spboyer
ms.author: scaddie
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.prod: asp.net-core
ms.technology: aspnet
ms.assetid: 1f3b9a68-4dea-4b60-8cb3-f46164eedbbf
uid: publishing/vs-tools-for-docker
ms.openlocfilehash: 2d8e337141ae4e0d0258f1d7546510b0ab077e39
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="visual-studio-tools-for-docker"></a>Visual Studio Tools for Docker

[Docker for Windows](https://docs.docker.com/docker-for-windows/install/) を含む [Microsoft Visual Studio 2017](https://www.visualstudio.com/) では、Windows と Linux コンテナーを使用した .NET Framework および .NET Core の Web アプリケーションやコンソール アプリケーションのビルド、デバッグ、実行をサポートします。

## <a name="prerequisites"></a>必須コンポーネント

- .NET Core ワークロードを含む [Microsoft Visual Studio 2017](https://www.visualstudio.com/)
- [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>インストールとセットアップ

.NET Core ワークロードで [Microsoft Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) をインストールします。 Visual Studio をインストール済みの場合は、[Visual Studio のインストールを変更して](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) .NET Core ワークロードを追加できます。

Docker をインストールする場合、「[Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)」 (Docker for Windows: インストール前に知っておくべきこと) の情報を確認し、[Docker for Windows](https://docs.docker.com/docker-for-windows/install/) をインストールします。

Docker for Windows では、**[共有ドライブ](https://docs.docker.com/docker-for-windows/#shared-drives)**を設定する必要があります。 この設定は、ボリュームのマップとデバッグのサポートで必要です。

システム トレイの Docker アイコンを右クリックして **[設定]** をクリックし、**[Shared Drives]\(共有ドライブ\)** を選択します。 Docker によるファイルの格納先となるドライブを選択して、変更を適用します。

![共有ドライブ](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

## <a name="create-an-aspnet-web-application-and-add-docker-support"></a>ASP.NET Web アプリケーションを作成し、Docker のサポートを追加する

Visual Studio を使用して、新しい ASP.NET Core Web アプリケーションを作成できます。 アプリケーションが読み込まれたら、**[プロジェクト] メニュー**の **[Add Docker Support]\(Docker サポートの追加\)** を選択するか、ソリューション エクスプローラーからプロジェクトを右クリックし、**[追加]** > **[Docker Support]\(Docker サポート\)** の順に選択します。

*[プロジェクト] メニュー*

![[プロジェクト]、「Add Docker Support」 (Docker サポートの追加)](./visual-studio-tools-for-docker/_static/project-add-docker-support.png)

*[プロジェクト] のコンテキスト メニュー*

![[追加]、[Docker Support]\(Docker サポート\) を右クリック](./visual-studio-tools-for-docker/_static/right-click-add-docker-support.png)

プロジェクトに Docker サポートを追加する場合、Windows または Linux のいずれかのコンテナーを選択できます。 (Docker ホストが同じコンテナーの種類を実行している必要があります。 実行中の Docker インスタンスでコンテナーの種類を変更する必要がある場合、システム トレイの **Docker** アイコンを右クリックして、**[Switch to Windows containers]\(Windows コンテナーに切り替える\)**、または **[Switch to Linux containers]\(Linux コンテナーに切り替える\)** を選択します。) 

プロジェクトに次のファイルが追加されます。

- **Dockerfile**: ASP.NET Core アプリケーション用の Docker ファイルは、[microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) イメージに基づきます。 このイメージには、Just-In-Time (JIT) にコンパイルされてパフォーマンスが向上した ASP.NET Core NuGet パッケージが含まれます。 .NET Core のコンソール アプリケーションを構築するとき、Dockerfile の FROM は最新の [microsoft/dotnet](https://hub.docker.com/r/microsoft/dotnet) イメージを参照します。   
- **docker-compose.yml**: docker-compose build/run と共に実行され構築されるイメージの集合を定義するために使用されるベースとなる Docker Compose ファイル。   
- **docker-compose.dev.debug.yml**: 構成がデバッグに設定されている場合の反復的な変更用の追加の docker-compose ファイル。 Visual Studio は -f docker-compose.yml -f docker-compose.dev.debug.yml を呼び出し、これらをマージします。 この構成ファイルは、Visual Studio の開発ツールによって使用されます。   
- **docker-compose.dev.release.yml**: リリース定義をデバッグする、追加の Docker Compose ファイル。 実稼働イメージの内容を変更しないように、デバッガーをボリューム マウントします。  

*docker-compose.yml* ファイルには、プロジェクトの実行時に作成されたイメージの名前が含まれています。 

```
version '2'

services:
  hellodockertools:
    image:  user/hellodockertools${TAG}
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80"
``` 

この例では、`image: user/hellodockertools${TAG}` によってアプリケーションが**デバッグ** モードで実行されるとき、`user/hellodockertools:dev` が生成され、**リリース** モードで実行されるときに、`user/hellodockertools:latest` が生成されます。 

イメージをレジストリにプッシュする場合、`user` を [Docker Hub](https://hub.docker.com/) のユーザー名に変更してください。 たとえば、`spboyer/hellodockertools` や、構成に応じて `privateregistry.domain.com/` のプライベート レジストリ URL に変更します。

### <a name="debugging"></a>デバッグ

ツールバーのデバッグ ドロップダウンから **[Docker]** を選択し、F5 を使用してアプリケーションのデバッグを開始します。 

- (キャッシュにない場合) *microsoft/aspnetcore* イメージが取得されます。
- *ASPNETCORE_ENVIRONMENT* は、コンテナー内で Development に設定されます。
- ポート 80 が公開され、localhost 用に動的に割り当てられているポートにマップされます。 このポートは、docker ホストによって決定され、docker ps でクエリを実行することができます。 
- アプリケーションがコンテナーにコピーされます。
- 動的に割り当てられたポートを使用して、デバッガーがコンテナーにアタッチされ、既定のブラウザーが起動します。 

構築された結果の Docker イメージは *microsoft/aspnetcore* イメージをベース イメージとするアプリケーションの *dev* イメージです。

**注:** デバッグ構成では、反復にボリューム マウントを使用するため、dev イメージにはアプリのコンテンツは含まれません。 イメージをプッシュするには、リリース構成を使用します。

```console
REPOSITORY                  TAG         IMAGE ID            CREATED         SIZE
spboyer/hellodockertools    dev         0b6e2e44b3df        4 minutes ago   268.9 MB
microsoft/aspnetcore        1.0.1       189ad4312ce7        5 days ago      268.9 MB
```

アプリケーションは、`docker ps` コマンドを実行して参照できるコンテナーを使用して実行されます。

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   4 minutes ago       Up 4 minutes        0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="edit-and-continue"></a>エディット コンティニュ

静的なファイルや razor テンプレート ファイル (*.cshtml*) に対する変更は、コンパイルをする必要はなく、自動的に更新されます。 変更を行って保存し、タップしてブラウザーを更新し、更新を確認します。  

コード ファイルを変更した場合、コンパイルを行い、コンテナー内で Kestrel を再起動する必要があります。 変更後、CTRL キーを押しながら F5 キーを使用して、手順を実行して、コンテナー内でアプリケーションを起動します。 Docker コンテナーは再構築されたり、停止されたりすることはありません。コマンド ラインから `docker ps` を使用すると、元のコンテナーが 10 分前と同じ状態で実行されていることを確認できます。 

```console
CONTAINER ID        IMAGE                          COMMAND               CREATED             STATUS              PORTS                   NAMES
3f240cf686c9        spboyer/hellodockertools:dev   "tail -f /dev/null"   10 minutes ago      Up 10 minutes       0.0.0.0:32769->80/tcp   hellodockertools_hellodockertools_1
```

### <a name="publishing-docker-images"></a>Docker イメージの発行

アプリケーションの開発とデバッグのサイクルが完了したら、Visual Studio Tools for Docker でアプリケーションの実稼働イメージを作成できます。 デバッグ ドロップダウンを **[リリース]** に変更し、アプリケーションを構築します。 このツールにより、イメージが、プライベート レジストリまたは Docker Hub にプッシュできる `:latest` タグ付きで生成されます。 

`docker images` を使用すると、イメージの一覧を確認できます。

```console
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
spboyer/hellodockertools   latest              8184ae38ba91        5 seconds ago       278.4 MB
spboyer/hellodockertools   dev                 0b6e2e44b3df        About an hour ago   268.9 MB
microsoft/aspnetcore       1.0.1               189ad4312ce7        5 days ago          268.9 MB
```

**dev** イメージと比較した場合、実稼働またはリリース イメージはサイズが小さいと思うかもしれません。しかし、ボリューム マッピングを使用することにより、デバッガーとアプリケーションは実際はコンテナーではなくローカル マシンから実行されます。 **latest** イメージには、ホスト コンピューターでアプリケーションを実行するために必要なアプリケーションのコード全体がパッケージ化されているため、デルタはアプリケーション コードのサイズです。
