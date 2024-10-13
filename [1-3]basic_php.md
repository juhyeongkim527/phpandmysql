# Function

js와 거의 동일한 방식으로, `function` 키워드를 통해 함수를 정의할 수 있다.

# Parameter, Return

`return`을 통해 당연히 데이터를 반환할 수 있다. 

당연히 C와 동일하게 `return` 이후의 코드는 실행되지 않으며,

함수의 재사용성 때문에 함수 안에서 `echo` 명령어를 사용하는 것보다, `echo` 하고자 하는 값을 `return`하는게 낫기 때문에 아래의 방법으로 리턴 받은 값을 출력할 수 있다.

```
<?= function_name(); ?>
```

---

parameter와 return 값에 복합 데이터 타입(배열, 객체 등)을 사용할 수 있다. 아래는 예시이다.
 
```
<?php
$us_price = 4;
$rates = [
    'uk' => 0.81,
    'eu' => 0.93,
    'jp' => 113.21,
];

function calculate_prices($usd, $exchange_rates)
{
    $prices = [
        'pound' => $usd * $exchange_rates['uk'],
        'euro'  => $usd * $exchange_rates['eu'],
        'yen'   => $usd * $exchange_rates['jp'],
    ];
    return $prices;
}

$global_prices = calculate_prices($us_price, $rates);
?>
<!DOCTYPE html>
<html> 
  <head>
    <title>Functions with Multiple Values</title>
    <link rel="stylesheet" href="css/styles.css">
  </head>
  <body>
    <h1>The Candy Store</h1>
    <h2>Chocolates</h2>
    <p>US $<?= $us_price ?></p>
    <p>(UK &pound; <?= $global_prices['pound'] ?> | 
        EU &euro;  <?= $global_prices['euro']  ?> | 
        JP &yen;   <?= $global_prices['yen'] ?>)</p>
  </body>
</html>
```

# Local Variable, Global Variable

함수는 `{}`를 통해 묶이는데, PHP 인터프리터는 `}`에 도달하면 함수에 저장된 모든 값을 잊는다.

왜냐하면 Parameter를 포함한 함수 내부의 값은 **Local Variable**이기 때문이다.

파이썬에서와 동일하게, 함수 내부에서 **Global Variable**에 접근하기 위해서는 `global` 키워드를 통해 변수명을 선언해주면 된다.

`global`을 쓰지 않고 함수 내부에서 전역 변수와 동일한 이름의 변수를 선언하면, 새로운 지역 변수를 선언하는 것으로 해석된다.

참고로 Python에서는 `global`을 쓰지 않고 전역 변수를 읽는 것까지는 가능하지만, php에는 이것 또한 불가능하다.

# Static Variable

**Static Variable**은 `static` 키워드를 통해 선언이 가능하고, 함수 내에서 지역 변수로 사용되었을 때도 정적 변수라면 값이 메모리에서 지워지지 않고 유지된다.

따라서 아래와 같은 코드의 경우, `static` 키워드가 없었다면 함수 호출이 끝난 경우 `$running_total` 변수의 값이 메모리에서 삭제되어 사라져서 기억되지 않는다.

하지만, `static` 키워드를 통해 함수 호출이 끝나도 메모리에 변수 값이 그대로 남아있기 때문에, 다음 호출 때 해당 값을 기억해놨다가 사용할 수 있다.

참고로, 당연히 `static` 지역 변수의 경우 함수 내부에서만 접근이 가능하다.

---

C에서는 **Static Global Variable**이 다른 파일에서 해당 파일에 접근하지 못하도록 하는 의미가 존재하지만,

php에서는 애초에 정적 전역 변수를 선언하면 오류가 발생하므로, `static` 키워드는 지역 변수의 사용에서만 의미가 존재한다.

# Type declaration

js와 달리, php에서는 아래와 같이 함수의 paramter와 return 타입을 지정할 수 있다.

```
<?php
$price    = 4;
$quantity = 3;

function calculate_total(int $price, int $quantity): int
{
    return $price * $quantity;
}

$total = calculate_total($price, $quantity);
?>
<!DOCTYPE html>
<html> 
  <head>
    <title>Type Declarations</title>
    <link rel="stylesheet" href="css/styles.css">
  </head>
  <body>
    <h1>The Candy Store</h1>
    <h2>Chocolates</h2>
    <p>Total $<?= $total ?></p>
  </body>
</html>
```

---

만약 paramter나 return 타입으로 지정해준 타입과 다른 타입이 전달되면, PHP 인터프리터는 지정해준 타입으로 입력받은 타입을 저글링한다.

예를 들어, parameter에 `int`를 받도록 지정했는데 `true` 값이 들어온다면 정수 `1`로 저글링하여 변환될 것이다.

그런데 여기서, 아예 아래와 같이 **Strict type**을 활성화하면 저글링을 하지 않고 오류를 발생하게 할 수 있다.

오류를 발생시킨 후 핸들링을 하거나, 아예 프로그램을 종료하도록 할 수 있을 것이다.

```
declare(strict_types = 1);
```

---

그리고 PHP 8에는 타입 지정에서 **union type**과 `mixed` 타입을 사용할 수 있다.

유니온 타입은 `int|null`(`?int`)과 같이 `|` 기호를 통해 여러 집합의 타입 중 하나가 될 수 있음을 나타내는 것이며,

`mixed` 타입은 모든 타입이 될 수 있음을 나타내는 것으로, paramter나 return 타입 지정을 제외하고 일반 변수들은 해당 타입을 가질 수 없기 때문에 **pseudo-type**이라고 부른다.

# Named argument

지정 인수(named argument) 없이는, 함수 정의에 있는 paramter와 동일한 순서로 argument를 전달해야 한다.

하지만, 지정 인수를 사용하면 arguemnt의 순서를 상관 쓸 필요가 없다.

예를 들어, 함수의 paramter가 아래와 같이 `$cost`, `$quantity`, `$discount = 0`, `$tax = 20` 순서대로 정의되어 있는 상황을 보자.

```
function calculate_cost($cost, $quantity, $discount = 0, $tax = 20,)
```

참고로, `$discount = 0`, `$tax = 20`은 해당 argument가 전달되지 않았을 때도 사용 가능한 **Default argument**이다.

여기서, 지정 인수를 사용하지 않고 `$tax` 값을 지정하려면, 순서대로 인자를 전달해줘야 하기 때문에 아래와 같이 `$discount` 값이 Default로 지정되어 있음에도 사용 시 똑같이 지정해줘야 한다.

```
calculate_cost(5, 10, 0, 5); // 또는 calculate(5, 10, '', 5);
```

하지만, 아래와 같이 지정 인수를 쓰면 순서도 상관쓰지 않아도 되서 `$discount` 값을 신경쓰지 않아도 된다.

```
calculate(cost : 10, quantity : 5, tax : 5);
```

그리고 순서를 지킨다면, 아래와 같이 일부 인수만 지정 인수로 사용할 수 있다.

```
calculate(5, 10, tax : 5);
```



