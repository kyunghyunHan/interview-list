## hsahmap

- HashMap<K, V> 타입은 K 타입의 키에 V 타입의 값을 매핑한 것을 저장
- 매핑은 해쉬 함수(hashing function) 을 통해 동작하는데, 해쉬 함수는 이 키와 값을 메모리 어디에 저장할지 결정
- 해쉬맵은 여러분이 벡터를 이용하듯 인덱스를 이용하는 것이 아니라 임의의 타입으로 된 키를 이용하여 데이터를 찾기를 원할때 유용

## 새로운 해쉬맵 생성하기

```rs

use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

```rs
use std::collections::HashMap;

let teams  = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];

let scores: HashMap<_, _> = teams.iter().zip(initial_scores.iter()).collect();

```

- 벡터와 마찬가지로, 해쉬맵도 데이터를 힙에 저장합니다. 이 HashMap은 String 타입의 키와 i32 타입의 값을 갖습니다. 벡터와 비슷하게 해쉬맵도 동질적입니다: 모든 키는 같은 타입이어야 하고, 모든 값도 같은 타입이여야 합니다.

## 해쉬맵과 소유권

- i32와 같이 Copy 트레잇을 구현한 타입에 대하여, 그 값들은 해쉬맵 안으로 복사됩니다. String과 같이 소유된 값들에 대해서는, 아래의 Listing 8-22와 같이 값들이 이동되어 해쉬맵이 그 값들에 대한 소유자가 될 것입니다:

```rs
use std::collections::HashMap;

let field_name = String::from("Favorite color");
let field_value = String::from("Blue");

let mut map = HashMap::new();
map.insert(field_name, field_value);
// field_name과 field_value은 이 지점부터 유효하지 않습니다.
// 이들을 이용하는 시도를 해보고 어떤 컴파일러 에러가 나오는지 보세요!
```

- insert를 호출하여 field_name과 field_value를 해쉬맵으로 이동시킨 후에는 더 이상 이 둘을 사용할 수 없습니다.

## 해쉬맵 내의 값 접근하기

```rs
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

let team_name = String::from("Blue");
let score = scores.get(&team_name);
```

```rs
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

## 해쉬맵 갱신하기

```rs
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);

println!("{:?}", scores);
```

## 키에 할당된 값이 없을 경우에만 삽입하기

```rs
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);

scores.entry(String::from("Yellow")).or_insert(50);
scores.entry(String::from("Blue")).or_insert(50);

println!("{:?}", scores);
```

## 예전 값을 기초로 값을 갱신하기

```rs

use std::collections::HashMap;

let text = "hello world wonderful world";

let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}

println!("{:?}", map);
```

## 해쉬 함수

## 정리

```
정수 리스트가 주어졌을 때, 벡터를 이용하여 이 리스트의 평균값(mean, average), 중간값(median, 정렬했을 때 가장 가운데 위치한 값), 그리고 최빈값(mode, 가장 많이 발생한 값; 해쉬맵이 여기서 도움이 될 것입니다)를 반환해보세요.
스트링을 피그 라틴(pig Latin)으로 변경해보세요. 각 단어의 첫번째 자음은 단어의 끝으로 이동하고 “ay”를 붙이므로, “first”는 “irst-fay”가 됩니다. 모음으로 시작하는 단어는 대신 끝에 “hay”를 붙입니다. (“apple”은 “apple-hay”가 됩니다.) UTF-8 인코딩에 대해 기억하세요!
해쉬맵과 벡터를 이용하여, 사용자가 회사 내의 부서에 대한 피고용인 이름을 추가할 수 있도록 하는 텍스트 인터페이스를 만들어보세요. 예를들어 “Add Sally to Engineering”이나 “Add Amir to Sales” 같은 식으로요. 그후 사용자가 각 부서의 모든 사람들에 대한 리스트나 알파벳 순으로 정렬된 부서별 모든 사람에 대한 리스트를 조회할 수 있도록 해보세요.
```
