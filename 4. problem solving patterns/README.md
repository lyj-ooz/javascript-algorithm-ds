# 많이 사용하는 문제 해결 패턴들

- frequency counter
- multiple pointers
- sliding window
- divide and conquer
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
