# awscli の Docker イメージの生成と使い方

```
$ docker build -t awscli aws
```

```
$ docker run -v $PWD:/work -v $HOME/.aws:/root/.aws -w /work -it --rm awscli
```

```
# aws cloudformation create-stack --stack-name 'codeartifact' --template-body "`cat codeartifact.yml`"
```

# Maven の設定

`~/.m2/settings.xml` に次の内容でファイルを作成します。

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 https://maven.apache.org/xsd/settings-1.0.0.xsd">
    <servers>
        <server>
            <id>aws</id>
            <username>aws</username>
            <password>${env.CODEARTIFACT_AUTH_TOKEN}</password>
        </server>
    </servers>
    <profiles>
        <profile>
            <id>aws</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <repositories>
                <repository>
                    <id>aws</id>
                    <url>${env.AWS_CODE_ARTIFACT_URL}</url>
                </repository>
            </repositories>
        </profile>
    </profiles>
</settings>
```

IAM のポリシーには `"sts:GetServiceBearerToken"` が必要です。

AWS CodeArtifact のリポジトリにアクセスする URL を設定します。

```
$ export AWS_CODE_ARTIFACT_URL=<Repo.url>
```

トークンを取得して環境変数に設定します。

```
$ export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token --domain <YOUR DOMAIN> --query authorizationToken --output text`
```
