---
title: CLion 원격 개발에서 CUDA를 찾지 못하는 문제
tags: JetBrains CLion
key: 202312181105
---

안녕하세요? 삼각형입니다.

CLion 원격 개발에서 CMake가 CUDA를 찾지 못하는 경우가 있습니다. 이 문제는 Bash가 대화형으로
실행되지 않아 환경변수 설정 전에 스크립트가 종료되기 때문에 발생합니다. 이를 해결하는 방법은 다음과
같습니다.

1. `/etc/bash.bashrc`를 엽니다.
   ```
   sudo vi /etc/bash.bashrc
   ```
2. Bash가 대화형이 아닌 경우에도 CUDA 환경변수를 설정할 수 있도록 해당 부분을 수정합니다. 개발 환경에
   따라 이 파일의 내용은 달라질 수 있습니다.
   - Ubuntu 22.04
     ```
     # If not running interactively, don't do anything
     case $- in
         *i*) ;;
           *) export PATH=/usr/local/cuda/bin:${PATH}
              export LD_LIBRARY_PATH=/usr/local/cuda/lib64:${LD_LIBRARY_PATH}
              return;;
     esac
     ```
   - Ubuntu 20.04
     ```
     # If not running interactively, don't do anything
     if [ -z "$PS1" ]; then
         export PATH=/usr/local/cuda/bin:${PATH}
         export LD_LIBRARY_PATH=/usr/local/cuda/lib64:${LD_LIBRARY_PATH}
         return
     fi
     ```

이러한 수정 후에 CLion을 재실행하면 CMake가 CUDA를 인식하는 것을 확인할 수 있습니다.

감사합니다.

<!--more-->
