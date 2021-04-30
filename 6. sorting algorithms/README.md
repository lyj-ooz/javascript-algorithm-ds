# Sorting Algorithms

데이터들을 특정 순서로 재배열하는 과정을 정렬이라고 한다. 일단 여기서는 배열에서의 sorting만 먼저 알아보고 추후에 linked list나 tree에서의 sorting도 살펴볼 것이다. \

- 정렬의 예: 숫자들을 크기 순으로 정렬하기, 문자열을 알파벳 순으로 정렬하기, 영화를 개봉일자나 흥행 순서로 정렬하기..

정렬 알고리즘의 종류는 다양하며 각각의 장단점이 있다. 어떤 형태의 데이터를 정렬하느냐에 따라 여기에선 처리 속도가 빨랐던 알고리즘이 저기에선 처리 속도가 느리기도 한다. \
(참고 https://www.toptal.com/developers/sorting-algorithms )

- bubble sort 버블 정렬
- selection sort 선택 정렬
- insertion sort 삽입 정렬
- merge sort 병합 정렬
- quick sort 퀵 정렬
- radix sort

#

## `1. bubble sort 버블 정렬 🧼`

- 시간 복잡도 O(n\*n)

현재 요소와 다음에 오는 요소를 비교하여 더 큰 것을 스왑하는 방식을 반복하여 정렬한다. (참고 https://visualgo.net/en/sorting)

- 자바스크립트에서 swap하는 방법 2가지
  ```javascript
  //es5
  var temp = arr[idx1];
  arr[idx1] = arr[idx2];
  arr[idx2] = temp;
  //es6 ~
  [arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]];
  ```

버블 정렬은 처리 속도가 그렇게 빠른 알고리즘은 아니다. 하지만 처리하는 데이터가 `이미 거의 정렬된` 데이터라면, 스왑 여부를 확인하여 성능을 좀 더 개선할 수 있다. `거의 정렬된` 데이터라면 스왑이 일어나지 않는 횟수가 많을 것이다. 이미 정렬된 것이 많기 때문이다. 스왑이 일어나지 않을 것이라면 loop를 돌 이유로 없다. 그래서 아래의 코드와 같이 noSwap 변수를 추가하여 구현할 수 있겠다. 여기서의 시간 복잡도는 "best case"일 때 O(n)이다.

```javascript
function bubbleSort(arr) {
  let noSwap;

  for (let i = arr.length; i > 0; i--) {
    noSwap = true;
    for (let j = 0; j < i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
        noSwap = false;
      }
    }

    if (noSwap) break;
  }

  return arr;
}
```

## `2. selection sort 선택 정렬 🤏`
