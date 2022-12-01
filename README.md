# Monorepo CI & CD workflows

## Outline
모노래포 형태의 백엔드([GO](https://go.dev/)) 통합 저장소에서 브랜치별로 배포하는 방법에 대해서 설명한다.
배포도구로는 git workflow를 사용한다.

## Feature
- 서비스는 각각 배포(dev, release)가 가능하다
- 서비스 배포 이력은 각각 tag로 관리한다
- 과도하게 브랜치 개수를 늘리지 않는다

## Detail
메인 브랜치로는 develop 브랜치를 사용한다. 각 마이크로서비스 코드들은 develop 브랜치에 병합되며 병합 과정에서 발생하는 이슈를 해결하기 위해서 test workflow가 동작한다.

테스트가 정상적으로 동작하고 병합된 develop 브랜치에서는 일정한 패턴의 태그를 생성하고 원격 저장소에 push 함으로써 해당 버전의 서비스를 배포 할 수 있다.
예를들어, serviceA를 develop에 v1.0.0 버전을 배포하고 싶다면 develop 브랜치에서 `develop-serviceA-v1.0.0`과 같은 태그를 생성하고 원격 저장소에 push하면 된다.

release도 develop 브랜치와 동일하다.

## Usage
```shell
# serviceA 배포 과정
# 1. develop 브랜치를 기준으로 feature 브랜치를 생성
$ git checkout -b feature/serviceA#login

# 2. feature/serviceA#login -> develop PR 요청 후 병합
# 3. develop 브랜치로 이동해서 태그를 생성
$ git tag develop-serviceA-v1.0.0 

# 4. 태그 원격 저장소에 push (deploy workflow trigger)
$ git push origin develop-serviceA-v1.0.0
```