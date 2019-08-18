## next.js

## 도커 

빌드

> docker build -t ipes4579/test-nextjs .

이미지 확인
> docker images

이미지 실행

> docker run -p 3000:3000 -d ipes4579/test-nextjs

컨테이너 ID 확인

> docker ps

앱 로그 출력

> docker logs \<container id>

컨테이너 안으로 들어감

> docker exec -it \<container id> /bin/bash

컨테이너 제거

> docker stop \<container id> && docker rm \<container id>

도커 로그인

> docker login

도커 푸시

> docker push ipes4579/test-nextjs

이미지 제거

> docker rmi ipes4579/text-nextjs

## K8s

Replication Controller 생성

> kubectl create -f test-nextjs-replication-controller.yaml

Pod 이 잘 생성되었는지 확인

> kubectl get pods

describe 로 확인 (성공했을 때나 오류 확인시나 동일)

> kubectl describe pod text-nextjs-rc-XXXX

RUNNING 이라면 첫 번째 pod 의 IP 주소를 확인해서 저장해놓자. 그리고 다른 Pod 에서 wget 으로 날려보자(curl 은 alpine 에 없어서..). 아래 명령에서 두 번째 팟 이름과 첫 번째 팟 IP 로 바꾸자.

> kubectl exec -it test-nextjs-rc-qbl85 -- wget -qO- 10.1.0.17:3000
이러한 결과가 나와야함
```
<!DOCTYPE html><html><head><meta charSet="utf-8"/><meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1"/><meta name="next-head-count" content="2"/><link rel="preload" href="/_next/static/fJBSG7YiHfNsplJe5FWMZ/pages/index.js" as="script"/><link rel="preload" href="/_next/static/fJBSG7YiHfNsplJe5FWMZ/pages/_app.js" as="script"/><link rel="preload" href="/_next/static/runtime/webpack-3df6523e264ff2ac6548.js" as="script"/><link rel="preload" href="/_next/static/chunks/commons.d1e43511aad0240f79a0.js" as="script"/><link rel="preload" href="/_next/static/runtime/main-110164ad59d82497a6e0.js" as="script"/></head><body><div id="__next"><div><p>Hello Next.js</p></div></div><script id="__NEXT_DATA__" type="application/json">{"dataManager":"[]","props":{"pageProps":{}},"page":"/","query":{},"buildId":"fJBSG7YiHfNsplJe5FWMZ","dynamicBuildId":false,"nextExport":true}</script><script async="" id="__NEXT_PAGE__/" src="/_next/static/fJBSG7YiHfNsplJe5FWMZ/pages/index.js"></script><script async="" id="__NEXT_PAGE__/_app" src="/_next/static/fJBSG7YiHfNsplJe5FWMZ/pages/_app.js"></script><script src="/_next/static/runtime/webpack-3df6523e264ff2ac6548.js" async=""></script><script src="/_next/static/chunks/commons.d1e43511aad0240f79a0.js" async=""></script><script src="/_next/static/runtime/main-110164ad59d82497a6e
```

혹시 오류가 있었다면 파일을 변경한 후 다시 apply 해주자

> kubectl apply -f test-nextjs-replication-controller.yaml

서비스를 생성해서 외부 포트를 오픈해보자. 

> kubectl create -f test-nextjs-service.yaml 

서비스가 떠있는지 확인해보자

> kubectl get service

서비스가 떠 있으면 로컬에 curl 해보자

> curl localhost:3000

pod 을 모두 제거해보자. 모두 제거해도 Replication Controller 에 의해 다시 실행된다

> kubectl delete pod --all
> kubectl get pods

RC와 서비스를 제거하려면 아래 명령어를 입력

> kubectl delete rc test-nextjs-rc
> kubectl delete svc test-nextjs-service

