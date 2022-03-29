Learn Laravel ECS deploy with Docker Context
============================================

## Prepare
### AWS CLIコマンド実行の準備をする
1. AWS IAMで作成し、アクセスキーとシークレットキーを取得する
2. 環境変数を以下のように指定する
  ```shell
  > export AWS_ACCESS_KEY_ID="[取得したアクセスキー]"
  > export AWS_SECRET_ACCESS_KEY="[取得したシークレットキー]"
  > export AWS_DEFAULT_REGION="[AWSのリージョン]"
  ```
3. 以下のコマンドを実行し、AWSにアクセスできていることを確認する
  ```shell
  > aws iam get-user
  ```

### Docker contextでECRコンテキストを作成する
前述のAWS CLIコマンド実行の準備を行った後に以下を実行する。
```shell
> docker context create ecs ecs-context
? Create a Docker context using: An existing AWS profile
? Select AWS Profile default
Successfully created ecs context "ecs-context"
```

## Usage
以下の手順はdev環境に対してデプロイする場合の流れ。

1. ECRにログインする（`<aws_account_id>` と `<region>` は環境に合わせる。）
  ```shell
  > aws ecr get-login-password | docker login --username AWS --password-stdin https://<aws_account_id>.dkr.ecr.<region>.amazonaws.com
  ```

2. Dockerfileをビルドしてイメージを作成する
  ```shell
  > docker compose -f docker-compose.dev.yml build
  ```

3. 作成したイメージをECRにPushする
  ```shell
  > docker compose -f docker-compose.dev.yml push
  ```

4. docker contextでECSコンテキストに切り替える
  ```shell
  > docker context use ecs-context
  ```

5. compose upでECS上にコンテナを展開する
  ```shell
  > docker compose -f docker-compose.dev.yml up
  ```
