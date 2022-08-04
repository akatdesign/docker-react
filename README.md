## docker-react


**環境構築**
- file name frontで作成
``` bash
$ cd docker-react/
$ docker-compose build
$ docker-compose up -d
$ docker exec -it node sh
$ npx create-react-app . --template typescript --use-npm
$ npm start
``` 
**ホットリロード(windows)**
- windowsの場合jsonが確実、いずれかを試す
```bash
package.js
"scripts": {
  "start": "WATCHPACK_POLLING=true react-scripts start",
  "build": ...
},

.env
CHOKIDAR_USEPOLLING=true

docker-compose.yml
    environment:
      - ....
      - CHOKIDAR_USEPOLLING=true
```
**環境から抜ける**
```bash
$ exit
$ docker compose down
$ docker image rm imageid
```
**確認**
```bash
$ docker image ls
$ docker container ls -a
```
