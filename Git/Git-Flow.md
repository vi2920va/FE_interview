# Git - git-flow

## 설명

### 1. git glow 전략

git-flow에는 5가지 종류의 브랜치가 존재합니다. 항상 유지되는 메인 브랜치들(main, develop)과 일정 기간 동안만 유지되는 보조(feature, release, hotfix)가 있습니다.

- main : 제품으로 출시될 수 있는 브랜치
- develop : 다음 출시 버전을 개발하는 브랜치
- feature : 기능을 개발하는 브랜치
- release : 이번 출시 버전을 준비하는 브랜치
- hotfix : 출시 버전에서 발생한 버그를 수정하는 브랜치

## 요약

- git flow는 형상 관리 전략일 뿐이다.
- 코드 형상/이력 관리를 효율적으로 하고, 협업할 때 발생할 수 있는 문제점을 최소할 수 있는이 전략이 git flow 이다.

## 참고
- [git-flow cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/index.ko_KR.html)