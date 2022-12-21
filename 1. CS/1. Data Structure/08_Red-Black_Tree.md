### Red-Black Tree

- Red-Black 트리란 자가 균형 이진 탐색 트리로서, 좌, 우 균형이 맞춰지는 이진 탐색 트리이다.
- 트리 균형에 있어 높이 차이가 최대 1까지 차이날 수 있는 이진 탐색 트리이다.
- 특징
    - 모든 노드는 Red 또는 Black이다.
    - 루트 노드는 Black이다.
    - 리프 노드들은 Black이다.(리프 노드들은 NULL 노드들이다.)
    - Red 노드의 자식 노드는 Black이다.(Red가 두 번 연속으로 나올 수 없다.)
    - 모든 리프 노드에서 루트 노드까지 가는 데 만나는 Black 노드의 수는 같다.
    - 새로운 노드는 항상 Red 노드로 삽입되는데, Red 노드가 두 번 연속으로 나오는 Double Red는 Restructuring / Recoloring 과정을 통해 해결된다.
        - Restructuring
            - 삼촌 노드가 Black인 경우
        - Recoloring
            - 삼촌 노드가 Red인 경우
    - C++에서 Set, Map은 Red Black Tree를 기반으로 만들어져 있다.