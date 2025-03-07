---
layout: post
title: 백준 14600-동물원
category: /baekjoon/14600
date: 2021-07-15 15:39 +0800
---

# **문제의 핵심**
- 경우의 수에 해당하는 규칙찾기

# **문제 설명**
- 대답한 동물에 대하여 토끼 혹은 고양이일 모든 경우의수 구하기
    - 입력 1:

            5
            0 0 1 1 2 
            # 자신보다 큰동물이 0인것2개 자신보다 작은 동물이 1인것 2개 자신보다 큰동물이 2인것 1개
    - 가능한 경우의수

        |count|0|0|1|1|2|
        |-|-|-|-|-|-|
        |0|토|고|토|고|토|
        |1|토|고|토|고|고|
        |2|토|고|고|토|토|
        |3|토|고|고|토|고|
        |4|고|토|토|고|토|
        |5|고|토|토|고|고|
        |6|고|토|고|토|토|
        |7|고|토|고|토|고|


    - 입력 2:

            5
            0 1 2 3 4
    - 가능한 경우의수


        |count|0|1|2|3|4|
        |-|-|-|-|-|-|
        |0|토|토|토|토|토|
        |1|고|고|고|고|고|
        

# **경우의수 규칙**
- 줄을 하나씩 세워본다면 다음과 같은 규칙을 찾을 수 있다.
    1. **숫자는 연속되어야한다.**
        - 같은 키가 없으므로
        - 정렬된것을 가정했을때
            ```
                data[i]: i번째 동물이 말한 '자기보다 큰 동물의수'
                data[i] == data[i-1] + 1 or
                        == data[i-1]
            ```
        - 2마리 있을때 5, 8이면 자기보다 큰숫자는 무조건 1개가 들어와야하는데,\
        자기보다 큰 숫자가 5이거나 8 이면 말이안됨
    2. **같은 숫자는 최대 2개** 
        - 고양이/ 토끼 두종류밖에 없으므로
        - 따라서 둥물이 숫자를 말하는 횟수가 1번, 2번으로 제한되어야함
    3. **키가 큰 동물의 수는 키가 작은 동물의 수보다 무조건 같거나 많아야한다.**
        - 작은숫자를 말한 동물의 수(키가 큰 동물의 수)가\
          큰 숫자를 말한 동물의수(키작은 큰동물의 수)보다\
          같거나 많아야한다.
        - ex) 1 0 1
            - 1을 말한 동물은 자기보다 큰 동물이 각각 1마리씩 있어야하는데 제일 큰 동물(0)은 1마리 밖에 없으므로 조건에 맞는 경우의 수 세는것이 불가능하다

# **알고리즘**
1. **입력받음** 
    - data[i]: i번째 동물이 말한 자기보다 큰 같은 종의 동물수
2. **입력받은것의 데이터 변형**
    - data[i]: 
        - i: 자기보다 큰 동물의수
        - data[i]: 자기보다 큰 동물의수의 개수 
            - data[2]=2: 자기보다 큰 동물의수가 2마리인 동물의 마리수가 2마리임(토끼, 고양이모두해당됨)
            - data[2]=1: 자기보다 큰 동물의수가 2마리인 동물의 수가 1개임 (토끼 혹은 고양이)
    - 규칙 따져서 계산하기
3. **숫자 계산**
    - 2개인경우: 토끼, 고양이 2가지 경우가 올 수 있음
    - 1개인경우: 토끼 혹은 고양이가 될수있음
    - 따라서 2인경우는 2의 개수만큼 2를 곱해주고 1인경우가 존재하면 2를 곱해줌
    - ex) 


        |0|1|2|result|설명|
        |-|-|-|-|-|
        |2|1|1|2 * 2 = 4|(토 and 고)x(토or고)|
        |2|2|1|2 * 2 * 2 = 8|(토 and 고)x(토 and 고) * (토 or 고)|
        |2|2|2|2 * 2 * 2 * 2 = 16|(토 and 고)x(토 and 고)x(토 and 고)x(토 and 고)|


# **복기할것**
- 이해하면 어렵지 않은문제
- 이해하기까지 오래걸리는 문제


<details>
<summary>전체 코드</summary>
<div markdown="1">

# **전체 코드**
```python
import sys
input = sys.stdin.readline
def main():
    N = int(input().rstrip())
    tall_info = sorted(list(map(int, input().rstrip().split(" "))))
    data = [0] * N
    prev_idx = -1
    for idx, value in enumerate(tall_info):
        if idx == 0 and value > 0:
            return 0
        if prev_idx != -1 and abs(tall_info[prev_idx]-tall_info[idx]) > 1:
            return 0
        data[value] += 1
        if data[value] > 2:
            return 0
        prev_idx = idx
    prev_value = -1
    count_two, count_one = 0, 0 

    for value in data:
        if value == 0:
            break
        if prev_value != -1 and value > prev_value:
            return 0
        if value == 2:
            count_two += 1
        if value == 1:
            count_one += 1
        prev_value = value
    multiple = 1 if count_one == 0 else 2
    return 2**count_two * multiple

if __name__ == "__main__":
    print(main())
```

</div>
</details>
