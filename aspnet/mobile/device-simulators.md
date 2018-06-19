---
uid: mobile/device-simulators
title: テストするための一般的なモバイル デバイスをシミュレート |Microsoft ドキュメント
author: rick-anderson
description: 以下のリンクで一般的なモバイル デバイスおよびブラウザーのエミュレーターをダウンロードすることができます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2011
ms.topic: article
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: a8293a5bff9ed73f177be2d9928d8d686c4f311d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043812"
---
<a name="simulate-popular-mobile-devices-for-testing"></a>テストするための一般的なモバイル デバイスをシミュレートします。
====================
> 以下のリンクで一般的なモバイル デバイスおよびブラウザーのエミュレーターをダウンロードすることができます。


| デバイスまたはブラウザー | エミュレーターまたはシミュレーター |
| --- | --- |
| BrowserStack には、ブラウザーの仮想化がホストされています。 ![BrowserStack には、ブラウザーの仮想化がホストされています。](device-simulators/_static/image1.png) | [BrowserStack ホストされているブラウザー Virtualization](http://browserstack.com)任意のプラットフォームで任意のブラウザーで、ローカルまたは実稼働環境をテストします。 ホストされる仮想マシンで、コンピューターと BrowserStack ネットワーク間トンネルを作成できます。 取得することを確認、 [BrowserStack Visual Studio Extension](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c)よりシームレスなエクスペリエンスのためです。 |
| Windows Phone | [Windows Phone SDK をダウンロード](https://dev.windowsphone.com/downloadsdk)Windows Phone ソフトウェア開発キット (SDK) には、すべての Windows Phone のアプリやゲームを開発する必要のあるツールが含まれています |
| iPhone / iPod / iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) iPhone および iPad シミュレーター for Windows、だけでなく、Responsive デザイン ツールです。 VS 2012「と参照..」オプションと統合できます。 |
| Android | [Android SDK ホーム ページ](https://developer.android.com/sdk) |
| Opera Mobile/Opera Mini | 最新のバージョン: [Opera Developer Tools ホーム](http://www.opera.com/developer/tools/)Opera Mini 4.2:[オンライン Java ベース シミュレーター](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile 6.5.3 | [Windows Mobile 6.5.3 開発者ツール キット](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en)電話ネットワーク アクセス権を付与するにも必要がある場合に含まれる VPC ネットワーク アダプター [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en)です。 IE を接続する Visual Studio 開発サーバーに電話で、次を参照してください。 [Kiran Patil のブログの投稿](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/)です。 |
| Windows Mobile 6.1 | [Visual Studio 2005/2008 のエミュレーターの画像](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

(Windows の場合は true。 エミュレーターが存在しないために、iPhone または iPad を十分にテストの唯一のオプション) 実際のモバイル デバイスにアプリケーションを表示する場合は、IIS または IIS Express でアプリケーションをホストする必要ありますに注意してください。 簡単に、他のマシンからの要求に応答しませんので、Visual Studio の組み込みの web サーバーをこれは、使用できません。
