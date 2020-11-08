---
title: "[Algorithm] Repeated String"
date: 2020-11-08T15:39:57+09:00
tags: [Algorithm, Hackerrank]
draft: false
---
문자열 `s` 와 문자열을 반복적으로 채워넣을 인덱스 수 `n` 이 주어졌을 때, 문자 `a` 의 개수를 찾는 문제다.

예를 들어, 문자열 `s = "abcac"` 가 주어지고, 이 문자열을 반복할 개수  `n = 10` 이 주어진다면, `n` 만큼의 인덱스에 반복해서 문자를 채우면  `"abcacabcac"` 가 된다. 여기서 문자 `a` 의 개수는 **4**개가 된다. 

### 결과
```java
public long solution(String s, long n) {

        long x = n / s.length();
        long r = n % s.length();
        long count = 0;

        for (int i = 0; i < s.length(); i++) {
             if (s.charAt(i) == 'a') {
                  count++;
             }
        }
        count *= x;
        if (r != 0) {
            for (int i = 0; i < r; i++) {
               if (s.charAt(i) == 'a') {
                    count++;
               }
           }
     }
     
   return count;
}
```

### 풀이

문자열 `s = "abcac"` 를  `n = 10` 에 반복해서 채워 넣으면 다음과 같다.
![img1](/images/2020/11/hackerrank1.png)
n 만큼의 인덱스에 문자열을 반복해서 넣으면 주어진 문자열 s가 2세트로 채워진다.
하지만 만약 문자열 `s = "aba"` 를 `n=10` 에 채워 넣는다면..?
![img2](/images/2020/11/hackerrank2.png)

총 3세트가 만들어지고 마지막 인덱스가 애매하게 남아버린다. 마지막 인덱스까지 빈 인덱스가 없도록 확인해야한다.

따라서, 총 생겨날 세트의 개수와 남게될 인덱스의 개수를 구했다. 
```java
long x = n / s.length(); // 총 세트
long r = n % s.length(); // 남게 될 인덱스 개수
```
그리고 문자열 길이만큼 순환하면서 문자  `a` 를 찾고 찾을 때마다 `count++` 한다.
```java
for (int i = 0; i < s.length(); i++) {
     if (s.charAt(i) == 'a') {
         count++;
    }
}
```

그리고 `count` 를 이전에 구해 둔 총 생겨날 세트의 개수만큼 곱해준다. 각 세트마다 문자열 `s` 와 동일한 `a` 의 개수가 있기 때문이다.
```java
count *= x;
```
마지막으로 세트가 생기고 남게 될 인덱스의 개수가 `0`이 아니라면 (세트가 인덱스에 딱 맞게 떨어지지 않는다면) 남게되는 인덱스 만큼 순환하며 문자 `a` 가 있는지 확인하고 있으면 `count` 를 추가해준다.
```java
if (r != 0) {
     for (int i = 0; i < r; i++) {
         if (s.charAt(i) == 'a') {
            count++;
        }
    }
}
```

### Test case

여러차례의 시도 중 테스트 케이스에서 런타임 에러가 발생하면서 통과하지 못한 케이스가 있었는데 그 때 테스트 케이스는 다음과 같았다.
```java
s = "kmretasscityylpdhuwjirnqimlkcgxubxmsxpypgzxtenweirknjtasxtvxemtwxuarabssvqdnktqadhyktagjxoanknhgilnm"
n = 736778906400L;
```

**out put**
```markdown
51574523448
```
   
처음에는 단순하게 문자열을 n 만큼 순환하며 a 를 찾으려고 했는데 문자열이 n 과 동일하지 않다보니 out of index 에러를 방지하려고 이렇게 저렇게 시도 해봤지만 결국 위와 같은 방법으로 해결할 수 있었다. 알고리즘은 반복되는 규칙을 찾는 것이 중요하다는 사실을 다시 깨달았다.
   
   
> **출처:**
[Repeated String | HackerRank](https://www.hackerrank.com/challenges/repeated-string/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=warmup)
