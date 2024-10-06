# 조건문

- `if`, `switch`, **삼항 연산자** 등 C언어와 사용 방법이 동일하다.
- `switch`에서 조건 변수에 string을 사용할 수 있다는 차이점이 있다.
- `match`문을 통해 parameter로 전달되는 변수가 특정 값과 일치할 경우 표현식을 실행하고 값을 변수에 반환할 수 있다. (`switch`문의 변형) 예시는 아래와 같다.
- `match`문에서는 `switch` 문과 달리 `===`를 통해 변수의 타입까지 비교하고, 일치하는 항목이 없는데 `default` 항목이 없는 경우 오류가 발생한다.

```
<?php
$day = 'Monday';

$offer = match($day) {
    'Monday'             => '20% off chocolates',
    'Saturday', 'Sunday' => '20% off mints',
    default              => '10% off your entire order',
}; 
/*
NOTE: The semi-colon added at the end of the last statement was missing in print edition. 

It worked without it because it is was the last statement in the PHP block,
but it is clearer to have a semi-colon after every statement 
(and one would be required if another statement was added afterwards).
*/ 
?>
<!DOCTYPE html>
<html>
  <head>
    <title>match Expression</title>
    <link rel="stylesheet" href="css/styles.css">
  </head>
  <body>
    <h1>The Candy Store</h1>
    <h2>Offers on <?= $day ?></h2>
    <p><?= $offer ?></p>
  </body>
</html>
```

# 반복문

- `while`, `do while`, `for` 역시 C언어와 사용법이 동일하며, 배열과 같은 복합 데이터 타입에 대해 element를 순회하는 `foreach`문을 사용할 수 있다.
- `foreach`문을 통해 배열의 element인 key와 value에 접근할 수 있다.
- `foreach($array as $key => $value)`와 같이, `$array` 배열의 key와 value를 각각 `$key`, `$value` 변수에 저장한다. 
- key를 제외하고 value에만 접근할 때에는 `foreach($array as $value)`와 같이 사용하면 `$value` 변수에 value 값이 저장된다. 사용 예시는 아래와 같다.

```
<?php
$products = [
    'Toffee' => 2.99,
    'Mints'  => 1.99,
    'Fudge'  => 3.49,
];
?>
<!DOCTYPE html>
<html>
  <head>
    <title>foreach Loop</title>
    <link rel="stylesheet" href="css/styles.css">
  </head>
  <body>
    <h1>The Candy Store</h1>
    <h2>Price List</h2>
    <table>
      <tr>
        <th>Item</th>
        <th>Price</th>
      </tr>
      <?php foreach ($products as $item => $price) { ?>
        <tr>
          <td><?= $item ?></td>
          <td>$<?= $price ?></td>
        </tr>
      <?php } ?>
    </table>
  </body>
</html>
```

참고로 위 코드에서 아래 부분은 HTML 태그와 나눠서 가독성을 높였는데, 다르게도 사용할 수 있다. 하지만 위처럼 사용하는게 제일 좋다고 한다.

```
 <?php
foreach ($products as $item => $price) { ?>
    <tr>
      <td><?= $item ?></td>
      <td>$<?= $price ?></td>
    </tr>
<?php } ?>
```

```
<?php
foreach ($products as $item => $price) {
    echo "<tr>
            <td>{$item}</td>
            <td>\${$price}</td>
          </tr>";
  } 
?>
```

```
<?php
foreach ($products as $item => $price) {
    echo "<tr>
            <td>" . $item . "</td>
            <td>$" . $price . "</td>
          </tr>";
} 
?>
```

```
<?php
foreach ($products as $item => $price) {
    echo <<<HTML
    <tr>
      <td>$item</td>
      <td>$$price</td>
    </tr>
HTML;
} 
?>
```

# include, require

대부분 웹사이트는 동일한 코드를 여러 페이지에서 반복해서 사용해야 하는 경우가 많다.

예를 들어, header와 footer는 모든 페이지에서 동일해야 한다. 이렇게 모든 페이지에 동일한 코드를 작성하는 대신 include 파일을 통해 효율을 높일 수 있다.

```
<?php include 'includes/filename.php'; ?>
```

코드를 통해 서버에 있는 파일을 가져와서, 마치 그 파일의 내용이 include 구문이 작성된 위치에 작성된 것처럼 처리하도록 php 인터프리터에게 지시한다.

대부분 include 파일은 `includes` 디렉토리 내에 저장해둔다. 아래와 같이 header와 footer를 구성하는 php 파일을 저장하여 불러쓸 수 있다.

#### header.php
```
<!DOCTYPE html>
<html>
  <head>
    <title>The Candy Store</title>
    <link rel="stylesheet" href="css/styles.css">
  </head>
  <body>
    <h1>The Candy Store</h1>
    <nav>
      <a href="index.php">Home</a> | 
      <a href="candy.php">Candy</a> | 
      <a href="about.php">About</a> | 
      <a href="contact.php">Contact</a>
    </nav>
```

#### footer.php
```
    <footer>&copy; <?php echo date('Y')?></footer>
  </body>
</html>
```

### 전체 코드
```
<?php 
$stock = 25;

if ($stock >= 10) {
    $message = 'Good availability';
}
if ($stock > 0 && $stock < 10) {
    $message = 'Low stock';
}
if ($stock == 0) {
    $message = 'Out of stock';
}
?>
<?php require_once 'includes/header.php'; ?>

<h2>Chocolate</h2>
<p><?= $message ?></p>

<?php include 'includes/footer.php'; ?>
```

`include`와 `require`의 차이는 php 인터프리터가 파일의 경로를 찾을 때, 파일이 읽을 수 없는 경우 동작하는 방식에서 차이가 난다.

- `include` : 인터프리터가 오류를 생성하지만, 페이지의 나머지 부분은 계속 처리한다.
- `require` : 인터프리터가 오류를 생성하며, 페이지의 나머지 부분에 대한 처리를 중단한다.

`include_once`와 `require_once`는 `include`, `require`와 동작 방식은 동일하지만, php 인터프리터가 페이지에 파일을 한 번만 포함하도록 하는지 여부를 정한다.

두 키워드를 사용하면, 이미 인클루드 파일이 포함된 경우, 다시 동일한 파일을 포함하지 않게 된다.

# 예제

```
<?php
$name = 'Ivy';                                   // Store the user's name

$greeting = 'Hello';                             // Create initial value for greeting
if ($name) {                                     // If $name has a value
    $greeting = 'Welcome back, ' . $name;        // Create personalized greeting
}

$product = 'Lollipop';                           // Product name
$cost    = 2;                                    // Cost of single pack

for ($i = 1; $i <= 10; $i++) {
    $subtotal   = $cost * $i;                    // Total for this quantity
    $discount   = ($subtotal / 100) * ($i * 4);  // Discount for this quantity
    $totals[$i] = $subtotal - $discount;         // Add discounted price to indexed array
}
?>

<?php require 'includes/header.php'; ?> 

  <p><?= $greeting ?></p>
  <h2><?= $product ?> Discounts</h2>
  <table>
    <tr>
      <th>Packs</th>
      <th>Price</th>
    </tr>
    <?php foreach ($totals as $quantity => $price) { ?>
    <tr>
      <td>
        <?= $quantity ?>
        pack<?= ($quantity === 1) ? '' : 's'; ?>
      </td>
      <td>
        $<?= $price ?>
      </td>
    </tr>
    <?php } ?>
  </table>

<?php include 'includes/footer.php' ?>
```

- C언어와 달리 `for`문 블록안에서 변수를 선언해도, 블록 밖에서 접근할 수 있다.
- 선언되지 않은 배열을 `$totals[$i] = $subtotal - $discount;`와 같이 인덱스와 함께 선언이 가능하다.
- `pack<?= ($quantity === 1) ? '' : 's'; ?>`에서 삼항 연산자를 통해 `'pack'`에 `'s'`를 붙이는 부분 잘 살펴보자.
