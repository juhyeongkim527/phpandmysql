# php 태그

- `<?php>` 로 시작하여 `?>`로 닫는다.

- `echo` 명령어 뒤에 묶인 텍스트 또는 HTML 태그를 브라우저로 전달할 수 있다.
- `echo` 태그는 `'` 또는 `"` 로 시작해야하고, 여는 따옴표를 닫는 따옴표 전에 사용하려면 escaping을 해줘야 한다.
- `"`을 쓰면 php 인터프리터는 텍스트에 변수명이 포함되어있는지 확인하고, `'`를 쓰면 확인하지 않는다.

# 주석

- `<?php ~~ ?>` 태그 안에 작성할 수 있다.
- C와 비슷하게 한 줄은 `//`, `#`을 사용하고 여러 줄은 `/*`으로 열어서 `*/`으로 닫는다.
- 주석은 당연히 php 인터프리터에게도 해석되지 않고, HTML 페이지를 만드는데 사용되지 않기 때문에 브라우저에서는 나타나지 않는다.

# 변수

- 변수명은 `$` 기호로 시작해야한다.
- 변수명은 대소문자를 구분하고, keywords는 사용할 수 없다.
- 변수의 데이터 타입에는 **string, integer, float, boolean, array** 등이 존재한다.
- php 인터프리터의 해석이 끝난 후 HTML 파일이 브라우저로 전송되면, php 인터프리터는 저장하고 있던 변수의 값을 모두 잊어버린다.

## 배열

- 모든 배열은 **key**와 **value**를 가지는 **요소(element)** 로 이루어져있다.
- key를 변수명처럼 사용하느냐 index로 사용하느냐에 따라 아래와 같이 나뉜다.

### 연관 배열(associative array)

- key를 변수명처럼 사용하는 배열이다. 아래와 같이 예를 들 수 있다.

```
$member = [
  'name' => 'Ivy',
  'age' => 32,
  'country => 'Italy',
];
```

배열을 생성할 때는 대괄호 `[`로 열어서 `]`로 닫아주고, 이중 화살표 연산자인 `=>`를 통해 key에 value를 할당한다.

key와 value는 따옴표로 묶어주면 되고, key에 숫자를 쓰는 경우 따옴표로 묶어주지 않아도 된다.

참고로, key에 숫자를 쓰는 경우 따옴표로 묶지 않아도 배열에 접근할 때, `arr[1]`, `arr['1']`과 같이 두 방법으로 모두 접근이 가능하다.

대괄호 대신 아래와 같이 `array()` 를 통해 배열을 생성할 수도 있다.

```

$member = array(
  'name' => 'Ivy',
  'age' => 32,
  'country => 'Italy',
);
```

연관 배열을 할당한 후에는 인덱스 배열처럼 `member[0]` 으로 첫 번째 요소에 접근할 수 없다. ('0'이라는 key가 integer 키가 없는 경우)

### 인덱스 배열(index array)

- key를 index로 사용하는 배열이다. 아래와 같이 예를 들 수 있다.

```
$member_info = [
  'name',
  'age',
  'country',
];
```
또는
```
$member_info = array(
  'name',
  'age',
  'country',
);
```

역시 인덱스 배열을 만든 이후에는 연관 배열처럼 key에 string과 같은 key를 사용할 수 없다.

만약, 인덱스 배열의 크기가 2인데 index 10을 추가하는 것은 가능하다.

이 때는 `arr[2]`가 아닌 `arr[10]`으로 접근해야한다.

# 다차원 배열

- 앞에서 설명한 배열의 element에 또 다른 배열을 추가하여 다차원 배열을 만들 수 있다. 예시는 아래와 같다.

```
$students = array(
    array(
        "name" => "Tom",
        "age" => 20,
        "major" => "Computer Science"
    ),
    array(
        "name" => "Jerry",
        "age" => 22,
        "major" => "Mathematics"
    ),
    array(
        "name" => "Spike",
        "age" => 21,
        "major" => "Physics"
    )
);
```

다차원 배열도 연관 배열처럼 `key`를 가지도록 만들 수 있긴 한데, 행으로 나타내기 위해 위와 같이 인덱스 배열로 많이 사용하는듯 하다.

`students[0]['name']`에 접근하면 `"Tom"`을 가져올 수 있다.

# `echo`의 간단 표기법

php 태그가 브라우저에 값을 출력하는 `echo`만 사용한다면 `<?php echo $name; ?>`처럼 작성하는 대신에 `<?= $name ?>` 으로 간단하게 작성할 수 있다. 참고로, 맨 앞의 `?=`는 붙여써야한다.

대부분 아래와 같이 php 코드는 변수에 값을 저장하는 부분 첫 번째 부분과 브라우저에 전송되는 HTML 코드를 생성하는 두 부분으로 나뉜다.

```
<?php 
$best_sellers = ['Chocolate', 'Mints', 'Fudge',
    'Bubble gum', 'Toffee', 'Jelly beans',];
?>
<!DOCTYPE html>
<html>
  <head>
    <title>Indexed Arrays</title>
    <link rel="stylesheet" href="css/styles.css">
  </head>
  <body>
    <h1>The Candy Store</h1>
    <h2>Best Sellers</h2>
    <ul>
      <li><?php echo $best_sellers[0]; ?></li>
      <li><?php echo $best_sellers[1]; ?></li>
      <li><?php echo $best_sellers[2]; ?></li>
    </ul>
  </body>
</html>
```

따라서, 대부분 php에서 `echo`를 사용하여 텍스트를 출력한 후, HTML 코드 내에 텍스트를 삽입하는 부분인 두 번째 부분에서 `<?=`, `?>`으로 코드를 축약할 수 있을 것이다.

그리고 참고로, php 코드와 HTML 코드는 가능한 한 많이 분리하는 것이 좋다.

# 문자열 연산자

Concatenation을 해주는 문자열 연산자로는 `+` 대신 `.`을 사용한다.

그리고 연결 할당 연산자로 `.=`를 사용한다.

```
$prefix  = 'Thank you';
$name    = 'Ivy';
$message = $prefix . ', ' . $name;
```

예를 들어 위와 같이 `.` 연결 연산자를 통해 두 변수를 `, `으로 연결할 수도 있고, `" "`을 변수 안에 묶어서 아래와 같이 사용할 수도 있다.

```
$prefix  = 'Thank you';
$name    = 'Ivy';
$message = "$prefix, $name"; // 'Thank you, Ivy'
$message = "$prefix $name";  // 'Thank you Ivy'
```

# 비교 연산자

- `==`           : 값이 동일한지 비교
- '!=`  또는 `<>` : 값이 다른지 비교
- `===`          : 값과 데이터 타입 모두 같은지 비교
- `!==`          : 값과 데이터 타입 모두 다른지 비교
- `<=>`          : 두 값이 같으면 `0`, 왼쪽 값이 더 크면 `1`, 오른쪽 값이 더 크면 `-1`을 리턴

# 타입 저글링

php 인터프리터는 Javascript와 같이 **Loosely typed** 언어이기 때문에 변수를 생성할 때 데이터 타입을 지정해주지 않고, 변수에 데이터가 저장될 때 데이터 타입이 지정되게 된다.

참고로 C와 C++ 같은 언어는 **Strictly typed** 언어라고 한다.

따라서 integer가 저장되어 있었지만, string이 재할당되는 경우 타입이 string으로 바뀌기도 한다.

php는 만약 어떤 타입을 받을 것으로 예상되는 데이터 타입이, 다른 데이터 값을 입력으로 받으면 예상하는 데이터 타입으로 바꾸려고 하는 **타입 저글링(type juggling)** 을 시도한다.

예를 들어, `$total = 1 + '2';`의 경우 `'2'`가 integer로 변환되어 `$total`에는 `3`이 저장되고, 

반대로 `$total = 'Hi' + 2;'의 경우 `2`가 string으로 변환되어 `$total`에는 `'Hi2'`가 저장된다.

타입 저글링은 php 인터프리터가 캐스팅을 수행하므로 **implicit casting**이라고 하고, 반면에 프로그래머가 명시적으로 php 인터프리터에게 데이터 타입을 변경하도록 하는 것은 **explicit casting**이라고 한다.

아래는 PHP & MySQL 책의 61페이지에 존재하는 타입 저글링 예시들이다.

<img width="981" alt="image" src="https://github.com/user-attachments/assets/41aad21b-e9bf-4095-82f2-549ce69211d7">

