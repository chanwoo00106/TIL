# Rust 배우기

[참고 자료](https://dhghomon.github.io/easy_rust/Chapter_0.html)<br />
[Rust Playground](https://play.rust-lang.org/)

## Commnets

Rust의 주석은 `//`나 `/**/`로 이루어 진다

```rs
fn main() {
    // Teemo is best
    // 아무말
    let some_number /*어쩌고 저쩌고*/ = 100;
}
```

## Types

Rust는 많은 타입을 가지고 있는데 숫자, 문자 등이 있는데 일부는 간단하고 일부는 복잡하기도 하며 직접 만들 수 있기도 하다.

## Primitive Type(원시 타입?)

Rust에서 가장 쉬운 타입으로 가장 기본적인 것들이다. 우선 `Integers`는 소수점을 가지지 않는 모든 숫자를 말하는데 거기서 아래와 같이 두 가지 종류로 나뉜다

- Signed integers
- Unsigned integers

우선 `Signed integers`는 `+`, `-` 둘 다 가질 수 있다.<br>
하지만 `Unsigned integers`는 오직 `+`만 가질 수 있다.

`Signed integers`에는 `i8`, `i16`, `i32`, `i64`, `i128`, `isize`이런 것들이 있고,<br />
`Unsigned integers`에는 `u8`, `u16`, `u32`, `u64`, `u128`, `usize` 이런 것들이 있다.

여기서 옆에 있는 숫자가 커지면 당연히 변수에 담을 수 있는 숫자의 크기도 커진다.

글고 `isize`랑 `usize`는 사용자의 컴퓨터 성능에 따라 바뀌는데 만약 내 컴퓨터가 64bit 컴표터면 `i64` 또는 `u64`가 되는 거고, 32bit 컴표터면 `i32` 또는 `u32`가 된다.

이렇게 타입이 많은 이유는 컴퓨터 마다 성능 차이가 있기 때문이라고 한다.

다음은 `char`형 타입인데 Rust에서는 Unicode를 사용하고 있다.<br />
그래서 이모지 라던가 중국어 같은 한자도 적을 수 있다고 한다

```rs
fn main() {
    let first_letter = 'A';
    let space = ' '; // A space inside ' ' is also a char
    let other_language_char = 'Ꮔ';
    let cat_face = '😺';
}
```

가장 자주 사용되는 문자들은 0 ~ 256 사이에 들어가는데 이것은 `u8`타입으로 표현이 가능하다.<br />
`u8`은 0부터 255까지 표현 할 수 있다. 이것은 Rust가 `u8`을 char로 안전하게 바꿀 수 있음을 의미한다.

타입을 바꿀 때는 `as`를 사용하는데 Rust는 매우 엄격해서 `u8`로만 타입 변경을 할 수 있나보다. 그래서 다음 예제는 동작 하지 않는다고 한다.

```rs
fn main() { // main() is where Rust programs start to run. Code goes inside {} (curly brackets)

    let my_number = 100; // We didn't write a type of integer,
                         // so Rust chooses i32. Rust always
                         // chooses i32 for integers if you don't
                         // tell it to use a different type

    println!("{}", my_number as char); // ⚠️
}
```

`let my_number = 100;`이렇게 변수를 선언하면 기본적으로 `i32`타입이 되게 되는데 이 타입은 char 형으로 바꿀 수 없어서 나는 오류이다.

그러므로 저 숫자 100을 char형으로 바꾸려면 처음부터 변수 선언을 `u8`타입으로 하던지 아님 print할때 `u8`타입으로 바꾼 후 `char`형으로 바꿔서 출력 하면 된다.

```rs
fn main() {
    let my_number = 100;
    println!("{}", my_number as u8 as char);
}
// ----------------or------------------------
fn main() {
    let my_number: u8 = 100;
    println!("{}", my_number as char);
}
```

마지막으로 문자열 뒤에 `.len()`을 붙여주면 그 문자열의 사이즈를 알려준다

```rs
fn main() {
    println!("Size of a char: {}", std::mem::size_of::<char>()); // 4 bytes
    println!("Size of string containing 'a': {}", "a".len()); // .len() gives the size of the string in bytes
    println!("Size of string containing 'ß': {}", "ß".len());
    println!("Size of string containing '国': {}", "国".len());
    println!("Size of string containing '𓅱': {}", "𓅱".len());
}
```

출력 결과

```
Size of a char: 4
Size of string containing 'a': 1
Size of string containing 'ß': 2
Size of string containing '国': 3
Size of string containing '𓅱': 4
```

이때 문자열의 길이를 알고 싶다면 `.chars().count()`를 사용한다.<br />
`.chars().count()`는 작성한 내용을 문자로 변환한 다음 몇 개인지 계산한다.

```rs
fn main() {
    let slice = "Hello!";
    println!("Slice is {} bytes and also {} characters.", slice.len(), slice.chars().count());
    let slice2 = "안녕!";
    println!("Slice2 is {} bytes but only {} characters.", slice2.len(), slice2.chars().count());
}
```

출력 결과

```
Slice is 6 bytes and also 6 characters.
Slice2 is 7 bytes but only 3 characters.
```

## Type inference(타입 추론)

변수를 타입없이 안에 숫자를 넣고 선언하면 기본적으로 `u32`타입을 갖게 된다.<br />
하지만 이게 싫다면 직접 타입을 적어줘야 한다.

타입을 명시하는 방법은 3가지(?) 정도 있는 것 같은데

첫 번째로는 변수명 옆에 `:`을 붙여서 작성하는 거랑

```rs
fn main() {
    let small_number: u8 = 10;
}
```

두 번째로는 숫자 뒤에 같이 적는 방법이랑

```rs
fn main() {
    let small_number = 10u8;
}
```

세 번째로는 숫자랑 문자 사이에 `_`를 붙여주는 방법이다.

```rs
fn main() {
    let small_number = 10_u8;
}
```

그런데 여기서 `_`는 숫자를 보기 편하게 하려고 적는 것이여서 많이 적어도 상관 없다.

```rs
fn main() {
    let number = 10_________________________u8;
    let number2 = 1___6______2____4______i32;
    println!("{} {}", number, number2); // 10 1624
}
```

## floats

`Floats`는 소수점이 있는 숫자를 말하는데 예를 들어 `0.5`, `5.0` 같은 것들을 말한다.

글고 코드에서는 `5.0` 같은 것들은 `5.`이라 적어도 된다.

```rs
fn main() {
    let my_float = 5.;
}
```

만약 타입을 명시하지 않았다면 Rust는 변수 타입을 기본적으로 `f64`로 선언하게 됩니다.<br />
Rust에는 `f32`랑 `f64`밖에 없어서 둘 중 하나만 고르면 된다.

여기서 `f32`랑 `f64`는 서로 연산이 불가능하다
그래서 둘 중 하나의 타입을 맞춰줘야 한다

```rs
fn main() {
    let my_float: f64 = 5.0;
    let my_other_float: f32 = 8.5;

    let third_float = my_float + my_other_float as f64;
}
```

하지만 Rust는 똑똑해서 타입을 선언하는 부분을 쓰지 않으면 f32가 필요한 경우에는 f32를 선택한다.

```rs
fn main() {
    let my_float: f32 = 5.0;
    let my_other_float = 8.5; // Rust는 보통 f64 선택함,

    // 여기서는 f32가 필요하기 때문에 f32를 선택함
    let third_float = my_float + my_other_float;
}
```

## Printing 'hello, world!'

코드 분석을 좀 하자면 아래와 같은 코드가 있다고 할 때

```rs
fn main() {
    println!("Hello, world!");
}
```

- `fn`은 함수(function)을 의미하고
- `main`은 프로그램을 시작했을 때 처음 실행되는 함수이다.
- `()`는 함수에게 넘겨줄 매개변수를 여기에 적어준다.
- `{}`는 코드블럭으로 여기 안에 코드를 작성한다.

마지막으로 코드의 마무리는 `;`으로 끝나야 한다.

`println!()`은 말 그대로 출력하는 건데 대충 변수를 섞어서 출력하는 방법은 아래와 같은 방법들이 있다.

```rs
fn main() {
    println!("{} is Best", "Teemo")
    println!("{name} is Best", name="Teemo")
}
```

함수를 만들고 선언하는 방법도 아래와 같다

```rs
fn number() -> i32 {
    8
}

fn main() {
    // Hello, world number 8!
    println!("Hello, world number {}!", number());
}
```

`fn number()`로 해서 `number`라는 함수를 선언하고 -> 다음에 `return`타입을 적어주면 된다.

그런데 `number`함수에서 `return`을 하지 않는데 이건 함수 마지막에 `;`을 붙이지 않으면 자동으로 `return`이 된다. 대신 `return`을 적어주면 꼭 `;`을 붙여야 한다.

그러므로 아래 두 함수는 같은 함수다

```rs
fn number() -> i32 {
    8
}
//------------------------
fn number() -> i32 {
    return 8;
}
```

함수에서 매개변수를 받는 법은 아래와 같다

```rs
fn multiply(number_one: i32, number_two: i32) {
    let result = number_one * number_two;
    println!("{} times {} is {}", number_one, number_two, result);
}

fn main() {
    multiply(8, 9);
    let some_number = 10;
    let some_other_number = 2;
    multiply(some_number, some_other_number);
}
```

`()`사이에 매개변수 명이랑 타입을 적고 `,`으로 구분한다.<br />
함수를 사용할 때는 함수명을 적고 `()`사이에 매개변수를 적으면 된다

## Declaring variables and code blocks

이게 스코프를 말하는 것 같은데 아래와 같은 예제가 있을 때 오류가 나는데<br />
이유는 코드 블럭 안에서 선언된 변수를 바깥에서 찾으려 해서 나는 오류이다

```rs
fn main() {
    {
        let my_number = 8; // my_number starts here
                           // my_number ends here!
    }

    println!("Hello, number {}", my_number); // ⚠️ there is no my_number and
                                             // println!() can't find it
}
```

Rust에서는 코드 블럭을 `return` 변수로 사용할 수 있다고 한다

```rs
fn main() {
    let my_number = {
        let second_number = 8;
        second_number + 9 // No semicolon, so the code block returns 8 + 9.
                          // It works just like a function
    };

    println!("My number is: {}", my_number);
}
```

위와 같이 `second_number`를 코드블럭 안에서 return 하면 그 값은 `my_number`로 가게 된다.

만약 여기서 `second_number + 9` 부분에 세미콜론을 추가하면 이 코드 블럭은 `()`를 반환하게 되는데 이걸 출력하려면 `{:?}`을 사용해야 한다고 한다

```rs
fn main() {
    let my_number = {
    let second_number = 8; // declare second_number,
        second_number + 9; // add 9 to second_number
                           // but we didn't return it!
                           // second_number dies now
    };

    println!("My number is: {:?}", my_number); // my_number is ()
}
```

`{:?}` 이걸 사용해야 하는 이유는 다음 챕터에서

## Display and debug

간단한 변수들은 `{}`안에 넣어서 쉽게 출력이 되지만 가끔 그렇지 않은 것들도 있다<br>
그럴때는 `debug print`라는 걸 사용하면 된다. 이걸 사용하면 많은 정보를 알 수 있지만 그만큼 예쁘게 보이지는 않는다

`debug print`를 언제 사용할지는 compiler가 알려줄 것이다. 예를 들어

```ts
fn main() {
    let doesnt_print = ();
    println!("This will not print: {}", doesnt_print); // ⚠️
}
```

실행시키면 아래와 같은 결과가 나온다

```
error[E0277]: `()` doesn't implement `std::fmt::Display`
 --> src\main.rs:3:41
  |
3 |     println!("This will not print: {}", doesnt_print);
  |                                         ^^^^^^^^^^^^ `()` cannot be formatted with the default formatter
  |
  = help: the trait `std::fmt::Display` is not implemented for `()`
  = note: in format strings you may be able to use `{:?}` (or {:#?} for pretty-print) instead
  = note: required by `std::fmt::Display::fmt`
  = note: this error originates in a macro (in Nightly builds, run with -Z macro-backtrace for more info)
```

중요한 부분은 `you may be able to use {:?} (or {:#?} for pretty-print) instead` 이 부분인데 이 문장의 의미는 `{:?}` 또는 `{:#?}`로 시도해 보라는 의미이다. 그리고 `{:#?}`는 `pretty-print`라고 부른단다. `pretty-print`는 `{:?}`와 비슷한 것 같지만 더 많은 줄로 그나마 예쁘게 보여준다.

따라서 Display는 `{}`로 출력 하는 걸 말하고 Debug는 `{:?}`로 출력하는 걸 말한다.

하나 더: `print!`도 사용 할 수 있다고 한다.

```rs
fn main() {
    print!("This will not print a new line");
    println!(" so this will be on the same line");
}
// 결과
//This will not print a new line so this will be on the same line
```

## Smallest and largest numbers

만약 한 타입에 최대값과 최소값을 보고싶다면 타입 다음에 콜론 두 개를 찍고 MIN과 MAX를 쓰면 된다.

```rs
fn main() {
    println!("The smallest i8 is {} and the biggest i8 is {}.", i8::MIN, i8::MAX); // hint: printing std::i8::MIN means "print MIN inside of the i8 section in the standard library"
    println!("The smallest u8 is {} and the biggest u8 is {}.", u8::MIN, u8::MAX);
    println!("The smallest i16 is {} and the biggest i16 is {}.", i16::MIN, i16::MAX);
    println!("The smallest u16 is {} and the biggest u16 is {}.", u16::MIN, u16::MAX);
    println!("The smallest i32 is {} and the biggest i32 is {}.", i32::MIN, i32::MAX);
    println!("The smallest u32 is {} and the biggest u32 is {}.", u32::MIN, u32::MAX);
    println!("The smallest i64 is {} and the biggest i64 is {}.", i64::MIN, i64::MAX);
    println!("The smallest u64 is {} and the biggest u64 is {}.", u64::MIN, u64::MAX);
    println!("The smallest i128 is {} and the biggest i128 is {}.", i128::MIN, i128::MAX);
    println!("The smallest u128 is {} and the biggest u128 is {}.", u128::MIN, u128::MAX);
}
```

결과

```
The smallest i8 is -128 and the biggest i8 is 127.
The smallest u8 is 0 and the biggest u8 is 255.
The smallest i16 is -32768 and the biggest i16 is 32767.
The smallest u16 is 0 and the biggest u16 is 65535.
The smallest i32 is -2147483648 and the biggest i32 is 2147483647.
The smallest u32 is 0 and the biggest u32 is 4294967295.
The smallest i64 is -9223372036854775808 and the biggest i64 is 9223372036854775807.
The smallest u64 is 0 and the biggest u64 is 18446744073709551615.
The smallest i128 is -170141183460469231731687303715884105728 and the biggest i128 is 170141183460469231731687303715884105727.
The smallest u128 is 0 and the biggest u128 is 340282366920938463463374607431768211455.
```

## Mutability (changing)

`let`으로 변수를 선언하면 그 변수는 상수(절대 바뀌지 않는 값)가 된다

```rs
fn main() {
    let my_number = 8;
    my_number = 10; // ⚠️
}
// 결과
// error[E0384]: cannot assign twice to immutable variable my_number
```

만약 상수가 아닌 변수를 만들고 싶다면 `let` 다음에 `mut`를 적어주면 된다.

```rs
fn main() {
    let mut my_number = 8;
    my_number = 10;
}
```

하지만 변수의 타입까지는 바꿀 수 없다

```rs
fn main() {
    let mut my_variable = 8; // it is now an i32. That can't be changed
    my_variable = "Hello, world!"; // ⚠️
}
```

## shadowing

```rs
fn main() {
    let my_number = 8; // This is an i32
    println!("{}", my_number); // prints 8
    let my_number = 9.2; // This is an f64 with the same name. But it's not the first my_number - it is completely different!
    println!("{}", my_number) // Prints 9.2
}
```

위 예제를 보면 `my_number`가 두 번 선언 된 것을 볼 수 있는데 이걸 보고 `my_number`가 "shadowed" 됐다고 하는 것이다.

처음 선언 됐던 `my_number`는 다음에 선언되는 `my_number`와 같은 코드 블럭에 감싸져 있기 때문에 처음 선언 됐던 `my_number`는 사라지게 된다.

하지만 다른 코드 블럭에 감짜져 있다면 두 변수 모두 볼 수 있다

```rs
fn main() {
    let my_number = 8; // This is an i32
    println!("{}", my_number); // prints 8
    {
        let my_number = 9.2; // This is an f64. It is not my_number - it is completely different!
        println!("{}", my_number) // Prints 9.2
                                  // But the shadowed my_number only lives until here.
                                  // The first my_number is still alive!
    }
    println!("{}", my_number); // prints 8
}
```

#### 그럼 shadowing의 유리한 점은 무엇일까?

shadowing은 변수의 변경이 많을 때 좋다. 만약 변수를 가지고 간단한 연산을 많이 한다고 해본다면

```rs
fn times_two(number: i32) -> i32 {
    number * 2
}

fn main() {
    let final_number = {
        let y = 10;
        let x = 9; // x starts at 9
        let x = times_two(x); // shadow with new x: 18
        let x = x + y; // shadow with new x: 28
        x // return x: final_number is now the value of x
    };
    println!("The number is now: {}", final_number)
}
```

하지만 shadowing이 없다면 계속해서 새로운 변수를 만들어야 한다. 아래 예제와 같이

```rs
fn times_two(number: i32) -> i32 {
    number * 2
}

fn main() {
    // Pretending we are using Rust without shadowing
    let final_number = {
        let y = 10;
        let x = 9; // x starts at 9
        let x_twice = times_two(x); // second name for x
        let x_twice_and_y = x_twice + y; // third name for x!
        x_twice_and_y // too bad we didn't have shadowing - we could have just used x
    };
    println!("The number is now: {}", final_number)
}
```

## The stack, the heap, and pointers

stack, heap 그리고 pointer는 Rust에서 가장 중요하다

일단 stack과 heap의 차이점을 정리해 보자면

- stack은 매우 빠르지만 heap은 그렇지 않다
- Rust는 컴파일 타임에 타입의 크기를 알고 있어야 한다. 그래서 `i32`는 4바이트이기 때문에 stack에 저장이 된다.
- 하지만 몇몇 타입들은 컴파일 타임에 크기를 알지 못할 때가 있다. 하지만 stack은 정확한 크기를 필요로 한다 그래서 데이터는 heap에 두고 stack에서는 그 데이터를 pointer로 가리키면 된다

Pointer는 책을 생각하면 쉽게 이해할 수 있다. 만약 어떤 챕터의 위치를 알고 싶을 때 차례에 가서 몇 페이지인지 보고 찾고 싶었던 챕터를 쉽게 찾을 수 있을 텐데 여기서 챕터를 데이터라고 하고 쪽수를 pointer라고 생각하면 된다

보통 Rust에서는 pointer를 **reference**라고 부른다. 매우 중요한 부분인데 reference는 다른 변수의 메모리를 가리킵니다. reference의 의미는 값을 _빌리는_ 것을 의미한다. 하지만 그 값을 갖지는 않는다. 이것은 방금 설명 했던 책과 같은 의미이다. 그리고 Rust에서는 앞에 `&`를 붙여서 메모리를 가리킨다.

```rs
fn main() {
    let my_variable = 8; // makes a regular variable, but
    let my_reference = &my_variable; // makes a reference.
}
```

여기서 `my_reference = &my_variable`부분은 "`my_reference`는 `my_variable`을 참조한다" 라고 읽으면 된다.

저 코드의 의미는 `my_reference`는 오직 `my_variable`의 데이터만 보게 된다. `my_variable`는 여전히 data를 가지고 있다.

마지막으로 reference를 또 reference 할 수 있는데 아래 예제를 보면

```rs
fn main() {
    let my_number = 15; // This is an i32
    let single_reference = &my_number; //  This is a &i32
    let double_reference = &single_reference; // This is a &&i32
    let five_references = &&&&&my_number; // This is a &&&&&i32
}
```

`my_number`를 reference한 `single_reference`, `single_reference`를 또 reference한 `double_reference`, 심지어는 `my_number`를 5번 reference한 `five_references`도 있다. 결국 다 출력해보면 다 15를 가리키고 있다.
