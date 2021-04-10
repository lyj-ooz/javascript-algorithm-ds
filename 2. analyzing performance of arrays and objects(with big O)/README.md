# big O로 array와 object의 퍼포먼스 분석하기

## 배열과 big O

- 값 넣기:
  - 배열의 맨 앞에 값을 넣으면 O(n)
  - 배열의 맨 끝에 값을 넣으면 O(1)
- 값 삭제:
  - 배열의 맨 앞에서 값을 삭제하면 O(n)
  - 배열의 맨 끝에서 값을 삭제하면 O(1)
- 특정 값을 찾기: O(n)
- 어떤 값에 접근하기: O(1)

### 배열 메소드들의 big O

- .concat(): O(n)
- .slice(): O(n)
- .splice(): O(n)
- .sort(): O(n\*log n)
- .forEach/map/filter/reduce..: O(n)

=> 순서가 필요하다면 array가 있지만, linked list 같은 것도 고려할 수 있음..

#

## 오브젝트와 big O

- 값 넣기: O(1)
- 값 삭제: O(1)
- 특정 값을 찾기: O(n)
- 어떤 값에 접근하기: O(1)

### 오브젝트 메소드들의 big O

- Object.keys(): O(n)
- Object.values(): O(n)
- Object.entries(): O(n)
- Object.hasOwnProperty(): O(1)

=> 순서가 딱히 필요 없다면 오브젝트를 선택하는 것도 좋은 방법..!
