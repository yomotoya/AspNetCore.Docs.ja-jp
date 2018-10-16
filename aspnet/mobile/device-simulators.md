---
uid: mobile/device-simulators
title: テスト用の一般的なモバイル デバイスのシミュレート |Microsoft Docs
author: rick-anderson
description: 次のリンクで人気のあるモバイル デバイスおよびブラウザー用のエミュレーターをダウンロードします。
ms.author: riande
ms.date: 10/11/2018
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 8299295154d23f8fc430676b2c8ad8efc98ad185
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348378"
---
# <a name="simulate-popular-mobile-devices-for-testing"></a>テスト用の一般的なモバイル デバイスをシミュレートします。

> 一般的なモバイル デバイスおよびブラウザー エミュレーターをダウンロードするには、以下のリンクします。

| デバイスまたはブラウザー | エミュレーターまたはシミュレーター |
| --- | --- |
| BrowserStack がホストされるブラウザーの仮想化 ![BrowserStack がホストされるブラウザーの仮想化](device-simulators/_static/image1.png) | [BrowserStack ホストされているブラウザー Virtualization](http://browserstack.com)任意のプラットフォームで任意のブラウザーで、ローカルまたは実稼働環境をテストします。 ホストされている仮想マシンで、コンピューターと BrowserStack ネットワークの間のトンネルを作成できます。 取得することを確認、 [BrowserStack Visual Studio 拡張機能](https://marketplace.visualstudio.com/items?itemName=browserstackcom.BrowserStack)さらにシームレスにします。 |
| iPhone / iPod / iPad | [電気 Mobile Studio](http://www.electricplum.com/studio.aspx) iPhone および iPad シミュレーターの Windows、だけでなく、レスポンシブ デザイン ツールです。 |
| Android | [Android Studio](https://developer.android.com/studio/)または[Visual Studio Emulator for Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) |
| Opera Mobile | [Opera Mobile クラシック エミュレーター](https://www.opera.com/developer/mobile-emulator) |
| Windows Mobile 6.5.3 | [Windows Mobile 6.5.3 Developer Tool Kit](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en)電話ネットワークへのアクセスを付与することも必要がある場合に含まれる VPC のネットワーク アダプター [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en)します。 IE の接続に、Visual Studio 開発サーバーを電話で、次を参照してください。 [Kiran Patil のブログの投稿](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/)します。 |

> [!NOTE]
> (これは、Windows の場合は true。 エミュレーターがないために、iPhone または iPad を十分にテストの唯一のオプションは) 実際のモバイル デバイスでアプリケーションを表示する場合は、IIS または IIS Express でアプリケーションをホストする必要があります。 他のマシンからの要求に応答しませんので、これは、Visual Studio の組み込みの web サーバーを使用することはできません簡単にします。