---
title: "[Hackerrank] Mark and Toys"
date: 2020-11-08T17:25:39+09:00
tags: [Algorithm, Hackerrank]
draft: false
---
### 문제
`int` 타입의 배열 `prices` 에는 다양한 가격의 장난감들이 있는데 이 장난감들 중 마크가 가진 재산 내에서 살 수 있는 최대 갯수의 장난감을 구하는 문제이다.
input:
```
7 50			      7: 장난감이 들어있는 배열의 길이, 50: 마크의 재산
1 12 5 111 200 1000 10        각 장난감의 가격
```
output:
```
4	마크가 가진 재산으로 살 수 있는 최대 장난감의 개수
```
### 결과
```java
static int maximumToys(int[] prices, int k) {
    int result = 0;
    int count = 0;
    Arrays.sort(prices);
    for (int price : prices) {
    	if (price <= k) {
        	result += price;
        if (result > k) break;
        	count++;
       	}
   }
   return count;
}
```
### 풀이
`result` 는 각 장난감 가격의 합을 담을 변수이고, `count` 는 구입 가능한 장난감의 개수를 담는다.
우선 `prices` 배열에서 최대한 작은 값의 장난감들을 구입해, 마크가 가진 재산에 맞춰야 하기 때문에 배열을 `sort` 메소드로 정렬 해 주었다. 
```java
int result = 0;
int count = 0;
Arrays.sort(prices)
```
그리고 `for each` 문을 사용해 `prices` 배열에 담긴 장남감의 가격을 하나씩 마크의 재산인 `k` 와 비교하고 장난감의 가격이 `k` 보다 작거나 같을 경우에는 `result` 값에 장난감의 가격을 계속 더해 나간다.
```java
for (int price : prices) {
    if (price <= k) {
    	result += price;
    	// ...
```
계속해서 장난감의 가격을 더해 나가다가 `result` 의 값이 `k` 보다 크게 되면 더 이상 장난감을 구입할 수 없기 때문에 `break` 를 걸어 반복문을 빠져 나간다.
그리고 이 조건문에서 `break` 가 걸리지 않으면 `count++` 로 살 수 있는 장난감의 개수를 추가한다. 반복문을 반복하다가 모든 작업이 끝나면 `count` 를 반환한다.
```java
for (int price : prices) {
    if (price <= k) {
    	result += price;
    	if (result > k) break;
        	count++;
       	}
   }
   return count;
}
```

> 출처:
[Mark and Toys | HackerRank](https://www.hackerrank.com/challenges/mark-and-toys/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=sorting)
