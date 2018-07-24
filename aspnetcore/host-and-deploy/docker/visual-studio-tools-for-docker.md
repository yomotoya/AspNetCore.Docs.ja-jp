---
title: Visual Studio Tools for Docker と ASP.NET Core
author: spboyer
description: Visual Studio 2017 ツールと Docker for Windows を使用して、ASP.NET Core アプリをコンテナー化する方法を説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 07/18/2018
uid: host-and-deploy/docker/visual-studio-tools-for-docker
ms.openlocfilehash: afa7b05820ba021c50d9c23804095f7edd8b71f1
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146885"
---
# <a name="visual-studio-tools-for-docker-with-aspnet-core"></a>Visual Studio Tools for Docker と ASP.NET Core

[Visual Studio 2017](https://www.visualstudio.com/) は、.NET Core をターゲットとするコンテナー化された ASP.NET Core アプリのビルド、デバッグ、実行をサポートします。 Windows と Linux の両方のコンテナーがサポートされます。

## <a name="prerequisites"></a>必須コンポーネント

* [Visual Studio 2017](https://www.visualstudio.com/) と **[.NET Core クロスプラットフォームの開発]** ワークロード
* [Docker for Windows](https://docs.docker.com/docker-for-windows/install/)

## <a name="installation-and-setup"></a>インストールとセットアップ

Docker をインストールする場合、「[Docker for Windows: What to know before you install](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install)」 (Docker for Windows: インストール前に知っておくべきこと) の情報を確認し、[Docker for Windows](https://docs.docker.com/docker-for-windows/install/) をインストールします。

ボリュームのマッピングとデバッグをサポートするように、Docker for Windows で**[共有ドライブ](https://docs.docker.com/docker-for-windows/#shared-drives)** を構成する必要があります。 システム トレイの Docker アイコンを右クリックして **[設定]** をクリックし、**[Shared Drives]\(共有ドライブ\)** を選択します。 Docker がファイルを保存するドライブを選択します。 **[適用]** を選択します。

![共有ドライブ](./visual-studio-tools-for-docker/_static/settings-shared-drives-win.png)

> [!TIP]
> Visual Studio 2017 バージョン 15.6 以降では、**共有ドライブ**が構成されていない場合にプロンプトが表示されます。

## <a name="add-docker-support-to-an-app"></a>アプリに Docker サポートを追加する

ASP.NET Core プロジェクトに Docker のサポートを追加するには、プロジェクトで .NET Core をターゲットにする必要があります。 Linux と Windows の両方のコンテナーがサポートされています。

プロジェクトに Docker サポートを追加する場合は、Windows または Linux のいずれかのコンテナーを選択します。 Docker ホストが同じコンテナーの種類を実行している必要があります。 実行中の Docker インスタンスでコンテナーの種類を変更するには、システム トレイの Docker アイコンを右クリックして、**[Switch to Windows containers...]\(Windows コンテナーに切り替える...\)** または **[Switch to Linux containers...]\(Linux コンテナーに切り替える...\)** を選択します。

### <a name="new-app"></a>新しいアプリ

**ASP.NET Core Web アプリケーション** プロジェクト テンプレートを使用して新しいアプリを作成する場合は、次のように **[Enable Docker Support]\(Docker サポートを有効にする\)** チェック ボックスをオンにします。

![[Enable Docker Support]\(Docker サポートを有効にする\) チェック ボックス](visual-studio-tools-for-docker/_static/enable-docker-support-check box.png)

ターゲット フレームワークが .NET Core の場合、**[OS]** ドロップダウンでコンテナーの種類を選択できます。

### <a name="existing-app"></a>既存のアプリ

Visual Studio Tools for Docker では、.NET Framework をターゲットとする既存の ASP.NET Core プロジェクトへの Docker の追加はサポートされません。 .NET Core をターゲットとする ASP.NET Core プロジェクトの場合、ツールを使用して Docker サポートを追加するための 2 つのオプションがあります。 Visual Studio でプロジェクトを開き、次のオプションのいずれかを選択します。

* **[プロジェクト]** メニューから **[Docker サポート]** を選択します。
* ソリューション エクスプローラーでプロジェクトを右クリックして、**[追加]** > **[Docker サポート]** の順に選択します。

## <a name="docker-assets-overview"></a>Docker 資産の概要

Visual Studio Tools for Docker は、ソリューションに *docker-compose* プロジェクトを追加します。以下のファイルが含まれます。

* *.dockerignore*: ビルド コンテキストを生成するときに除外するファイルとディレクトリのパターンの一覧が含まれています。
* *docker-compose.yml*: `docker-compose build` と `docker-compose run` を使用してビルドされ実行されるイメージのコレクションを定義するために使用されるベースとなる [Docker Compose](https://docs.docker.com/compose/overview/) ファイル。
* *docker-compose.override.yml*: サービスの構成オーバーライドを含む、Docker Compose によって読み取られるオプション ファイル。 Visual Studio は `docker-compose -f "docker-compose.yml" -f "docker-compose.override.yml"` を実行してこれらのファイルをマージします。

*Dockerfile* (Docker の最終イメージを作成するためのレシピ) は、プロジェクトのルートに追加されます。 その中に含まれるコマンドの詳細については、「[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)」 (Dockerfile リファレンス) を参照してください。 この特定の *Dockerfile* では、次のような、4 つの異なる名前付きのビルド ステージを含む、[multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) を使用します。

[!code-dockerfile[](visual-studio-tools-for-docker/samples/HelloDockerTools/HelloDockerTools/Dockerfile?highlight=1,5,14,17)]

*Dockerfile* は、[microsoft/aspnetcore](https://hub.docker.com/r/microsoft/aspnetcore) イメージに基づいています。 このベース イメージには、Just-In-Time (JIT) にコンパイルされて起動時のパフォーマンスが向上した、ASP.NET Core NuGet パッケージが含まれます。

*docker-compose.yml* ファイルには、プロジェクトの実行時に作成されたイメージの名前が含まれています。

[!code-yaml[](visual-studio-tools-for-docker/samples/HelloDockerTools/docker-compose.yml?highlight=5)]

上の例では、`image: hellodockertools` によって、アプリが**デバッグ** モードで実行されるときに、イメージ `hellodockertools:dev` が生成されます。 `hellodockertools:latest` イメージは、アプリが**リリース** モードで実行されるときに生成されます。

イメージがレジストリにプッシュされる場合は、イメージ名の前に [Docker Hub](https://hub.docker.com/) のユーザー名を付けます (例: `dockerhubusername/hellodockertools`)。 または、構成に応じて、プライベート レジストリ URL を含めるようにイメージ名を変更します (例: `privateregistry.domain.com/hellodockertools`)。

## <a name="debug"></a>デバッグ

ツールバーのデバッグ ドロップダウンから **[Docker]** を選択し、アプリのデバッグを開始します。 **[出力]** ウィンドウの **[Docker]** ビューには、行われている次のアクションが表示されます。

* *microsoft/aspnetcore* ランタイム イメージが取得されます (まだキャッシュにない場合)。
* *microsoft/aspnetcore-build* コンパイル/発行イメージが取得されます (キャッシュにない場合)。
* *ASPNETCORE_ENVIRONMENT* 環境変数は、コンテナー内で `Development` に設定されます。
* ポート 80 が公開され、localhost 用に動的に割り当てられているポートにマップされます。 このポートは、Docker ホストによって決定され、`docker ps` コマンドでクエリを実行することができます。
* アプリがコンテナーにコピーされます。
* 動的に割り当てられたポートを使用して、デバッガーがコンテナーにアタッチされ、既定のブラウザーが起動します。

結果の Docker イメージは *microsoft/aspnetcore* イメージをベース イメージとする、アプリの *dev* イメージです。 **パッケージ マネージャー コンソール** (PMC) ウィンドウで `docker images` コマンドを実行します。 コンピューター上のイメージが表示されます。

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                f8f9d6c923e2        About an hour ago   391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        15 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> **デバッグ**構成では、反復にボリューム マウントを使用するため、dev イメージにはアプリのコンテンツはありません。 イメージをプッシュするには、**リリース**構成を使用します。

PMC で `docker ps` コマンドを実行します。 アプリがコンテナーを使用して実行されていることがわかります。

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   21 seconds ago      Up 19 seconds       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="edit-and-continue"></a>エディット コンティニュ

静的なファイルや Razor ビューに対する変更は、コンパイルを行う必要はなく、自動的に更新されます。 変更を行って保存し、ブラウザーを更新し、更新を確認します。

コード ファイルを変更する場合、コンテナー内で Kestrel をコンパイルして再起動する必要があります。 変更したら、`CTRL+F5` キーを使用して、コンテナー内でプロセスを実行し、アプリを起動します。 Docker コンテナーが再構築されたり、停止されたりすることはありません。 PMC で `docker ps` コマンドを実行します。 元のコンテナーが 10 分前と同じ状態で実行されていることを確認できます。

```console
CONTAINER ID        IMAGE                  COMMAND                   CREATED             STATUS              PORTS                   NAMES
baf9a678c88d        hellodockertools:dev   "C:\\remote_debugge..."   10 minutes ago      Up 10 minutes       0.0.0.0:37630->80/tcp   dockercompose4642749010770307127_hellodockertools_1
```

## <a name="publish-docker-images"></a>Docker イメージの発行

アプリの開発とデバッグのサイクルが完了したら、Visual Studio Tools for Docker でアプリの実稼働イメージを作成できます。 構成ドロップダウンを **[リリース]** に変更し、アプリを構築します。 このツールにより、イメージが、プライベート レジストリまたは Docker Hub にプッシュできる *latest* タグ付きで生成されます。

PMC で `docker images` コマンドを実行して、次のようにイメージの一覧を表示します。

```console
REPOSITORY                   TAG                   IMAGE ID            CREATED             SIZE
hellodockertools             latest                4cb1fca533f0        19 seconds ago      391MB
hellodockertools             dev                   85c5ffee5258        About an hour ago   389MB
microsoft/aspnetcore-build   2.0-nanoserver-1709   d7cce94e3eb0        16 hours ago        1.86GB
microsoft/aspnetcore         2.0-nanoserver-1709   8872347d7e5d        40 hours ago        389MB
```

> [!NOTE]
> `docker images` コマンドは、*\<none>* として識別されるリポジトリ名とタグを持つ中間イメージを返します (上にはリストされていません)。 これらの名前のないイメージは、[multi-stage build](https://docs.docker.com/engine/userguide/eng-image/multistage-build/) *Dockerfile* によって生成されます。 これにより、最終イメージの構築効率が向上します&mdash;変更時には必要なレイヤーのみが再構築されます。 中間イメージが不要になった場合は、[docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) コマンドを使用して削除します。

*dev* イメージと比較した場合、実稼働またはリリース イメージはサイズが小さいと思うかもしれません。 ボリューム マッピングにより、デバッガーとアプリは、コンテナー内ではなく、ローカル コンピューターから実行されています。 *latest* イメージには、ホスト コンピューターでアプリを実行するために必要なアプリ コードがパッケージ化されています。 そのため、デルタはアプリ コードのサイズです。

## <a name="additional-resources"></a>その他の技術情報

* [Docker を使用した Visual Studio 2017 開発のトラブルシューティング](/azure/vs-azure-tools-docker-troubleshooting-docker-errors)
* [Visual Studio Tools for Docker の GitHub リポジトリ](https://github.com/Microsoft/DockerTools)
