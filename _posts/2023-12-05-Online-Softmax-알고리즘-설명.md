---
title: Online Softmax 알고리즘 설명
tags: 딥러닝
key: 202312050956
---

안녕하세요? 삼각형입니다.

Softmax는 다중 클래스 분류에서 자주 사용되는 연산입니다. Softmax가 다중 클래스 분류에서 자주 사용되는
이유에는 여러가지가 있습니다.

1. Softmax는 출력을 확률로 변환합니다. 각 클래스에 대한 출력값은 0과 1사이의 값으로, 모두 합하면 1이
됩니다. 이는 각 클래스에 속할 확률을 직관적으로 나타냅니다.
2. Softmax는 큰 값에는 더 큰 가중치를 부여하고 작은 값은 더 줄여버리는 특성을 가지고 있습니다.
3. Softmax는 모든 곳에서 미분 가능합니다. 이러한 성질은 최적화 알고리즘을 사용할 때 중요합니다.

Softmax 계산의 효율성을 향상시킬 수 있다면, 이는 딥러닝 모델 개발의 속도를 현저히 개선할 수 있을
것입니다. 이러한 맥락에서, Softmax의 속도를 향상시킬 수 있는 Online Softmax 알고리즘에 대해
알아보겠습니다.

### Safe Softmax 알고리즘

일반적으로 Safe Softmax 알고리즘을 사용하여 Softmax를 구현합니다. 이 알고리즘은 Softmax 계산
과정에서 발생할 수 있는 오버플로우와 언더플로우 문제를 효과적으로 방지하여 안정적인 계산이 가능해집니다.

Safe Softmax 알고리즘은 다음과 같습니다.

1. 벡터 원소 중 **최대값** 찾기
2. $$m = \sum_{i = 1}^V{e^{x_i-max}}$$ 계산
3. $$\frac{e^{x_i}}{m}$$ 계산

이 알고리즘은 Softmax 계산을 위해 벡터 원소에 총 **3번** 접근합니다. 컴퓨터 시스템에서 **메모리
접근 비용**은 **매우 비싸기** 때문에 벡터 원소의 접근 횟수를 줄일 수 있다면 계산 속도를 향상시킬 수
있습니다.

### Online Softmax 알고리즘

Online Softmax 알고리즘은 벡터 원소의 접근 횟수를 총 **2번**으로 줄입니다. 이 알고리즘을 살펴보기
전에 얼마나 속도가 개선되는지 확인해 보겠습니다.

<div style="text-align: center">
  <img src="https://github.com/daemyung/daemyung.github.io/assets/7459074/8230d457-2760-4f09-9e35-dc8091ceef37" width="720">
</div>

이 차트는 Online Softmax 알고리즘과 Safe Softmax 알고리즘의 성능을 비교한 것입니다. 벡터의 크기가
작을 때는 두 알고리즘 간의 속도 차이가 거의 없습니다. 그러나 벡터의 크기가 커짐에 따라 속도 차이가
나타나기 시작합니다. 속도 차이가 발생하는 이유는 메모리 접근 횟수가 다르기 때문입니다.

크기가 $$V$$인 벡터 $$X = [x_1, x_2, ..., x_V]$$에 대해서 Online Softmax 알고리즘을 적용한 의사
코드는 다음과 같습니다.

<div style="text-align: center">
  <img src="https://github.com/daemyung/daemyung.github.io/assets/7459074/e1d4a9ac-4120-421f-93b7-b787548a3bc8" width="720">
</div>

의사 코드의 3~4 부분에서 벡터 원소에 대한 접근 횟수를 2번에서 1번으로 줄여 속도를 개선합니다.

### Online Softmax 병합

GPU에서 Online Softmax 알고리즘을 효과적으로 실행하기 위해서는 병합이 반드시 성립해야합니다. 병합이
성립하지 않을 경우, GPU를 충분히 활용하지 못하여 속도가 저하될 수 있습니다. 다행히 Online Softmax
알고리즘은 병합이 성립합니다.

<div style="text-align: center">
  <img src="https://github.com/daemyung/daemyung.github.io/assets/7459074/ebca1597-14a6-40c6-ad78-0edd8d80221d" width="720">
</div>

수식에서 알 수 있듯이 계산된 두 결과를 병합하여 원하는 최종값을 계산할 수 있습니다.

### 참고

- [Online normalizer calculation for softmax](https://arxiv.org/pdf/1805.02867.pdf)

감사합니다.

<!--more-->
