MicroStackにオーケストレーション機能、特にOpenStackのオーケストレーションサービスであるHeatを追加するには、いくつかのステップが必要です。ただし、MicroStackは主に簡易的なテストやデモンストレーション用途に使われるため、全てのOpenStackコンポーネントがプリインストールされているわけではありません。Heatのようなオーケストレーション機能は、デフォルトでは含まれていません。

以下は、MicroStackにHeatを統合するための基本的な手順です。これは高度な設定が必要であり、OpenStackのインスタンス内部での作業が伴います：

1. **Heatのインストール**: UbuntuサーバーにHeatをインストールするため、まずは必要なパッケージを取得します。以下のコマンドを実行してHeat関連のコンポーネントをインストールします：
   ```bash
   sudo apt update
   sudo apt install heat-api heat-api-cfn heat-engine
   ```

2. **データベースの設定**: Heatはデータベースを使用して情報を格納します。MySQLまたはMariaDBが使われることが多いです。MicroStackではデフォルトでSQLiteが使われることが多いので、MySQLに変更する場合は、MySQLにHeatのデータベースとユーザーを設定する必要があります。

3. **Heatの設定**: `/etc/heat/heat.conf` ファイルを編集し、Keystoneとの接続情報、RabbitMQ（メッセージブローカー）、データベース設定などを更新します。

4. **サービスの登録とエンドポイントの作成**: KeystoneにHeatサービスを登録し、適切なAPIエンドポイントを作成します。これには、以下のようなコマンドを使用します：
   ```bash
   openstack service create --name heat --description "Orchestration" orchestration
   openstack service create --name heat-cfn --description "Orchestration CloudFormation" cloudformation
   openstack endpoint create --region RegionOne orchestration public http://<your-ip>:8004/v1/%\(tenant_id\)s
   openstack endpoint create --region RegionOne cloudformation public http://<your-ip>:8000/v1
   ```

Heatの主要コンポーネントであるHeat API、Heat API for AWS CloudFormation（heat-api-cfn）、およびHeat Engineのサービスを起動するためのコマンドは、Ubuntuやその他のLinuxディストリビューションで一般的に使用されるsystemdを使います。以下は、これらのサービスを起動し、自動起動を設定するための一連のコマンドです。

### Heat サービスの起動

1. **Heat APIの起動**
   ```bash
   sudo systemctl start heat-api
   ```

2. **Heat API for CloudFormationの起動**
   ```bash
   sudo systemctl start heat-api-cfn
   ```

3. **Heat Engineの起動**
   ```bash
   sudo systemctl start heat-engine
   ```

### サービスの自動起動設定

各サービスがシステムの起動時に自動的に起動するように設定します。

1. **Heat APIの自動起動設定**
   ```bash
   sudo systemctl enable heat-api
   ```

2. **Heat API for CloudFormationの自動起動設定**
   ```bash
   sudo systemctl enable heat-api-cfn
   ```

3. **Heat Engineの自動起動設定**
   ```bash
   sudo systemctl enable heat-engine
   ```

### ステータス確認

各サービスが正常に起動しているかを確認するためには、以下のコマンドを使用します。

1. **Heat APIのステータス確認**
   ```bash
   sudo systemctl status heat-api
   ```

2. **Heat API for CloudFormationのステータス確認**
   ```bash
   sudo systemctl status heat-api-cfn
   ```

3. **Heat Engineのステータス確認**
   ```bash
   sudo systemctl status heat-engine
   ```

これらのコマンドを実行することで、Heatの各サービスを管理し、問題がある場合はログをチェックしてトラブルシューティングを行うことができます。また、何かエラーが発生している場合は、エラーメッセージを確認し、設定が正しく行われているかを再確認する必要があります。

6. **テスト**: Heatが正しく設定されているかをテストするために、Heatテンプレートを使ってスタックを作成し試みます。

このプロセスは複雑であり、OpenStackの深い理解を必要とします。また、MicroStackと統合する際には特有の課題が発生することがあります。正式なサポートやコミュニティのガイダンスを受けながら進めることをお勧めします。
