---
title: CLion 원격 개발에서 Inspection 문제
tags: JetBrains CLion
key: 202312221253
---

안녕하세요? 삼각형입니다.

CLion 원격 개발시 Inspection이 제대로 동작하지 않는 문제가 발생할 수 있습니다. 이러한 상황에서는
`No template named 'vector' in namespace 'std'`와 같은 에러 메세지가 나타납니다. 이 문제는
Insepction에 필요한 헤더 파일들이 다운로드되지 않았기 때문에 발생하는 것입니다. 이를 해결하는 방법은
다음과 같습니다.

1. CLion을 종료합니다.
2. CLion의 캐시 폴더를 삭제합니다.
   ```
   rm -rf ~/Library/Caches/JetBrains/CLion
   ```

이후에 CLion을 재실행하면 Inspection이 제대로 동작하는 것을 확인할 수 있습니다.

감사합니다.

<!--more-->
