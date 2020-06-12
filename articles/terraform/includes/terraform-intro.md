---
title: インクルード ファイル
description: インクルード ファイル
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 05/31/2020
ms.author: tarcher
ms.openlocfilehash: 43e684a44c657ab44f3f8f13719e08f0e339f498
ms.sourcegitcommit: db56786f046a3bde1bd9b0169b4f62f0c1970899
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/03/2020
ms.locfileid: "84329900"
---
[Azure 上に Terraform](https://www.terraform.io/docs/providers/azurerm/index.html) を構成し、基本的な Azure リソース グループを作成することにより、[Terraform](https://www.terraform.io) の使用を開始します。

Terraform を使用すると、クラウド インフラストラクチャの定義、プレビュー、およびデプロイを行うことができます。 Terraform を使用する際は、[HCL 構文](https://www.terraform.io/docs/configuration/syntax.html)を使って構成ファイルを作成します。 HCL 構文では、Azure などのクラウド プロバイダーと、クラウド インフラストラクチャを構成する要素を指定できます。 構成ファイルを作成したら、"*実行プラン*" を作成します。これにより、インフラストラクチャの変更をデプロイ前にプレビューすることができます。 変更を確認したら、実行プランを適用してインフラストラクチャをデプロイします。
