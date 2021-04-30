# Searching Algorithms

## `1. 선형 검색 linear search`

배열의 모든 원소를 하나씩 살펴보는 것이다. 시간 복잡도는 best case가 O(1), worse case는 O(n)이다. \
자바스크립트 자체에도 indexOf, includes, find, findIndex 같은 검색 메소드가 있으며, 이것들도 모든 원소들을 다 살펴보는 linear search 방식이다.

```javascript
function linearSearch(arr, n) {
  return arr.indexOf(n);
}
```

## `2. 이진 검색 binary search`

한 번에 하나씩 보는게 아니라 절반씩 나눠가며 찾아본다. (divide and conquer) \
시간 복잡도는 best case가 O(1), worse case는 O(log n)이다. 주의할 점은 반드시 `정렬된` 배열에다가 해야 제대로 찾을 수 있다는 것!!

```javascript
function binarySearch(arr, n) {
  let left = 0;
  let right = arr.length - 1;
  let middle = Math.floor((left + right) / 2);

  while (left <= right) {
    if (arr[middle] === n) {
      return middle;
    } else if (arr[middle] > n) {
      right = middle - 1;
    } else if (arr[middle] < n) {
      left = middle + 1;
    }

    middle = Math.floor((left + right) / 2);
  }

  return -1;
}
```

## `3. 문자열 검색`

1. naive 버전: 두 가지 문자열을 한 글자씩 비교하는 것.

```javascript
// for문으로 구현하는게 잘 안돼서 while문으로 구현했다.
function naiveStringSearch(long, short) {
  let i = 0;
  let j = 0;
  let count = 0;

  while (i < long.length) {
    if (long[i] === short[j]) {
      j += 1;
      if (j === short.length) {
        j = 0;
        count += 1;
      }
    } else {
      j = 0;
    }
    i += 1;
  }

  return count;
}

// for문으로 구현.
function naiveStringSearch(l, s) {
  let count = 0;

  for (let i = 0; i < l.length; i++) {
    for (let j = 0; j < s.length; j++) {
      //⭐️⭐️
      if (s[j] !== l[i + j]) {
        break;
      }
      if (j === s.length - 1) {
        count += 1;
      }
    }
  }
  return count;
}
```

2. KMP
   - KMP는 이미 비교한 문자들을 처음부터 다시 하나씩 비교하는 것을 방지하는 것으로 부터 시작한 알고리즘인 것 같다. 지금 내 실력에 이해하는 것은 조금 어려운 것 같아 나중에 다시 공부할 목적으로 참고 링크를 남긴다.
     https://youtu.be/UcjK_k5PLHI?t=225
