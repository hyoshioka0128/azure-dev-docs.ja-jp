---
title: GitHub と Azure DevOps を使用してクラウド内でモバイル アプリケーションのソース コードをホストする
description: Microsoft サービスを使用してクラウド内でモバイル アプリケーション コードをホストするためのサービスについて説明します。
author: codemillmatt
ms.assetid: 12a8a079-9b3c-4faf-2222-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 7a97aa687c80b4b49a566df2f5552bf919a0ec9e
ms.sourcegitcommit: 3d906f265b748fbc0a070fce252098675674c8d9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2021
ms.locfileid: "98699750"
---
# <a name="cloud-hosted-source-code-management-services"></a>クラウドでホストされるソース コードの管理サービス

貴社の開発チームの複数のチーム メンバーが同じコード ベースで作業する場合、コードをホストする適切な場所を見つける必要があります。 クラウドにデータを格納し、一元化されたリポジトリを用意することにより、すべてのユーザーがコード ファイルのアップロード、編集、および管理を行うことができます。 また、プロジェクトの他の開発者とやりとりすることもできます。 インターネットに接続しているかぎり、どこにいてもコードに簡単にアクセスできます。

クラウド ホスティングは、オンプレミスのオプションに比べてはるかに簡単です。 その理由は、ハードウェアの構成が少なくて済み、組織がより俊敏に実装プロセスを完了できるようになることにあります。

## <a name="benefits-of-hosting-source-code-in-the-cloud"></a>クラウドでソース コードをホストする利点

- **一元化されたクラウド ストレージ:** ご利用のデータをどこからでも表示および管理できます。
- **コラボレーションの改善とよりクリーンなコード:** チーム内でコードを追跡してプロジェクトを管理することで、優れたソフトウェアを継続的に配信できます。
- **参加がより簡単に:** プロジェクトに簡単に貢献できます。
- **リリース サイクルの短期化:** チームの作業スピードを上げて、プロジェクトに容易に貢献できます。
- **コストの削減:** 独自のハードウェア、サーバー、VPN などのメンテナンスを心配する必要がありません。

ご利用のアプリケーション データをクラウド内でホストするには、次のサービスを使用します。

## <a name="github"></a>GitHub

[GitHub](https://github.com/) は、さまざまなプログラミング言語でソース コードプロジェクトをホストするオープン ソースのリポジトリ ホスティング サービスです。 GitHub は、すべての反復に加えられたさまざまな変更を追跡します。

### <a name="github-key-features"></a>GitHub の主な機能

- コード ホスティングを使用してすべてのコードを 1 か所に保持します。 プライベート リポジトリ、パブリック リポジトリ、およびオープン ソース リポジトリのすべてに、コードのホスティング、バージョン管理、およびリリースに役立つツールが用意されています。
- コードをレビューし、組み込みのレビュー ツールを使用することで、コード レビューがチームのプロセスの重要な部分になります。
  - ブランチを保護し、変更を提案し、レビューを要求します。
  - 相違点を見つけ、コンテキストにコメントし、明確なフィードバックを受け取ります。
- GitHub のプロジェクト管理ツールを使用して、早期に調整し、連携を保ち、より多くのことを成し遂げます。
  - プロジェクトの全体像を確認できます。
  - GitHub 内のコードのすぐ横にあるタスク ボードを使用できます。
  - カードをドラッグして、チーム メンバーに課題または pull request を割り当てることができます。
  - 進行状況を整理および追跡するマイルストーンを設定できます。
  - 役立つ可能性があるものの、特定の問題や pull request に属さないアイデアをキャプチャするためのメモを作成できます。
- [GitHub Marketplace](https://github.com/marketplace) からアプリケーションを購入することにより、コミュニケーションの向上と作業の自動化に適切なツールを簡単に見つけて選択できます。
- 以下を使用してチームを管理および成長させることができます。 
  - チームとアクセス許可の整理に役立つユーザー ロール。
  - トピックとチームに焦点を当てた会話を維持するディスカッション スレッド ツール。
  - アカウントを使用して新しいチームメンバーをすばやく設定するためのコミュニティ ガイドライン。
- 人気のあるプロジェクトを参照およびスター付けして、フォローします。
- ネットワークに接続して、業界内の他のユーザーから学ぶことができます。

### <a name="github-references"></a>GitHub の参考資料

- [GitHub](https://github.com/)
- [GitHub ガイド](https://guides.github.com/)
- [GitHub コミュニティ フォーラム](https://github.community/)
- [GitHub Marketplace](https://github.com/marketplace)

## <a name="azure-devops"></a>Azure DevOps

[Azure DevOps](https://azure.microsoft.com/services/devops/) では、ソース管理オプションとして [Azure Repos](https://azure.microsoft.com/services/devops/repos/) と [Team Foundation バージョン管理 (TFVC)](/azure/devops/repos/tfvc/index) がサポートされています。 用意されている無制限の無料プライベート リポジトリでは、コラボレーション コード レビュー、高度なファイル管理、コード検索、ブランチ ポリシーを使用して、高品質のコードを確実に実現できます。 Azure Repos は、小規模なプロジェクトにも、ネイティブの Azure Active Directory サポートと高度なポリシーを必要とする大規模な組織にも適しています。

### <a name="azure-devops-features"></a>Azure DevOps 機能

- パブリック リポジトリおよびプライベート リポジトリとして、クラウドでホストされている無制限の Git ソース コード リポジトリを使用します。
  - 任意の Git クライアントのサポートを受けられます。
  - Web hook と API の統合を使用できます。
- リポジトリの pull request から次のビルドを開始できます。
  - 協力してより良いコードをビルドするために、変更ごとにスレッドのディスカッションと継続的インテグレーションを使用します。
  - 継続的インテグレーション/継続的デリバリー (CI/CD) を設定して、完成したすべての pull request でビルド、テスト、デプロイを自動的にトリガーします。 Azure Pipelines やご利用のツールを使用できます。
  - ブランチ ポリシーを使用してご利用のコードの品質を保護します。
- コード レビューを含む、Team Foundation バージョン管理を使用した一元的なバージョン管理を維持します。
- Xcode、Eclipse、IntelliJ、Android Studio、Visual Studio、Visual Studio Code などを使用してコードに接続します。
- 強力なセマンティック コード検索を使用します。
- エンタープライズ対応の機能からメリットを得られます。 Azure DevOps は Azure Active Directory とネイティブ統合されているため、ご利用のコード リポジトリへのアクセスを管理するプロセスが簡素化されます。
- 最小限のコード レビュー、正常なビルドの要件、Git マージ戦略の適用などのブランチ ポリシーを使用してコードの品質を確保します。
- お気に入りの開発環境に接続して、リポジトリへのアクセスおよび作業管理を行います。

### <a name="azure-devops-references"></a>Azure DevOps の参考資料

- [Azure Repos を使ってみる](https://azure.microsoft.com/services/devops/repos/) 
- [Azure Repos のドキュメント](/azure/devops/repos)