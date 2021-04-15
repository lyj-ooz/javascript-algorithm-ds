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
