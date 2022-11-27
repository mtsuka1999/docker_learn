米国AI開発者がゼロから教えるDocker講座(https://www.udemy.com/course/aidocker/)の学習記録
$docker login
$docker pull <image>
$docker run -it <image> bash imageからコンテナを作成してそのコンテナに入る
$exit
$docker ps -a 
$docker images
$docker restart コンテナをupにする
$docker exec -it <container> bash コンテナを起動
$docker commit <container> <image> containerを指定してimageを作成
$docker tag <source> <target> sourceのimagesをtargetに新しい名前を入れる
$docker push <image> dockerHubにプッシュする(image名とdockerHubのrepository名を一致させる)
$docker rmi <image> imageを削除

$docker run <image> run=crate+ start
$docker create <image>  
$docker start -a <container>設定されたコマンドを出力
$docker run -it ubuntu bash 
-itとは、、、
-i:インプット可能　
-t:表示がきれいになる
コンテナの削除
$docker stop <container>
$docker rm <contianer>
コンテナの全削除
$docker system prune

--------------------------------------------------------------------------------------------------------
Dockerfileの作成
FROM 土台となるosを指定
RUN RUNのあとに書かれたコマンドを(Linuxで実行)、RUNごとにlayerが作成
CMD
実行
$docker build <Dokerfileのあるディレクトリ>
$docker run -it <nameを付ける> bash

Dockerimageのlayer数を最小限にする。
→Layerを作成するのはRUn,COPY,ADDの３つ
→コマンドを&&でつなげる