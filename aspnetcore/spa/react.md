---
title: "対応プロジェクト テンプレートを使用します。"
author: SteveSandersonMS
description: "ASP.NET Core 単一ページ アプリケーション (SPA) プロジェクト テンプレートを使用して対応と作成対応アプリの作業を開始する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react
ms.openlocfilehash: e093a47159fd8278ff3705bb2c53571a8e27fab8
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-react-project-template"></a>対応プロジェクト テンプレートを使用します。

> [!NOTE]
> このドキュメントに関する対応プロジェクト テンプレートに含まれていない ASP.NET Core 2.0。 手動で更新できる新しい対応テンプレートことです。 既定では ASP.NET Core 2.1 では、テンプレートが含まれます。

更新された対応プロジェクト テンプレートでは便利な開始点 ASP.NET Core の反応を使用してアプリと[作成対応アプリ](https://github.com/facebookincubator/create-react-app)豊富なクライアント側のユーザー インターフェイス (UI) を実装する (CRA) 規則。

テンプレートは、利便性の両方で、ビルドと 1 つの単位として公開された 1 つのアプリ プロジェクトをホストしているのですが、UI として機能する標準的な CRA 対応プロジェクトと、API のバックエンドとして機能する、ASP.NET Core プロジェクトの両方を作成できます。

## <a name="create-a-new-app"></a>新しいアプリを作成します。

ASP.NET Core 2.0 を使用する場合は、いることを確認[更新の対応プロジェクト テンプレートがインストールされている](xref:spa/index#installation)です。 ASP.NET Core 2.1 があれば、それをインストールする必要はありません。

コマンドを使用してコマンド プロンプトから、新しいプロジェクトを作成する`dotnet new react`空のディレクトリにします。 次のコマンドがでアプリを作成するなど、 *my-新しい-アプリ*ディレクトリとそのディレクトリへの切り替え。

```console
dotnet new react -o my-new-app
cd my-new-app
```

Visual Studio または .NET Core CLI からアプリを実行します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

開く、生成された*.csproj*ファイル、およびそこから通常の方法でアプリを実行します。

ビルド プロセスでは、数分かかることが、最初の実行で npm の依存関係を復元します。 後続のビルドは非常に高速です。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

という環境変数があることを確認`ASPNETCORE_Environment`の値を持つ`Development`します。 Windows PowerShell 以外の入力を要求) の「、で実行される`SET ASPNETCORE_Environment=Development`です。 Linux または macOS、実行`export ASPNETCORE_Environment=Development`です。

実行`dotnet build`正しくビルドするのには、アプリを確認してください。 最初の実行には、ビルド プロセスは、数分かかることが npm の依存関係を復元します。 後続のビルドは非常に高速です。

実行`dotnet run`でアプリを起動します。

---

プロジェクト テンプレートは、ASP.NET Core アプリおよび対応アプリを作成します。 ASP.NET Core アプリケーションがデータ アクセス、承認、およびその他のサーバー側の懸念事項に使用するためのものです。 内に存在する、対応アプリ、 *ClientApp*サブディレクトリがすべての UI に関する注意事項を使用するためです。

## <a name="add-pages-images-styles-modules-etc"></a>ページ、画像、スタイル、モジュールなどを追加します。

*ClientApp*ディレクトリは、標準の CRA 対応アプリ。 公式を参照してください[CRA ドキュメント](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md)詳細についてはします。

このテンプレートで作成した対応アプリケーションと CRA 自体によって作成されたものの間のわずかな違いがあります。ただし、アプリの機能は変更されません。 テンプレートによって作成されたアプリが含まれています、[ブートス トラップ](https://getbootstrap.com/)-ベースのレイアウトとルーティングの基本的な例です。

## <a name="install-npm-packages"></a>Npm パッケージをインストールします。

サード パーティ製の npm パッケージをインストールするでコマンド プロンプトを使用して、 *ClientApp*サブディレクトリです。 例:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>発行と配置

開発中のアプリは、開発者の利便性の最適化モードで実行されます。 たとえば、JavaScript のバンドルは、(をデバッグする場合は、元のソース コードを確認できます) をソース マップを含めます。 アプリは JavaScript、HTML、および CSS のディスクのファイルの変更を監視し、自動的にコンパイルされそれらのファイルの変更が発生したときに再読み込みされます。

実稼働環境でパフォーマンスが最適化された、アプリのバージョンを提供します。 これは、自動的に実行される構成されます。 パブリッシュすると、ビルド構成は、クライアント側コードの縮小された、transpiled ビルドを出力します。 開発ビルドとは異なり、運用ビルドは、サーバーにインストールされている Node.js を必要としません。

標準を使用する[ASP.NET Core のホストと展開方法](xref:host-and-deploy/index)です。

## <a name="run-the-cra-server-independently"></a>CRA サーバーを個別に実行します。

プロジェクトが ASP.NET Core アプリケーションの開発モードの開始時に、バック グラウンドで CRA 開発サーバーの独自のインスタンスを開始するように構成します。 これは、機能は、別のサーバーを手動で実行する必要はありませんので便利です。

この既定の設定の欠点があります。 C# コードとアプリを再起動する必要があります、ASP.NET Core を変更するたびに、CRA サーバーを再起動します。 バックアップを開始するには、数秒が必要です。 頻繁に c# コードの編集を加えようとして CRA サーバーを再起動するまで待機しない場合は、ASP.NET Core プロセスとは無関係に外部から、CRA サーバーを実行します。 これを行うには。

1. コマンド プロンプトでに切り替えると、 *ClientApp*サブディレクトリ、および CRA 開発サーバーの起動。

    ```console
    cd ClientApp
    npm start
    ```

2. 独自のいずれかを起動せずに、外部の CRA サーバー インスタンスを使用する ASP.NET Core アプリケーションを変更します。 *スタートアップ*クラス、置換、`spa.UseReactDevelopmentServer`を次の呼び出し。

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:3000");
    ```

ASP.NET Core アプリを起動するときに CRA サーバーの起動時にはありません。 手動で開始したインスタンスが使用されます。 これにより、起動し、再起動を高速にできます。 不要になった、たびに再構築する対応アプリケーションが待機しています。
