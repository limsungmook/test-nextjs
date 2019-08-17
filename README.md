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

이미지 제거

> docker rmi ipes4579/text-nextjs

