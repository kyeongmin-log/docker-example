# docker - example

-  docker 를 이용하여 nodejs 서버 동작하기

-  참고영상 : https://www.youtube.com/watch?v=LXJhA3VWXFA

## Dockerfile

-  어떤 이미지를 만들건지, 해당 서버를 만들기위해 어떤 것이 필요한 지 명시하는 곳

-  layer 형태로 이루어져 있으며 자주 바뀌는 파일은 layer에 뒤쪽으로 배치한다.

[nodejs 기준]

```docker
# [작성된 코드]
FROM node:16-alpine

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm ci

COPY  index.js .

ENTRYPOINT [ "node", "index.js" ]

# [코드 설명]
FROM node:16-alpine => baseImage : 기본적인 이미지

WORKDIR /app => 프로젝트 파일들을 docker 이미지 안에 /app 이라는 경로에 copy 해 오겠다는 것

COPY package.json package-lock.json ./ => 해당 파일을 COPY 해옴(package.json 은 서버를 이루기위해 필요한 라이브러리 정보를 가지고 있음)

RUN npm ci => 라이브러리 설치(npm install 로 해도 되지만 해당 라이브러리의 최신 버전이 나온다면 업데이트하여 설치하게 된다. 이는 서버 동작에 문제가 될 수 있으므로 npm ci 라는 명령어를 통해 package-lock.json 에 작성된 버전으로 라이브러리를 받아온다.)

COPY  index.js .

ENTRYPOINT [ "node", "index.js" ] => 서버 실행
```

## Command Line

-  build docker

```bash
docker build -f Dockerfile -t fun-docker .

# -f : 어떤 dockerfile 을 사용할 건지 명시
# -t : image 에 이름을 설정(tag)
# reference site : https://docs.docker.com/engine/reference/builder/
```

-  show docker images

```bash
docker images
```

-  run docker

```bash
docker run -d -p 8080:8080 fun-docker

# -d : detached. 터미널을 종료해도 background 에서 해당 container 가 동작하도록 한다.
# -p : host 와 container 의 port를 mapping 한다.
```

-  show docker process

```bash
docker ps
```

## docker hub

-  docker 도 github 처럼 저장소에 올려서 보관할 수 있다.

-  주소 : https://hub.docker.com/

-  사이트로 들어가서 가입한 뒤 docker-example 이라는 저장소를 만들어둔다.

-  hub 에 올리기 전에 올릴려고 하는 images 의 tag 를 변경해줘야한다. (username 은 본인이 가입한 계정으로 이용해야한다.)

```bash
docker tag fun-docker:latest kyeongminking[username]/docker-example:latest
```

-  login 후에 push 해야한다.

```bash
docker login

# (...LOGIN DONE)

docker push kyeongminking[username]/docker-example:latest
```

## 결론

-  해당 방법은 간단한 nodejs 서버를 통해 docker를 image 하여 동작시켜 이해하는 과정이다. 해당 과정으로 docker의 process를 이해하고 프로젝트를 통해 docker 를 이용해보자.
