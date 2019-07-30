# docker-compose-python-simple

## 概要

Pythonスクリプトを実行するためのDockerコンテナを構築する

## 環境

```
$ sw_vers
ProductName:	Mac OS X
ProductVersion:	10.13.6
BuildVersion:	17G7024

$ docker --version
Docker version 18.09.2, build 6247962

$ docker-compose version
docker-compose version 1.23.2, build 1110ad01
docker-py version: 3.6.0
CPython version: 3.6.6
OpenSSL version: OpenSSL 1.1.0h  27 Mar 2018
```

## 事前準備

Docker, Docker-Composeのインストールをおこなってください。
それぞれのインストール手順につきましては年月の経過によって差異が発生する可能性があるので本ドキュメントでの記載は割愛しております。

## 構築手順

### 1. ソースファイルの取得

[zipファイルをダウンロード](https://github.com/tokihidenori/docker-compose-python-simple/archive/master.zip)

### 2. Dockerセットアップ

下記のコマンドを実行すると、Dockerコンテナを生成し起動します。

```
docker-compose up -d --build
```

正常時のログは以下の通りです。

```
Building python3
Step 1/16 : FROM python:3
 ---> 59a8c21b72d4
Step 2/16 : USER root
 ---> Using cache
 ---> 6de793603cda
Step 3/16 : RUN apt-get update
 ---> Using cache
 ---> 32db1a9df083
Step 4/16 : RUN apt-get -y install locales &&     localedef -f UTF-8 -i ja_JP ja_JP.UTF-8
 ---> Using cache
 ---> 9025491ada8d
Step 5/16 : ENV LANG ja_JP.UTF-8
 ---> Using cache
 ---> f3b21016e7cc
Step 6/16 : ENV LANGUAGE ja_JP:ja
 ---> Using cache
 ---> 90553dad0d33
Step 7/16 : ENV LC_ALL ja_JP.UTF-8
 ---> Using cache
 ---> 5f5605523288
Step 8/16 : ENV TZ JST-9
 ---> Using cache
 ---> 3eddbc41a8da
Step 9/16 : ENV TERM xterm
 ---> Using cache
 ---> d6cd32222a59
Step 10/16 : RUN apt-get install -y vim less
 ---> Using cache
 ---> ea600f3448c6
Step 11/16 : RUN pip install --upgrade pip
 ---> Using cache
 ---> 2c7f57cadd05
Step 12/16 : RUN pip install --upgrade setuptools
 ---> Using cache
 ---> e58815e7eea1
Step 13/16 : ADD ./opt /root/opt
 ---> Using cache
 ---> 772a595f74c7
Step 14/16 : WORKDIR /root
 ---> Using cache
 ---> 2a3ea1eeb2d6
Step 15/16 : RUN pip install --upgrade pip
 ---> Running in a80eb572214b
Collecting pip
  Downloading https://files.pythonhosted.org/packages/62/ca/94d32a6516ed197a491d17d46595ce58a83cbb2fca280414e57cd86b84dc/pip-19.2.1-py2.py3-none-any.whl (1.4MB)
Installing collected packages: pip
  Found existing installation: pip 19.0.3
    Uninstalling pip-19.0.3:
      Successfully uninstalled pip-19.0.3
Successfully installed pip-19.2.1
Removing intermediate container a80eb572214b
 ---> 683df74eea5f
Step 16/16 : RUN pip install -r opt/requirements.txt
 ---> Running in fedd2b6acad5
Removing intermediate container fedd2b6acad5
 ---> 308924b5cd06
Successfully built 308924b5cd06
Successfully tagged docker-compose-python-simple_python3:latest
Recreating python_simple ... done
```


## Docker操作

### コンテナ一覧の確認

```
docker ps
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                                            NAMES
13247119d223        clowler_python3          "python3"                3 months ago        Up 4 weeks                                                           python3
```

### Dockerコンテナへの接続

```
docker exec -i -t #{接続したいコンテナのID} bash
```

上記手順通りセットアップした場合は
```
docker exec -i -t python_simple bash
```
で接続できるはずです。

## sample.py実行までの手順

### 1. Dockerコンテナ起動

```
docker-compose up -d --build
```

### 2. Dockerコンテナへの接続

```
docker exec -i -t python_simple bash
```
コンテナ名が異なる場合は「docker ps」でコンテナ名を確認してください。

### 3. pythonの実行

```
python src/sample.py
```

下記のような結果が表示されれば成功です。

```
Hello, Docker in Python!
```
