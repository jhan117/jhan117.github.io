---
title: "알고리즘 면접 대비"
categories:
  - "algorithm"
toc: true
toc_label: "알고리즘 면접 대비"
toc_sticky: true
last_modified_at: 2022-11-28
---

## DP와 조건에 대해서 설명해주세요

DP, 동적 계획법은 복잡한 문제를 간단한 여러개의 문제로 나누어 푸는 방법입니다. 즉, 똑같은 연산을 반복하지 않도록 만들어줍니다. 이는 두가지 조건이 충족할 때 DP를 이용하여 문제를 풀 수 있습니다. 먼저 작은 문제에서 반복이 일어나야 합니다. 두번째로는 같은 문제는 항상 정답이 같아야 합니다. 이 조건들을 이용해 메모이제이션으로 큰 문제를 해결해나가는 것입니다. 이를 구현할 수 있는 방식에는 두 가지가 있는데 Bottom-up 방식은 작은 문제부터 차근차근 구하는 방법이며 해결에 용이하지만 가독성이 떨어지는 반면에 Top-down 방식은 큰 문제를 풀다가 풀리지 않은 작은 문제가 있다면 그 때 해결하는 방법으로 가독성에는 좋지만 코드 작성이 힘듭니다.

## Bubble Sort vs Selection Sort vs Insertion Sort vs Quick Sort

### Bubble Sort

거품 정렬은 서로 인접한 두 원소의 대소를 비교하고 조건에 맞지 않다면 자리를 교환하며 정렬하는 알고리즘입니다. 시간 복잡도는 O(N^2)이 소요되고 공간 복잡도는 O(N)입니다. 이는 구현이 매우 간단하고 소스코드가 직관적이며 제자리 정렬이기에 다른 메모리 공간을 필요로 하지 않는다는 장점이 있습니다. 또한, 안정 정렬입니다. 반면에 시간 복잡도가 최악, 최선, 평균 모두 O(N^2)이므로 굉장히 비효율적이라는 단점이 있습니다.

### Selection Sort

선택 정렬은 해당 자리를 선택하고 그 자리에 오는 값을 찾는 알고리즘입니다. 시간 복잡도는 O(N^2)이 소요되며 공간복잡도는 O(N)입니다. 이는 알고리즘이 단순하며 비교 횟수는 많지만 거품 정렬보다는 실제로 교환하는 횟수가 적기 때문에 많은 교환이 일어나야 하는 자료 상태에서는 비교적 효율적입니다. 또한, 이는 제자리 정렬이므로 다른 메모리 공간을 필요하지 않습니다. 반면에 시간 복잡도가 O(N^2)으로 비효율적이며 불안정 정렬이라는 단점이 있습니다.

### Insertion Sort

삽입 정렬은 2번째 원소부터 시작해서 그 앞의 원소들과 비교하며 삽입할 위치를 지정한 후, 원소를 뒤로 옮기고 지정된 자리에 자료를 삽입하여 정렬하는 알고리즘입니다. 시간 복잡도는 최선의 경우 O(N), 평균과 최악의 경우 O(N^2)이 소요됩니다. 공간 복잡도는 O(N)입니다. 이는 알고리즘이 단순하며 대부분 정렬되어 있는 경우 매우 효율적입니다. 또한, 제자리 정렬이므로 다른 메모리 공간을 필요로 하지 않으며 안정 정렬이고 선택 정렬이나 거품 정렬과 같은 알고리즘에 비교하면 상대적으로 빠릅니다. 반면에 평균과 최악의 시간 복잡도가 O(N^2)이므로 비효율적이고 배열의 길이가 길어질수록 비효율적이라는 단점이 있습니다.

### Quick Sort

퀵 정렬은 비균등한 분할 정복 방법을 통해 주어진 배열을 정렬합니다. 시간 복잡도는 최선의 경우 O(NLogN), 최악의 경우 O(N^2), 평균의 경우 O(NLogN)이 소요됩니다. 공간 복잡도는 O(N)입니다. 이는 불필요한 데이터의 이동을 줄이고 먼 거리 데이터를 교환할 뿐만 아니라 결정된 피벗들은 추후 연산에서 제외되는 특성 때문에 O(NLogN)을 가지는 다른 정렬 알고리즘보다 가장 빠릅니다. 제자리 정렬이므로 다른 메모리 공간을 필요로하지 않는 장점이 있습니다. 반면에 불안정 정렬이므로 정렬된 배열에 대해서는 Quick Sort의 불균형 분할에 의해 오히려 수행 시간이 더 많이 걸립니다.

## DFS vs BFS

DFS는 스택 또는 재귀함수를 통해 구현하며 깊게 모든 자식 노드를 탐색하고 넘어가는 특징으로 모든 경로를 방문해야 할 경우 적합합니다. BFS는 큐를 통해 구형하며 인접한 노드부터 먼저 탐색하는 특징으로 모든 경로를 방문하는 것보다 최소 비용이 우선일 때 적합합니다.