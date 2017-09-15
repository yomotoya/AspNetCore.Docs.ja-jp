---
title: "ASP.NET Core の概要"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: index
ms.openlocfilehash: 10831719630bc638ce2f7424f53548518868d433
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-aspnet-core"></a>ASP.NET Core の概要

著者: [Daniel Roth](https://github.com/danroth27)、[Rick Anderson](https://twitter.com/RickAndMSFT)、[Shaun Luttin](https://twitter.com/dicshaunary)

ASP.NET Core は、インターネットに接続された最新のクラウド ベース アプリケーションを構築するための、クロス プラットフォームで高パフォーマンスの[オープン ソース](https://github.com/aspnet/home) フレームワークです。 ASP.NET Core では次のことができます。

* Web アプリ、Web サービス、IoT アプリ、モバイル バックエンドを構築する。
* Windows、macOS、Linux で好みの開発ツールを使う。
* クラウドまたはオンプレミスに展開する。
* [.NET Core または .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) 上で実行する。

## <a name="why-use-aspnet-core"></a>ASP.NET Core を使う理由

何百万人もの開発者が、これまで、そして現在も、Web アプリの作成に ASP.NET を使っています。 ASP.NET Core は ASP.NET を設計し直したものであり、無駄のないモジュール形式のフレームワークになるようにアーキテクチャが変更されています。

ASP.NET Core の利点は次のとおりです。

* Web UI と Web API を構築するプロセスの統一。
* [最新のクライアント側フレームワーク](xref:client-side/index)と開発ワークフローの統合。
* クラウド対応で環境ベースの[構成システム](xref:fundamentals/configuration)。
* 組み込まれている[依存性の注入](xref:fundamentals/dependency-injection)。
* 軽量で高パフォーマンスのモジュール化された HTTP 要求パイプライン。
* IIS でホストする、または独自のプロセスで自己ホストする機能。
* [.NET Core](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server) で実行可能であり、アプリの真のサイド バイ サイド バージョン管理をサポート。
* 最新の Web 開発を簡単にするツール。
* Windows、macOS、Linux でビルドおよび実行する機能。
* オープン ソースでコミュニティ重視。

ASP.NET Core は、[NuGet](https://nuget.org) パッケージとして完全に提供されます。 これにより、必要な NuGet パッケージだけを含むようにアプリを最適化できます。 小さいアプリ領域の利点には、セキュリティの強化、サービスの削減、パフォーマンスの向上などがあります。

## <a name="build-web-apis-and-web-ui-using-aspnet-core-mvc"></a>ASP.NET Core MVC を使って Web API と Web UI を構築する

ASP.NET Core MVC は、[Web API](xref:tutorials/index#building-web-apis) と [Web アプリ](xref:tutorials/index#building-web-applications)の構築を支援する機能を備えています。

* [モデル ビュー コントローラー (MVC) パターン](xref:mvc/overview)は、Web API と Web アプリを[テスト可能](testing/index.md)にするのに役立ちます。
* [Razor ページ](xref:mvc/razor-pages/index) (2.0 の新機能) はページ ベースのプログラミング モデルであり、Web UI の開発を容易にし、生産性を高めます。
* [Razor 構文](xref:mvc/views/razor)では、[Razor ページ](xref:mvc/razor-pages/index)および [MVC ビュー](xref:mvc/views/overview)用に生産性の高い言語が提供されます。
* [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)を使うと、Razor ファイルでの HTML 要素の作成とレンダリングに、サーバー側コードを組み込むことができます。
* [複数のデータ形式とコンテンツ ネゴシエーション](mvc/models/formatting.md)の組み込みサポートにより、Web API はブラウザーやモバイル デバイスなどのさまざまなクライアントと接続できます。
* [モデル バインド](xref:mvc/models/model-binding)は、HTTP 要求からアクション メソッドのパラメーターにデータを自動的にマップします。
* [モデル検証](xref:mvc/models/validation)は、クライアント側とサーバー側の検証を自動的に実行します。

## <a name="client-side-development"></a>クライアント側の開発

ASP.NET Core は、[AngularJS](xref:client-side/angular)、[KnockoutJS](xref:client-side/knockout)、[Bootstrap](xref:client-side/bootstrap) など、さまざまなクライアント側フレームワークとシームレスに統合するように設計されています。 詳しくは、「[クライアント側の開発](client-side/index.md)」をご覧ください。

## <a name="next-steps"></a>次のステップ

詳細については、次のリソースを参照してください。

* [ASP.NET Core チュートリアル](xref:tutorials/index)
* [ASP.NET Core の基礎](xref:fundamentals/index)
* [週 1 回の ASP.NET Community Standup](https://live.asp.net/) では、チームの進行状況とプランが報告され、新しいブログやサード パーティ製ソフトウェアが採り上げられています。
