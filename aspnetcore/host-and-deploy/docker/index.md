---
title: Docker コンテナーで ASP.NET Core をホストする
author: rick-anderson
description: Docker コンテナーで ASP.NET Core アプリをホストする方法についてのリソースへのリンクを検出します。
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: e56f90ec7272ce0411651ee6f8e7c754ae44b78d
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516262"
---
# <a name="host-aspnet-core-in-docker-containers"></a>Docker コンテナーで ASP.NET Core をホストする

以下の記事では、Docker での ASP.NET Core アプリのホスティングについて説明されています。

[コンテナーと Docker の概要](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
コンテナリゼーションは、ソフトウェア開発のアプローチであり、アプリケーションまたはサービス、その依存関係、その構成がコンテナー イメージとしてどのようにパッケージ化されるかを示します。 イメージは、テストしてからホストに展開することができます。

[Docker について](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
オープン ソース プロジェクトの Docker が、クラウドまたはオンプレミスで実行できる移植可能な自己完結型のコンテナーとしてアプリの展開を自動化するしくみを説明します。

[Docker に関する用語](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
Docker 技術の用語と定義について学習します。

[Docker のコンテナー、イメージ、およびレジストリ](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
環境全体での一貫性のある展開のため、Docker コンテナー イメージがイメージ レジストリに保存されるしくみを確認します。

[Visual Studio Tools for Docker](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
Visual Studio 2017 が、Docker for Windows で .NET Framework または .NET Core をターゲットとする ASP.NET Core アプリのビルド、デバッグ、実行をどのようにサポートするかについて説明します。 Windows と Linux の両方のコンテナーがサポートされます。

[Docker イメージへの公開](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
Visual Studio Tools for Docker 拡張機能を使用して、PowerShell を使用する Azure で Docker ホストに ASP.NET Core アプリを展開する方法を確認します。

[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)  
プロキシ サーバーとロード バランサーの背後でホストされているアプリでは、追加の構成が必要になる場合があります。 プロキシ経由で要求を渡すと、スキームやクライアントの IP アドレスなど、元の要求に関する情報が不明になることがよくあります。 その場合、要求に関する情報をアプリに手動で転送しなければならないことがあります。
