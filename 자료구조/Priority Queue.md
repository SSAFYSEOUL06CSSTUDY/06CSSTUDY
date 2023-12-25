# 목차

1. [**우선순위 큐(Priority Queue)의 개념**](#1-우선순위-큐priority-queue의-개념)
2. [**우선순위 큐의 구현**](#2-우선순위-큐의-구현)
3. [**힙(Heap)의 개념**](#3-힙heap의-개념)
4. [**Java에서의 적용**](#4-java에서의-적용)
5. [**참고자료**](#5-참고자료)

## 1. 우선순위 큐(Priority Queue)의 개념

> 우선순위가 높은 데이터를 가장 먼저 삭제하는 자료구조

- 데이터를 우선순위에 따라 처리하고 싶을 때 사용
- 가격이 높은 순, 숫자가 큰 순 등으로 자료구조에서 꺼내어 사용하는 방식

<table>
  <tr>
    <td>자료구조</td>
    <td>우선 삭제되는 요소</td>
  </tr>
  <tr>
    <td>스택</td>
    <td>가장 최근에 들어온 데이터</td>
  </tr>
  <tr>
    <td>큐</td>
    <td>가장 먼저 들어온 데이터</td>
  </tr>
  <tr>
    <td>우선순위 큐</td>
    <td>가장 우선순위가 높은 데이터</td>
  </tr>
</table>

## 2. 우선순위 큐의 구현

> 우선순위 큐는 배열,리스트 와 힙(Heap)을 이용하여 구현 가능

데이터의 개수가 N개일 때, 구현 방식에 따라 시간 복잡도 비교

<table>
  <tr>
    <td>구현 방식</td>
    <td>삽입 시간</td>
    <td>삭제 시간</td>
  </tr>
  <tr>
    <td>순서 없는 리스트(배열)</td>
    <td>O(1)</td>
    <td>O(N)</td>
  </tr>
  <tr>
    <td>정렬된 리스트(배열)</td>
    <td>O(N)</td>
    <td>O(1)</td>
  </tr>
  <tr>
    <td>힙</td>
    <td>O(logN)</td>
    <td>O(logN)</td>
  </tr>
</table>

> 기본적으로 Java 등의 언어에서 제공되는 우선순위 큐 라이브러리는 힙을 기반으로 작동 => 삽입, 삭제 NlogN

## 3. 힙(Heap)의 개념

### 힙이란?

- 가장 큰 값이나 가장 작은 값을 빠르게 찾아내도록 만들어진 자료 구조
- 부모 노드의 우선 순위가 자식 노드의 우선순위보다 항상 크거나 같은(최대 힙 - Max Heap), 혹은 작거나 같은(최소 힙 - Min Heap) 완전이진트리
- 부모와 자식의 우선순위가 같을 수 있다 => 데이터 중복 가능
  [![heap1.png](https://i.postimg.cc/Z52vVGYj/heap1.png)](https://postimg.cc/yDhNNrZS)

### 배열의 인덱스로 구조 이해

- 왼쪽 자식 인덱스 = 부모 인덱스 \* 2
- 오른쪽 자식 인덱스 = (부모 인덱스 \* 2) + 1
- 부모 인덱스 = 자식 인덱스 / 2
  [![heap2.png](https://i.postimg.cc/t43GMPpf/heap2.png)](https://postimg.cc/K18WK1Kr)

### 삽입 연산 - add, offer

[![heap6.gif](https://i.postimg.cc/qBwLjvPL/heap6.gif)](https://postimg.cc/mhcCrB2z)

    1단계) 삽입 데이터는 가장 마지막 노드에 삽입
    2단계) 삽입 데이터를 부모 노드와 우선 순위를 비교하여 자리 교체
    3단계) 최대 O(logN)의 시간복잡도를 소요

### 삭제 연산 - poll, remove

[![heap5.gif](https://i.postimg.cc/prWBRwFz/heap5.gif)](https://postimg.cc/qhFKGZQv)

    1단계) 루트 노드의 데이터 삭제
    2단계) 마지막 노드의 데이터를 루트 노드로 가져온다
    3단계) 최상단 부모노드 부터 자식 노드와 우선순위를 비교해 정렬

## 4. Java에서의 적용

### 적용

```
[Min Heap - 숫자가 낮을 수록 우선순위 높음]
PriorityQueue<Integer> pq = new PriorityQueue<>();

[Max Heap - 숫자가 높을 수록 우선순위 높음]
PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());

[람다식 적용]
PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[0] - o2[0]);

[Comparator]
PriorityQueue<int[]> pq = new PriorityQueue<>(new Comparator<int[]>() {
    @Override
    public int compare(int[] o1, int[] o2) {
        return o1[0] - o2[0];
    };
});

```

### compareTo

<details>
<summary>실생활 적용 예시</summary>

```
class Vehicle implements Comparable<Vehicle>{
    private String name;
    private int time;

    public Vehicle(String name, int time) {
        this.name = name;
        this.time = time;
    }
    public String getName() {
        return this.name;
    }
    public int getTime() {
        return this.time;
    }

    @Override
    public int compareTo(Vehicle target) {
        if(this.time < target.getTime()) return -1;
        else if(this.time > target.getTime()) return 1;
        return 0;
    }
}

```

</details>

<details>
<summary>적용 결과</summary>

```
import java.util.PriorityQueue;

public class Priority_Queue {

    public static void main(String[] args) {
        PriorityQueue<Vehicle> pQueue = new PriorityQueue<Vehicle>();

        pQueue.offer(new Vehicle("대중교통", 70));
        pQueue.offer(new Vehicle("자가용", 45));
        pQueue.offer(new Vehicle("도보", 400));
        pQueue.offer(new Vehicle("자전거", 125));

        while(!pQueue.isEmpty()) {
            Vehicle v = pQueue.poll();
            System.out.println(v.getName() + " 시간 :" + v.getTime());
        }
    }

    [출력 결과]
    자가용 시간 :45
    대중교통 시간 :70
    자전거 시간 :125
    도보 시간 :400
}


```

</details>

## 5. 참고자료

동빈나 - 자료구조: 우선순위 큐(Priority Queue)와 힙(Heap) 10분 핵심 요약<br/>
https://www.youtube.com/watch?v=AjFlp951nz0

Java Priority Queue(우선순위 큐) 원리 및 사용 방법<br/>
https://haservi.github.io/posts/algorithms/priority-queue/
