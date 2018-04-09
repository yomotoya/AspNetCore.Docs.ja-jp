---
title: ASP.NET Core の概要
author: rick-anderson
description: インターネットに接続された最新のクラウド ベース アプリケーションを構築するための、クロス プラットフォームで高パフォーマンスのオープン ソース フレームワークである ASP.NET Core について説明します。
manager: wpickett
ms.author: riande
ms.date: 02/28/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: index
ms.openlocfilehash: 63ea2aaf7b502ee08fc2f5268d17ed459adaee73
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2018
---
# <a name="introduction-to-aspnet-core"></a>ASP.NET Core の概要

著者: [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core は、インターネットに接続された最新のクラウド ベース アプリケーションを構築するための、クロス プラットフォームで高パフォーマンスの[オープン ソース](https://github.com/aspnet/home) フレームワークです。 ASP.NET Core では次のことができます。

* Web アプリ、Web サービス、[IoT](https://www.microsoft.com/internet-of-things/) アプリ、モバイル バックエンドを構築する。
* Windows、macOS、Linux で好みの開発ツールを使う。
* クラウドまたはオンプレミスに展開する。
* [.NET Core または .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 上で実行する。

## <a name="why-use-aspnet-core"></a>ASP.NET Core を使う理由

何百万人もの開発者が、これまで、そして現在も、Web アプリの作成に [ASP.NET 4.x](https://docs.microsoft.com/aspnet/overview) を使っています。 ASP.NET Core は ASP.NET 4.x を設計し直したものであり、無駄のないモジュール形式のフレームワークになるようにアーキテクチャが変更されています。

ASP.NET Core の利点は次のとおりです。

* Web UI と Web API を構築するプロセスの統一。
* [最新のクライアント側フレームワーク](xref:client-side/index)と開発ワークフローの統合。
* クラウド対応で環境ベースの[構成システム](xref:fundamentals/configuration/index)。
* 組み込まれている[依存性の注入](xref:fundamentals/dependency-injection)。
* 軽量で[高パフォーマンス](https://github.com/aspnet/benchmarks)のモジュール化された HTTP 要求パイプライン。
* [IIS](xref:host-and-deploy/iis/index)、[Nginx](xref:host-and-deploy/linux-nginx)、[Apache](xref:host-and-deploy/linux-apache)、[Docker](xref:host-and-deploy/docker/index) でホストする、または独自のプロセスで自己ホストする機能。
* [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) に対応する場合は、アプリのサイド バイ サイド バージョン管理をサポート。
* 最新の Web 開発を簡単にするツール。
* Windows、macOS、Linux でビルドおよび実行する機能。
* オープン ソースで[コミュニティ重視](https://live.asp.net/)。

ASP.NET Core は、[NuGet](https://www.nuget.org/) パッケージとして完全に提供されます。 NuGet パッケージを使用することにより、必要な依存関係だけを含むようにアプリを最適化できます。 実際に、.NET Core に対応した ASP.NET Core 2.x アプリで必要なのは、[1 つの NuGet パッケージ](xref:fundamentals/metapackage)だけです。 小さいアプリ領域の利点には、セキュリティの強化、サービスの削減、パフォーマンスの向上などがあります。

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>ASP.NET Core MVC を使って Web API と Web UI を構築する

ASP.NET Core MVC は、[Web API](xref:tutorials/index#build-web-apis) と [Web アプリ](xref:tutorials/index#build-web-apps)を構築する機能を備えています。

* [モデル ビュー コントローラー (MVC) パターン](xref:mvc/overview)は、Web API と Web アプリを[テスト可能](testing/index.md)にするのに役立ちます。
* [Razor ページ](xref:mvc/razor-pages/index) (ASP.NET Core 2.0 の新機能) はページ ベースのプログラミング モデルであり、Web UI の開発を容易にし、生産性を高めます。
* [Razor マークアップ](xref:mvc/views/razor)では、[Razor ページ](xref:mvc/razor-pages/index)および [MVC ビュー](xref:mvc/views/overview)用に生産性の高い構文が提供されます。
* [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)を使うと、Razor ファイルでの HTML 要素の作成とレンダリングに、サーバー側コードを組み込むことができます。
* [複数のデータ形式とコンテンツ ネゴシエーション](xref:web-api/advanced/formatting)の組み込みサポートにより、Web API はブラウザーやモバイル デバイスなどのさまざまなクライアントと接続できます。
* [モデル バインド](xref:mvc/models/model-binding)は、HTTP 要求からアクション メソッドのパラメーターにデータを自動的にマップします。
* [モデル検証](xref:mvc/models/validation)は、クライアント側とサーバー側の検証を自動的に実行します。

## <a name="client-side-development"></a>クライアント側の開発

ASP.NET Core は、人気のあるクライアント側のフレームワークとライブラリ ([Angular](xref:spa/angular)、[React](xref:spa/react)、[Bootstrap](xref:client-side/bootstrap) など) をシームレスに統合します。 詳しくは、「[クライアント側の開発](xref:client-side/index)」をご覧ください。

## <a name="aspnet-core-targeting-net-framework"></a>.NET Framework を対象とする ASP.NET Core

ASP.NET Core は、.NET Core または .NET Framework を対象にすることができます。 .NET Framework を対象とする ASP.NET Core アプリはクロスプラットフォームではありません&mdash;Windows でのみ実行されます。 ASP.NET Core の .NET Framework を対象とするためのサポートを削除するプランはありません。 一般に、ASP.NET Core は [.NET Standard](/dotnet/standard/net-standard) ライブラリで構成されています。 .NET Standard 2.0 で記述されたアプリは、.NET Standard 2.0 がサポートされていればどこでも実行できます。

.NET Core を対象とする利点はいくつかあり、リリースのたびにその利点が増えています。 .NET Framework 経由による .NET Core には次のような利点があります。

* クロスプラットフォームである。 macOS、Linux、Windows で実行できる。
* パフォーマンスの向上
* side-by-side でのバージョン管理
* 新しい API
* ソースを開く

.NET Framework と .NET Core の間にある API のギャップを埋めるため、鋭意作業中です。 [Windows 互換機能パック](/dotnet/core/porting/windows-compat-pack)により、多くの Windows 限定の API が .NET Core で利用できるようになりました。 このような API は .NET Core 1.x で利用できませんでした。

## <a name="next-steps"></a>次の手順

詳細については、次のリソースを参照してください。

* [Razor ページの概要](xref:tutorials/razor-pages/razor-pages-start)
* [ASP.NET Core チュートリアル](xref:tutorials/index)
* [ASP.NET Core の基礎](xref:fundamentals/index)
* [週 1 回の ASP.NET Community Standup](https://live.asp.net/) では、チームの進行状況とプランが報告され、 新しいブログやサード パーティ製ソフトウェアが取り上げられています。
