# 많이 사용하는 문제 해결 패턴들

- frequency counter
- multiple pointers
- sliding window
- divide and conquer
- recursion
- 외에도..
- dynamic programming
- greedy algorithms
- backtracking

## `1. frequency counter`

이 패턴은 오브젝트나 set을 사용하여 복수의 입력값들을 서로 비교하거나, 어떤 값을 저장하거나, 어떤 값이 나타난 빈도수를 저장하여 문제를 해결하는 패턴이다. \
주로 배열이나 문자열에 대한 for 중첩문, 또는 O(n\*n) 연산을 대신할 때 사용한다.

예-1) Write a function called same, which accepts two arrays. The function should return true if every value in the array has its corresponding value squared in the second array. The frequency of values must be the same. \
same([1, 2, 3], [4, 1, 9]) \
same([1, 2, 3], [1, 9]) \
same([1, 2, 1], [4, 4, 1])

```javascript
function same(arr1, arr2) {
  if (arr1.length !== arr2.length) return false;

  const frequencyCounter1 = {};
  const frequencyCounter2 = {};

  for (let elem of arr1) {
    frequencyCounter1[elem] = (frequencyCounter1[elem] || 0) + 1;
  }
  for (let elem of arr2) {
    frequencyCounter2[elem] = (frequencyCounter2[elem] || 0) + 1;
  }

  for (key in frequencyCounter1) {
    if (!(key ** 2 in frequencyCounter2)) {
      return false;
    }
    if (frequencyCounter2[key ** 2] !== frequencyCounter1[key]) {
      return false;
    }
  }

  return true;
}
```

예-2) Anagrams - given two strings, write a function to determine if the second string is an anagram of the first. An anagram is a word, phrase, or name formed by rearranging the letters of another, such as ‘cinema’, formed from ‘iceman’.

validAnagram(‘ ’, ‘ ’) //true \
validAnagram(‘aaz ’, ‘zza ’) //false \
validAnagram(‘anagram’, ‘nagagram’) //true \
validAnagram(‘rat’, ‘car’) //false \
validAnagram(‘awesom’, ‘awesome’) //false

```javascript
function validAnagram(str1, str2) {
  if (str1.length !== str2.length) return false;
  if (str1 === "" && str2 === "") return true;

  const objFromStr1 = {};

  for (let i = 0; i < str1.length; i++) {
    let letter = str1[i];
    objFromStr1[letter]
      ? (objFromStr1[letter] += 1)
      : (objFromStr1[letter] = 1);
  }

  for (let i = 0; i < str2.length; i++) {
    let letter = str2[i];

    if (objFromStr1[letter]) {
      objFromStr1[letter] -= 1;
    } else {
      // can't find letter of letter is zero then it's not an anagram.
      return false;
    }
  }
}
```

## `2. multiple pointers`

인덱스나 자리값(?)에 대응하는 '포인터'를 만들고 조건에 따라서 처음, 중간 또는 끝으로 움직여가는 패턴이다. \
⭐️ 최소한의 공간 복잡도로 문제를 해결할 수 있다.

예-1) write a function call sumZero which accepts a `sorted` array of integers. The function should find the `first` pair where the sum is 0. Return an array that includes both values that sum to zero or undefined if a pair does not exist. \
sumZero([-3, -2, -1, 0, 1, 2, 3]) // return [-3, 3] \
sumZero([-2, 0, 1, 3]) // undefined \
sumZero([1, 2, 3]) // undefined

```javascript
function sumZero(arr) {
  let left = 0;
  let right = arr.length - 1;

  while (left < right) {
    if (arr[left] + arr[right] === 0) {
      return [arr[left], arr[right]];
    } else if (arr[left] + arr[right] > 0) {
      right -= 1;
    } else {
      left += 1;
    }
  }
}
```

예-2) Implement a function called countUniqueValues, which accepts a `sorted` array, and counts the unique values in the array. There can be negative numbers in the array, but it will always be sorted. \
countUniqueValues([1, 1, 1, 1, 1, 2]) // 2 \
countUniqueValues([1, 2, 3, 4, 4, 4, 7, 7, 12, 12, 13]) // 7 \
countUniqueValues([]) // 0 \
countUniqueValues([-2, -1, -1, 0, 1]) // 4

```javascript
function countUniqueValues2(arr) {
  /*
  인자로 주어진 배열 1, 1, 1, 1, 3 이 있다. 

  먼저, 아래처럼 포인터 i, j를 둔다. 
  i
  1, 1, 1, 1, 3
     j

  만약 i와 j가 같으면 j를 오른쪽으로 한 칸 옮긴다. 아래처럼 된다. 
  i
  1, 1, 1, 1, 3
        j
  
  역시 i와 j가 같으므로 j를 오른쪽으로 한 칸 옮긴다. 
  i
  1, 1, 1, 1, 3
           j 
  
  역시 i와 j가 같으므로 또 j를 오른쪽으로 한 칸 옮긴다. 
  i
  1, 1, 1, 1, 3
              j   
              
  이제는 i와 j가 다르다. 이 경우, i를 오른쪽으로 한 칸 옮긴다.
  그런 다음, 새로운 i의 자리에 j가 가리키는 값을 넣는다. 
     i
  1, 1, 1, 1, 3
              j 
  
     i
  1, 3, 1, 1, 3
              j 

  */
  if (arr.length === 0) return 0;

  let a = 0;
  let b = 1;

  // 나는 아래처럼 했는데 사실 for문을 사용하면 더 간결하게 해결할 수 있다.
  while (b < arr.length) {
    if (arr[a] === arr[b]) {
      b += 1;
    } else {
      a += 1;
      arr[a] = arr[b];
    }
  }
  // for문으로. (b는 위에 while에서 사용하므로 여기에는 let c를 새로 만들었다.)
  for (let c = 1; c < arr.length; c++) {
    if (arr[a] !== arr[c]) {
      a += 1;
      arr[a] = arr[c];
    }
  }

  return a + 1;
}
```

## `3. sliding window`

특정 조건에 따라 ‘윈도우’가 커지기도 하고 아예 없앴다가 새로운 ‘윈도우’를 다시 만들기도 한다. \
⭐️ 배열이나 문자열의 부분 집합을 계속 따라가면서 특정 조건에 맞는 것을 찾아야하는 문제에서 유용한 방법이다.

예) Write a function called maxSubarraySum which accepts an array of integers and a number called n. The function should calculate the maximum sum of n consecutive elements in the array. \
maxSubarraySum([1, 2, 5, 2, 8, 1, 5], 2) // 10 \
maxSubarraySum([1, 2, 5, 2, 8, 1, 5], 4) // 17 \
maxSubarraySum([4, 2, 1, 6], 1) // 6 \
maxSubarraySum([4, 2, 1, 6, 2], 4) // 13 \
maxSubarraySum([], 4) // null

```javascript
function maxSubarraySum(arr, n) {
  /*
  n만큼 숫자들을 더한 뒤에, 맨 앞의 숫자는 빼고 맨 끝의 숫자는 새로 더하면 된다. 

  1, 2, 5, 2, 8, 1, 5
  예를 들어 n이 3이면, 맨 처음에 1 + 2 + 5를 한다. 
  그 다음 윈도우에서는 2 + 5 + 2를 해서 어느 합이 더 큰지 비교해야 하는데
  이 때 2 + 5는 이미 했으므로 반복해서 더할 필요가 없다.
  그래서 1 + 2 + 5의 합에다가 2를 새롭게 더하고 1을 빼면 된다.
  매번 이런 방법으로 새로운 합을 구한 뒤, 기존의 합과 비교하여 더 큰 값으로 갱신하면서 진행한다. 
  */
  let maxSum = 0;
  let tempSum;
  for (i = 0; i < n; i++) {
    maxSum += arr[i];
  }

  tempSum = maxSum;

  for (i = n; i < arr.length; i++) {
    tempSum += arr[i];
    tempSum -= arr[i - n];
    if (tempSum > maxSum) maxSum = tempSum;
    // 또는 maxSum = Math.max(tempSum, maxSum);
  }

  return maxSum;
}
```

## `4. divide and conquer`

주어진 데이터를 작은 조각으로 나눠서 조건에 맞게 처리하는 것. \
이 과정에서 소개될 6~7가지 정렬 알고리즘 중에서 quick sort와 merge sort가 divide and conquer의 대표 알고리즘이다. binary search 도 마찬가지. 잘 이용하면 시간 복잡도를 많이 줄일 수 있다. (logN)

예) Given a `sorted` array of integers, write a function called search, that accepts a value and returns the index where the value passed to the function is located. If the value is not found, return -1. \
search([1, 2, 3, 4, 5, 6], 4) // 3 \
search([1, 2, 3, 4, 5, 6], 6) // 5 \
search([1, 2, 3, 4, 5, 6], 11) // -1

```javascript
function search(arr, n) {
  let min = 0;
  let max = arr.length - 1;

  while (min <= max) {
    let middle = Math.floor((min + max) / 2);
    if (arr[middle] > n) {
      max = middle - 1;
    } else if (arr[middle] < n) {
      min = middle + 1;
    } else if (arr[middle] === n) {
      return middle;
    }
  }
  return -1;
}
```

## `5. recursion`

하나의 문제를 점점 작은 조각에 적용하면서 어느 끝에 다다를 때까지 계속 반복하면서 풀어나가는 패턴. 회귀 함수는 자기 자신을 호출하는 함수이다. 회귀 함수에 필요한 두 가지 필수 요소는 `base case(end point)`와 `differnt input(회귀 함수가 자기 자신을 호출할 때마다 다른 입력값이 들어가야함)`이다. 이 두 가지는 회귀 함수가 무한히 반복하는 것을 막아주는 요소다. (예: 팩토리얼) \

basically taking one problem and doing it over and over on a smaller piece or on a changing piece until you hit some end point which is called as ‘base case’

- 콜스택(call stack): 스택은 자료구조의 한 종류이다. 함수가 호출되면 콜스택의 맨 위에 위치한다. return 키워드를 만나거나 함수가 끝나면 컴파일러가 콜스택의 맨 위에서 그 함수를 지운다. (pop) 다만 회귀 함수는 end point를 만나기 전까지 콜스택에 새로운 함수를 계속 올린다.
- recursion을 활용한 디자인 패턴 2가지

  - helper method recursion: 외부 함수(이것은 회귀 함수가 아님) 내에 회귀 함수를 정의하고 호출하는 것. 배열이나 데이터의 목록을 처리해야 할 때 사용할 수 있다.

    ```javascript
    // 배열에서 홀수 숫자 찾아서 새 배열에 넣기
    function collectOddNum(arr) {
      const oddNum = [];

      function helper(helperInput) {
        if (helperInput.length === 0) return;

        if (helperInput[0] % 2 !== 0) {
          oddNum.push(helperInput[0]);
        }

        helper(helperInput.slice(1));
      }

      helper(arr);

      return oddNum;
    }
    ```

  - pure recursion

    ```javascript
    function collectOddValues2(arr) {
      let newArr = []; // 함수를 호출할 때마다 새로운 배열을 선언.

      if (arr.length === 0) return newArr;

      if (arr[0] % 2 !== 0) newArr.push(arr[0]);

      newArr = newArr.concat(collectOddValues2(arr.slice(1)));
      return newArr;
    }
    ```
