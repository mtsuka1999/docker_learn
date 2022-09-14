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