---
title: "IDE 에서는 왜 Lombok 플러그인을 설치해야 할까?"
date: 2020-12-03T18:47:18+09:00
tags: [Lombok]
draft: false
---
`lombok` 은 개발을 참 편리하게 해 주는 라이브러리 중 하나다.   
어제 intelliJ 의 새로운 버전이 릴리즈돼서 바로 업데이트를 했다. (난 업데이트를 해야 직성이 풀린다..)
그랬더니 lombok 플러그인이 아직 업데이트되지 않아서인지 lombok을 적용한 코드가 에러를 마구마구 뱉어냈다.
이 때, 궁금해졌다. 

> 왜 lombok은 dependency 를 추가하고도 IntelliJ 에서 플러그인을 설치 해 줘야 할까?

## Lombok 플러그인 설치 필요성

> Lombok uses annotation processing through APT, so, when the compiler calls it, the library generates new source files based on annotations in the originals.

lombok은 어노테이션 프로세싱을 사용하는데, 컴파일러가 호출하면 lombok 라이브러리는 오리지널 어노테이션 기반의 새로운 소스 파일을 생성한다. 

> As Lombok generates code only during compilation, the IDE highlights errors in raw source code.

lombok 은 컴파일 중 코드만 생성하므로, IDE 는 소스 코드를 에러로 나타낸다.

> There is a dedicated plugin which makes IntelliJ aware of the source code to be generated. After installing it, the errors go away and regular features like Find Usages, Navigate To start working.

IntelliJ(또는 이클립스 등등)이 생성 될 소스 코드를 인식하도록하는 전용 플러그인이 있다. 이 플러그인을 설치하고나면, 에러는 사라지고 IntelliJ 의 고정 기능인 `Fine Usages`, `Navigate To` 와 같은 기능들을 사용할 수 있게 된다.

결국 IDE 가 소스 코드를 파악하고 IDE에서 제공하는 기능을 사용할 수 있도록 하기위한 것으로 확인된다.
lombok 의 동작 과정을 좀 더 깊이있게 살펴봐야 겠다.

> 출처: https://www.baeldung.com/lombok-ide
