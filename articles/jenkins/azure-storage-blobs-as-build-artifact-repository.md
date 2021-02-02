---
title: チュートリアル - ビルド成果物に Azure Storage を使用する
description: Jenkins 継続的インテグレーション ソリューションで作成されるビルド アーティファクトのリポジトリとして Azure BLOB サービスを使用する方法について説明します。
keywords: jenkins, azure, devops, ストレージ, cicd, ビルド成果物
ms.topic: article
ms.date: 01/12/2021
ms.custom: devx-track-jenkins, devx-track-azurecli
ms.openlocfilehash: 0f7f9bd8ca7997064745a5be431e17f4e3d3fc13
ms.sourcegitcommit: 6fbf9e489b194586887a2c11152044be5b3a2b99
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2021
ms.locfileid: "98759526"
---
# <a name="tutorial-use-azure-storage-for-build-artifacts"></a>チュートリアル:ビルド成果物に Azure Storage を使用する

[!INCLUDE [jenkins-integration-with-azure.md](includes/jenkins-integration-with-azure.md)]

この記事では、Jenkins 継続的インテグレーション (CI) ソリューションで作成されるビルド アーティファクトのリポジトリとして、またはビルド プロセスで使用されるダウンロード可能なファイルのソースとして BLOB ストレージを使用する方法について説明します。 このソリューションが有用になるシナリオの 1 つは、アジャイル開発環境で (Java などの言語を使って) コーディングをしており、継続的インテグレーションに基づいてビルドを実行するとき、ビルド アーティファクト用のリポジトリが必要な場合です。このリポジトリがあれば、ビルド アーティファクトを他の組織のメンバーや顧客と共有したり、そのアーカイブを保存したりできます。 もう 1 つのシナリオとしては、ビルド ジョブ自体にその他のファイルが必要になる場合、たとえば、ビルドの入力で依存関係のダウンロードが必要になる場合などが考えられます。

## <a name="prerequisites"></a>前提条件

- **Azure サブスクリプション**:Azure サブスクリプションをお持ちでない場合は、開始する前に [無料の Azure アカウント](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)を作成してください。
- **Jenkins サーバー**: Jenkins サーバーがインストールされていない場合は、[Azure に Jenkins サーバーを作成します](./configure-on-linux-vm.md)。
- **Azure CLI**:Azure CLI (バージョン 2.0.67 以上) を Jenkins サーバーにインストールします。
- **Azure ストレージ アカウント**:まだストレージ アカウントがない場合は、[ストレージ アカウントを作成します](/azure/storage/common/storage-account-create)。

## <a name="add-azure-credential-needed-to-execute-azure-cli"></a>Azure CLI を実行するために必要な Azure の資格情報を追加する

1. Jenkins ポータルに移動します。

1. メニューから、 **[Manage Jenkins]\(Jenkins の管理\)** を選択します。

1. **[資格情報の管理]** を選択します。

1. **グローバル** ドメインを選択します。

1. **[Add Credentials]\(資格情報の追加\)** を選択します。

1. 必須フィールドに以下のとおり入力します。

    - **種類**: **[Username with password]\(ユーザー名とパスワード\)** を選択します。
    - **[ユーザー名]** : サービス プリンシパルの `appId` を指定します。
    - **パスワード**:サービス プリンシパルの`password` を指定します。
    - **[ID]** :`azuresp` などの、資格情報の識別子を指定します。
    - **[説明]** :必要に応じて、環境を表すわかりやすい説明を含めます。

1. **[OK]** を選択して資格情報を作成します。

## <a name="create-a-pipeline-job-to-upload-build-artifacts"></a>パイプライン ジョブを作成してビルド成果物をアップロードする

次の手順では、パイプライン ジョブの作成について順番に説明します。 パイプライン ジョブでは、Azure CLI を使用して複数のファイルを作成し、それらのファイルをお使いのストレージ アカウントにアップロードします。

1. Jenkins ダッシュボードから **[New Item]\(新しい項目\)** を選択します。

1. ジョブに **myjob** という名前を指定し、 **[Pipeline]\(パイプライン\)** を選択してから **[OK]** を選択します。

1. ジョブ構成の **[Pipeline]\(パイプライン\)** セクションで、 **[Pipeline script]\(パイプライン スクリプト\)** を選択し、以下を **[スクリプト]** に貼り付けます。 プレースホルダーは、お使いの環境に合った適切な値になるように編集してください。

    ```groovy
    pipeline {
      agent any
      environment {
        AZURE_SUBSCRIPTION_ID='99999999-9999-9999-9999-999999999999'
        AZURE_TENANT_ID='99999999-9999-9999-9999-999999999999'
        AZURE_STORAGE_ACCOUNT='myStorageAccount'
      }
      stages {
        stage('Build') {
          steps {
            sh 'rm -rf *'
            sh 'mkdir text'
            sh 'echo Hello Azure Storage from Jenkins > ./text/hello.txt'
            sh 'date > ./text/date.txt'
          }
    
          post {
            success {
              withCredentials([usernamePassword(credentialsId: 'azuresp', 
                              passwordVariable: 'AZURE_CLIENT_SECRET', 
                              usernameVariable: 'AZURE_CLIENT_ID')]) {
                sh '''
                  echo $container_name
                  # Login to Azure with ServicePrincipal
                  az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
                  # Set default subscription
                  az account set --subscription $AZURE_SUBSCRIPTION_ID
                  # Execute upload to Azure
                  az storage container create --account-name $AZURE_STORAGE_ACCOUNT --name $JOB_NAME --auth-mode login
                  az storage blob upload-batch --destination ${JOB_NAME} --source ./text --auth-mode login
                  # Logout from Azure
                  az logout
                '''
              }
            }
          }
        }
      }
    }
    ```
    
1. **[Build Now]\(今すぐビルド\)** を選択し、**myjob** を実行します。

1. コンソール出力でステータスを確認します。 ビルド後のアクションによってビルド成果物がアップロードされると、Azure Storage のステータス メッセージがコンソールに書き込まれます。

1. 次のようなエラーが発生した場合は、コンテナー レベルでアクセス権を付与する必要があることを意味します。`ValidationError: You do not have the required permissions needed to perform this operation.` このエラー メッセージが表示された場合は、次の記事を参照して解決してください。

    - [Azure CLI で BLOB データへのアクセスの承認方法を選択する - Azure Storage](/azure/storage/blobs/authorize-data-operations-cli)
    - [Azure portal を使用してデータへのアクセスのための Azure ロールを割り当てる - Azure Storage](/azure/storage/common/storage-auth-aad-rbac-portal)

1. ジョブが正常に完了したら、パブリック BLOB を開いてビルド成果物を確認します。

    1. [Azure portal](https://portal.azure.com) にサインインします。
    1. **[ストレージ]** を選択します。
    1. Jenkins に使用したストレージ アカウント名を選択します。
    1. **[コンテナー]** を選択します。
    1. BLOB の一覧で、**myjob** という名前のコンテナーを選択します。
    1. **hello.txt** と **date.txt** という 2 つのファイルが表示されます。
    1. そのどちらかの項目の URL をコピーして、ブラウザーに貼り付けます。 
    1. このテキスト ファイルがビルド アーティファクトとしてアップロードされていることがわかります。
    
    **注**:

    - Azure Storage では、コンテナー名と BLOB 名は小文字です (大文字と小文字は区別されます)。

## <a name="create-a-pipeline-job-to-download-from-azure-blob-storage"></a>Azure BLOB ストレージからダウンロードするためのパイプライン ジョブを作成する

次の手順では、項目を Azure BLOB ストレージからダウンロードするようにパイプライン ジョブを構成する方法を示します。

1. ジョブ構成の **[Pipeline]\(パイプライン\)** セクションで、 **[Pipeline script]\(パイプライン スクリプト\)** を選択し、以下を **[スクリプト]** に貼り付けます。 プレースホルダーは、お使いの環境に合った適切な値になるように編集してください。

    ```groovy
    pipeline {
      agent any
      environment {
        AZURE_SUBSCRIPTION_ID='99999999-9999-9999-9999-999999999999'
        AZURE_TENANT_ID='99999999-9999-9999-9999-999999999999'
        AZURE_STORAGE_ACCOUNT='myStorageAccount'
      }
      stages {
        stage('Build') {
          steps {
            withCredentials([usernamePassword(credentialsId: 'azuresp', 
                            passwordVariable: 'AZURE_CLIENT_SECRET', 
                            usernameVariable: 'AZURE_CLIENT_ID')]) {
              sh '''
                # Login to Azure with ServicePrincipal
                az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
                # Set default subscription
                az account set --subscription $AZURE_SUBSCRIPTION_ID
                # Execute upload to Azure
                az storage blob download --account-name $AZURE_STORAGE_ACCOUNT --container-name myjob --name hello.txt --file ${WORKSPACE}/hello.txt --auth-mode login
                # Logout from Azure
                az logout
              '''   
            }
          }
        }
      }
    }
    ```
    
1. ビルドを実行した後、ビルド履歴コンソールの出力を確認します。 または、ダウンロード先を調べて、想定した BLOB が正常にダウンロードされたかどうかを確認することもできます。  

## <a name="next-steps"></a>次のステップ

> [!div class="nextstepaction"]
> [Azure 上の Jenkins](/azure/Jenkins/)
