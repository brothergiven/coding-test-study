# Week 1

## 문제 1. 250134 수레 움직이기

📌 링크 : https://school.programmers.co.kr/learn/courses/30/lessons/250134

### 문제 풀이 방법
자료구조 시간 때 배웠던 미로찾기와 같이 각 방향으로의 이동을 나타내는 `dx`,`dx`를 나타내는 배열을 선언하고 이를 활용하여 각 방향을 확인해보며 이동하는 경우의 수를 고려하여 풀이를 작성하였다.
   
방문했던 위치를 기록하여 이를 바탕으로 갈 수 있는 모든 방향을 dfs를 통해 체크한다.   
Red와 Blue를 나타내는 Point 클래스를 생성하고 이 객체의 위치를 통하여 각 포인트가 도착한 지 체크한다. 그렇지 않을 경우, 도착한 포인트의 경우를 나누어 이동 가능 여부를 체크하여 다음 포인트로 이동한다.   

dfs가 다음 함수를 호출할 때 `depth+1`을 하여 해당 깊이(실행 횟수)를 체크한다.

**풀면서 막혔던 부분**   
초기에는 테스트케이스 3,4,13,14번을 통과하지 못하였다.
해당 코드에서는 `canMove()` 함수를 통해서 포인트의 이동 가능 여부를 체크한다.   
Red와 Blue가 둘 다 End에 도착하지 않았을 경우 현재 Red(Blue)와 nextBlue(nextRed)의 위치가 동일하면 `false`를 리턴하도록 하여 해당 경우가 통과되지 않도록 설계하였다.

![image](https://github.com/hky035/coding-test-study/assets/128910345/5ecbc8f3-937f-409c-9649-0735616330a2)

그러나 예를 들어 다음 그림과 같이 이동하는 경우에도 `canMove()`에서도 `false`가 리턴하기 때문에 문제가 발생하였다. 다음 부분을 고려하기 위해 `Point` 클래스에 `equals()` 메서드를 오버라이드하여 
```
if(!(red.equals(nextBlue) && blue.equals(nextRed)) && ... }
```
로 해당 경우를 정확히 체크할 수 있었다.

### 실행결과
![image](https://github.com/hky035/coding-test-study/assets/128910345/6a7826b3-5fe8-4863-a553-dffbfb4dbaa2)


## 문제 2. 42860 조이스틱
📌 링크 : https://school.programmers.co.kr/learn/courses/30/lessons/42860

### 문제 풀이 방법

해당 문제는 예전에 어디선가 본 문제와 유사한 문제였다.   
우선, 일반적으로는 좌우로 한 칸씩 이동할 때마다 상하를 변경하는 것이 인간적인 사고지만, 사실 상하-좌우를 각각 분리해서 계산하는 것이 코드의 간결성이 높아진다.   

따라서, 상하 방향 이동 횟수와 좌우 방향 이동 횟수를 나눠서 구하였다.   
상하 방향 이동 횟수는 방향키를 위로 이동하는 경우 vs 아래로 이동하는 경우 중 최소값으로 정하였다.   
좌우 방향 이동 횟수는 문자 A를 만났을 때 스킵을 하는 경우와 그대로 가는 경우 중 최소값으로 정하여야 한다. 또한, 오른쪽으로 가는 값이 최소여서 오른쪽으로 가다가 A를 만나게 되어 다시 왼쪽으로 되돌아가는 것이 최소값이 되는 경우가 존재하며 이 반대의 경우도 존재한다.

**풀면서 막혔던 부분**   
따라서, 좌 ➡ 우로 가거나, 좌 ⬅ 우로 가며 더 짧은 경로로 이동하는 것이 중요하다.   
매번 이동할 때마다 최소값을 고려해야한다. 처음에는 왼쪽으로 갈 때, 맨 뒤의 문자로 가버리게 되었을 때 해당 문자는 방문한 것이기 때문에 다시 방문할 필요가 없지만 해당 경우를 생각하지 못하고 A의 다음 문자로 온 뒤 다시 오른쪽으로 계속해서 이동하는 횟수까지 더하여 오류가 많이 발생하였다.

### 풀면서 들었던 의문사항

예전에 해당 문제를 보았을 때 좌우이동 최솟값 비교식이 많이 이해가 되지 않았다.   
다른 팀원의 코드와 비슷한 방식이라 해당 방법을 더 쉽게 이해하고, 더 나은 방법이 있는 지 생각해보고 싶다.   

### 실행결과
![image](https://github.com/hky035/coding-test-study/assets/128910345/d332d2df-27f3-4e93-b92a-aa8c7a6e6e50)


## 문제 3. 49191 순위
📌 링크 : https://school.programmers.co.kr/learn/courses/30/lessons/49191

### 문제 풀이 방법
각 권투선수의 이기고 진 정보가 `results` 배열에 저장되어 있다. 모든 정보는 기록되어 있지 않기 때문에 해당 기록을 바탕으로 특정 선수를 대상으로 이긴 선수와 진 선수를 나눠서 푸는 방식으로 풀이법을 정하였다.

**풀이하며 막혔던 부분**   
처음에는 재귀함수를 들어간 뒤 계산 winner와 loser 그룹이 나눠지게 되면 winner 측에서 loser 열을 3으로 설정하고, loser도 이와 대칭되도록 설정하여 풀이를 작성했었다.   
   
그러나, 해당 방법 진행 시 나머지 노드들간의 이기고 진 정보를 업데이트하는 데 문제가 발생하여 재귀함수로 진입하기 전 이기고 진 정보를 우선 기록해야한다는 것을 알았다.    

해당 방법을 어떻게 구현할 지 고민해보았는데 제일 처음 그래프 형태로 생각 하였을 때 `A ➡ B && B ➡ C 면 A ➡ C` 라는 것이 떠올라 해당 방법을 적용하여 구현하였다. README를 작성하기 전 다른 스터디원의 PR을 보았을 때 해당 방법이 floyd warshall 방법이라는 것을 다시 떠올리게 되었다.

### 의문사항
- 처음에는 그래프 형태로 생각했었다. `Node` 클래스를 만들어 `in`/`out`\(또는 `win`/`lose`\) 멤버 필드를 선언하여 이를 바탕으로 계산을 하는 것이 더 효율적인지 궁금하다.   
해당 방법을 진행하지 않은 이유는 노드를 만들고 각 노드의 `in`과 `out`을 계속해서 수정할 시 메모리 상의 데이터 저장 공간을 계속해서 이동해야할 소요가 생겨 시간이 더 늘어날 것이라 생각하였기 때문이다.
- 적절한 함수 및 변수명 네이밍을 리뷰 때 고민해보면 좋을 것 같다.

### 실행결과 
![image](https://github.com/hky035/coding-test-study/assets/128910345/c03fda88-ee58-4030-a234-932134498ee2)


