## Minio

[公式ドキュメント](https://docs.min.io/)


## Minio webhook with docker-lambda

Webhookのエンドポイントとなるdocker-lambdaを立ち上げる
```
$ docker network create minio-lambda-net
$ docker run --rm \
  --name d-lambda \
  -p 9001:9001 \
  --net=minio-lambda-net \
  -e AWS_ACCESS_KEY_ID=minioadmin \
  -e AWS_SECRET_ACCESS_KEY=minioadmin \
  -e DOCKER_LAMBDA_STAY_OPEN=1 \
  -v "$PWD":/var/task \
  lambci/lambda:nodejs10.x \
  index.handler
```


次にMinio serverを立ち上げる。

```
$ docker-compose up
```

以下の手順でサーバーの設定を行う。（http://172.17.0.2:9000 の部分はdocker-compose upしたときのログをみて適宜変更）

```
$ docker run --rm --name mc --net=minio-lambda-net -it --entrypoint=/bin/sh minio/mc

# mc config host add myminio http://172.17.0.2:9000 minioadmin minioadmin
# mc mb myminio/test

# mc admin config set myminio notify_webhook:1 queue_limit="0"  endpoint="http://d-lambda:9001/2015-03-31/functions/myfunction/invocations" queue_dir=""
# mc admin service restart myminio

# mc event add myminio/test arn:minio:sqs::1:webhook --event put --suffix .mp4
```

ブラウザーを開き、testという名前のバケットにmp4をアップロードすると連番画像が生成される。
