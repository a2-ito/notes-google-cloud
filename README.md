# notes-google-cloud

## Compute Engine
- VM間での永続ディスクの共有
  - マルチライターモード
    - SSD永続ディスクを同時に最大2個のN2仮想マシンインスタンスにアタッチできる
  - 読み取り専用モードで複数のVMで永続ディスクを共有することができる
- MIGの自動ロールアウト
  - ```maxSurge``` オプション
    - 自動更新中にMIGが ```targetSize``` を超えて作成できる新しいインスタンスの数を構成します
    - 例えば、```maxSurge```を5に設定すると、MIGは新しいインスタンステンプレートを使用して、ターゲットサイズを超える最大5つの新しいインスタンスを作成します。
  - ```maxUnavailable``` オプション
    - 自動更新中に常時利用できないインスタンスの数を設定する。
    - 例えば、```maxUnavailable``` を5に設定すると、更新の際、同時にオフラインになるのは5つのインスタンスのみとなる。
  - 例
    1. ターゲットサイズを超える新しいインスタンス数の作成 5個までとする。
    1. 最大3台のオフラインマシン、最小待機時間3分を指定してローリングアップデートを実行してから、新しいインスタンスをオンラインとしてマークする
- Logging エージェントは、VMインスタンスや選択したサードパーティソフトウェアパッケージからCloud Loggingにログをストリーミングする
  - Compute EngineとAmazon Elastic Compute CloudのVMイメージにはLoggingエージェントが含まれていない
  - Windows/Linux 両方あり
- Opsエージェント
  - 標準的なCloud Loggingエージェントに比べてスループットが高い
  - a

## Cloud Storage
- オブジェクトのライフサイクル管理
  - Cloud Storage では、オブジェクトの有効期限（TTL）の設定、非現行バージョンの保持、コスト管理を容易にするためのストレージクラス「ダウングレード」など、一般的なユースケースをサポートするため、オブジェクトのライフサイクル管理機能を提供しています。
- ライフサイクル管理のユースケース例
  - 365日以上経過したオブジェクトのストレージクラスを Coldline Storage にダウングレードする
  - 2013年1月1日より以前に作成されたオブジェクトを削除する。
  - バージョニングが有効になっているバケット内の各オブジェクトで、最新の3バージョンのみを維持する。
- Cloud Storage の「クライアント側の暗号化」を利用する。
  - これを利用すると、Cloud Storage に送信される前に暗号化処理が行われるため、転送時も保存時も暗号化された状態が保たれる。、
- ストレージクラス
  - Standard
    - 最小保存期間　なし
  - Nearline
    - 最小保存期間　30日
    - 月に1回程度しか読み取りや変更を行わないデータに適する
    - データのバックアップ、ロングテールのマルチメディアコンテンツ、データ・アーカイブに適している
  - Coldline
    - 最小保存期間　90日
    - 四半期に1回程度しか読み取りや変更を行わないデータに適する
  - Archive
    - 最小保存期間　365日
    - 1年に1回未満しかアクセスしないデータに最適
- Providing Diagnostic Output To Cloud Storage Team
```
gsutil perfdiag -o output.json gs://your-bucket
```
- 複合オブジェクト
  - データ転送せずに既存のオブジェクトから作成する
  - ソースオブジェクトは影響を受けない
  - 1～32個のソースオブジェクトを使用可能

## Datastore

## Cloud SQL
- Cloud SQL は最大10TB
Cloud SQL インスタンスのリードレプリカを作成するには、インスタンスが次の要件を満たしている必要があります。
- バイナリロギングが有効になっていること。
- バイナリロギングを有効にした後に、少なくとも１つのバックアップが作成されていること。


## Dataproc
- Cloud Dataproc は、オープンソースのデータツールを利用してバッチ処理、クエリ実行、ストリーミング、機械学習を行えるマネージド Apache Spark / Apache Hadoop サービスです。
- オートスケールしない
- use cases
  - Data mining and analysis in datasets of known size
  - Migrate on-premises Hadoop jobs to the cloud

## Dataflow
- データの信頼性と表現力を損なうことなく、ストリーム（リアルタイム）モードまたはバッチ（履歴）モードでデータを変換して拡充する、フルマネージド サービスです。
- このサービスを利用すれば、複雑な回避策を用意したり、妥協策を講じたりする必要はなくなります。
- さらに、サーバーレス アプローチによるリソースのプロビジョニングと管理により、実質無制限の容量を従量課金制で使用して、膨大な量のデータ処理の問題を解決することができます。

## BigQuery
- roles/bigquery.jobUser
  - プロジェクト内でジョブ（クエリを含む）を実行する権限を付与します。
- バッチクエリの実行
  - キューに格納し、アイドル状態のリソースがBigQuery共有リソースプールで使用可能になり次第、クエリを開始する。
  - バッチクエリは同時実行レート制限に対してカウントされない。
- JOIN演算
  - INNER JOIN
    - 結合条件をみたさないすべての行を廃棄する
  - CROSS JOIN
    - M*N

## リソースの範囲

### Global Resource
 - イメージ
 - スナップショット
 - インスタンス テンプレート
 - VPC ネットワーク
 - ファイアウォール
 - ルート
 - グローバル オペレーション

### Region Resource
 - アドレス
 - サブネット
 - リージョンマネージドインスタンスグループ
 - リージョンオペレーション

### Zone Resource
 - インスタンス
 - ディスク
 - マシンタイプ
 - ゾーンマネージドインスタンスグループ
 - ゾーンオペレーション

### イメージとスナップショット
- どちらもグローバルリソース
- 価格はイメージの方が高い
- インスタンステンプレートとして指定可能なのは、イメージのみ

## App Engine
- Standard Environment と比較して Flexible Environment の利点
  - SSH可能
  - サードパーティバイナリのインストール
  - ローカルディスクへの書き込み
- インスタンスクラス
	- 自動スケーリング F1, F2, F4, F4_1G
	- 手動スケーリング B1, B2, B4, B4_1G, B8
- ```basic_scaling```
	- ```max_instances``` : 必須。最大のインスタンス数を指定する。
	- ```idle_timeout``` : 省略可能。最後のリクエストを受信した後、この時間が経過すると、インスタンスはシャットダウンされます。デフォルトは5分。
- ```manual_scaling```
	- ```instances``` : 起動時にサービスに割り当てるインスタンス数

## Cloud Endpoint

## GKE Kubernetes Engine
- GKEクラスタオートスケーラー
  - 特定のノードプール内のノード数を自動的に変更する。
  - ノードプールの最小サイズと最大サイズを指定するだけでよい。
- アップグレード完了時間を短縮するには、サージアップグレード機能が利用可能
  - 複数のノードを同時にアップグレードする
  - ```maxUnavailale```
    - 更新処理において利用不可となる最大のPod数を指定する
    - デフォルトは 25%
  - ```maxSurge```
    - 理想状態のPod数を超えて作成できる最大のPod数を指定する
  - 例 : replicas=3の場合
    - maxSurge=0, maxUnavailable=1 の場合、maxSurgeが0なので3Podまでしか起動を許されない。一方でmaxUnavailableが1なので、1Podは減少してよい。旧バージョンが1Pod減らされるため、旧podは2つでアップグレードする。
    - maxSurge=1, maxUnavailable=0 の場合、maxSurgeが1なので合計4Podまでの起動が許可される。maxUnavailableが0なので、3 replicasに対し1Podも減少を許さない。3 replicasを保ったまま、新バージョンに段階的に移行する。
下のようなイメージ。
## Bigtable
- 行キーアンチパターン
  - タイムスタンプ
  - 関連データをグループにまとめられないもの
  - シーケンシャル数値
  - 頻繁に更新されるもの
  - ハッシュ値
    - クエリに最適な形で行を格納できなくなる

## Cloud Debugger
- デバッグスナップショット
  - ソースコード内の特定の行位置でローカル変数とコールスタックを収集する。
  - スナップショット条件を指定可能。
- デバッグログポイント
  - サービスを再起動したりサービスの通常の機能を妨げたりせずに、実行中のサービスにロギングを挿入できる。
  - インスタンスがログポイントの場所のコードを実行する度に、Cloudデバッガがメッセージを記録する。

## Cloud Logging
- Compute EngineとEC2のVMイメージにはLoggingエージェントが含まれていないため、インストールが必要。

## Pub/Sub
- pull配信
  - サブスクライバーアプリケーションがPub/Subサーバに対してリクエストを開始し、メッセージを取得する
  - メッセージが多い場合（1秒間に1個以上）
  - メッセージ処理の効率とスループットが重要
- push配信
  - Pub/Subがサブスクライバーアプリケーションに対してリクエストを開始し、メッセージを配信する
  - 同じwebhookで処理する必要がある複数のトピック










