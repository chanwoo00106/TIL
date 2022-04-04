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

## More about printing

Rust에서는 거의 모든걸 출력할 수 있다<br />
이번 챕터에서는 출력할 때 알아야할 추가 사항을 설명할 것이다.

만약 출력을 할 때 엔터나 탭을 출력하려면 아래와 같이 `\n`과 `\t`를 적어주면 된다.

```rs
fn main() {
    // Note: this is print!, not println!
    print!("\t Start with a tab\nand move to a new line");
}
```

출력 결과:

```
    Start with a tab
and move to a new line
```

하지만 `""`안에 진짜 엔터나 탭을 해도 문제 없다.

```rs
fn main() {
    println!("Hello,
worl    d!");
}
// 출력 결과
//Hello,
//worl    d!
```

만약 `\n`이나 `\t`그리고 `\`를 출력하고 싶다면 `\\n` 나 `\\t` 그리고 `\\`로 해서 출력할 수 있다

```rs
fn main() {
    println!("Here are two escape characters: \\n and \\t");
}

// 출력 결과
// Here are two escape characters: \n and \t
```

만약 역슬래쉬 같은게 문장에 너무 많은데 그걸 전부 무시하고 싶다면 `""` 시작 부분에 `r#` 그리고 끝 부분에 `#`을 적어주면 된다.

```rs
fn main() {
    println!("He said, \"You can find the file at c:\\files\\my_documents\\file.txt.\" Then I found the file."); // We used \ five times here
    println!(r#"He said, "You can find the file at c:\files\my_documents\file.txt." Then I found the file."#)
}
```

출력 결과

```
He said, "You can find the file at c:\files\my_documents\file.txt." Then I found the file.
He said, "You can find the file at c:\files\my_documents\file.txt." Then I found the file.
```

이건 뭔 소린지 모르겠는데 `#`을 여러개 적을 수 있다고 한다

```rs
fn main() {

    let my_string = "'Ice to see you,' he said."; // single quotes
    let quote_string = r#""Ice to see you," he said."#; // double quotes
    let hashtag_string = r##"The hashtag #IceToSeeYou had become very popular."##; // Has one # so we need at least ##
    let many_hashtags = r####""You don't have to type ### to use a hashtag. You can just use #.""####; // Has three ### so we need at least ####

    println!("{}\n{}\n{}\n{}\n", my_string, quote_string, hashtag_string, many_hashtags);

}
```

변수 명을 `let`, `fn`같은 예약어로 하고 싶다면 변수명 앞에 `r#`을 적으면 된다.

```rs
fn main() {
    let r#let = 6; // The variable's name is let
    let mut r#mut = 10; // This variable's name is mut
}
```

추가로 함수명도 `r#`을 붙여 예약어로 사용이 가능하다.

```rs
fn r#return() -> u8 {
    println!("Here is your number.");
    8
}

fn main() {
    let my_number = r#return();
    println!("{}", my_number);
}
```

만약 문자열을 ASCII 코드로 변환하고 싶다면 문자열 맨 앞에 `b`를 붙여주고 출력할 때 debug print를 사용하면 된다

```ts
fn main() {
    println!("{:?}", b"This will look like numbers");
}

//[84, 104, 105, 115, 32, 119, 105, 108, 108, 32, 108, 111, 111, 107, 32, 108, 105, 107, 101, 32, 110, 117, 109, 98, 101, 114, 115]
```

Unicode 를 얻는 방법은 한 글자를 `u32`타입으로 해서 `{:X}`에 넣어 출력해주면 된다.

```rs
fn main() {
    println!("{:X}", '행' as u32); // Cast char as u32 to get the hexadecimal value
    println!("{:X}", 'H' as u32);
    println!("{:X}", '居' as u32);
    println!("{:X}", 'い' as u32);

    println!("\u{D589}, \u{48}, \u{5C45}, \u{3044}"); // Try printing them with unicode escape \u
}
```

지금까지 `{}`로 하거나 `{:?}` 또는 `{:#?}`로 변수를 출력했는데 사실은 이것 말고도 다른 많은 방법들이 있다.

예를 들어 `reference`를 출력 하고 싶으면 `{:p}`를 사용할 수 있다.
결과는 실행 할때 마다 다를 것이다

```rs
fn main() {
    let number = 9;
    let number_ref = &number;
    println!("{:p}", number_ref);
}
```

또는 binary(2진법)나 hexadecimal(16진법), octal(8진법)을 출력하고 싶다면 각각 `{:b}`, `{:x}`, `{:o}`로 사용하면 된다.

```rs
fn main() {
    let number = 555;
    println!("Binary: {:b}, hexadecimal: {:x}, octal: {:o}", number, number, number);
}
```

또 순서를 바꾸기 위해 `{}`사이에 숫자를 넣기도 한다

```rs
fn main() {
    let father_name = "Vlad";
    let son_name = "Adrian Fahrenheit";
    let family_name = "Țepeș";
    println!("This is {1} {2}, son of {0} {2}.", father_name, son_name, family_name);
}

//결과
// This is Adrian Fahrenheit Țepeș, son of Vlad Țepeș.
```

다른 방법도 있는데 각각 `{}` 사이에 이름을 정해서 변수와 연결하는 것도 있다.

```rs
fn main() {
    println!(
        "{city1} is in {country} and {city2} is also in {country},
but {city3} is not in {country}.",
        city1 = "Seoul",
        city2 = "Busan",
        city3 = "Tokyo",
        country = "Korea"
    );
}

// 결과
//Seoul is in Korea and Busan is also in Korea,
//but Tokyo is not in Korea.
```

또 다른 방법으론 `ㅎ`다섯 개 사이에 `a`를 넣고 싶다면 다음과 같이 하면 된다.

```rs
fn main() {
    let letter = "a";
    println!("{:ㅎ^11}", letter);
}

// 결과
//ㅎㅎㅎㅎㅎaㅎㅎㅎㅎㅎ
```

일단 `{:ㅎ^11}` 이거의 의미를 설명하자면 콜론 앞부분에는 변수가 들어간다.<br />
그래서 위 예제의 println은 다음과 같이도 쓸 수 있다.

```rs
fn main() {
    let letter = "a";
    println!("{value:ㅎ^11}", value = letter);
}

// 결과
//ㅎㅎㅎㅎㅎaㅎㅎㅎㅎㅎ
```

그리고 콜론 뒤에는 `ㅎ^11`이 있는데 이건 가운데 있는 기호를 잘 봐야 하는데 `<`, `^`, `>` 이렇게 사용할 수 있다. `<`의 의미는 변수의 왼쪽에 `ㅎ`들을 두겠다는 거고,
`>`의 의미는 오른쪽에, `^`의 의미는 가운데 이다.

마지막으로 맨 뒤에 숫자는 여기에 들어올 수 있는 글자의 길이 이다. 그래서 최솟값고 최댓값을 넣을 수 있는데 지금은 최소 11글자를 `ㅎ`으로 채우겠다는 거고 그 뒤에 `.`을 붙이고 숫자를 넣으면 그게 최댓값이 되고 만약 변수의 글자 길이가 설정한 최댓값을 넘어가면 최댓값에서 끊기게 된다.

```rs
fn main() {
    let letter = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa";
    println!("{value:ㅎ<11.20}", value = letter);
}

//결과
// aaaaaaaaaaaaaaaaaaaa
```

그래서 이걸 아래와 같이 활용할 수 있을 것 같다

```rs
fn main() {
    let title = "TODAY'S NEWS";
    println!("{:-^30}", title); // no variable name, pad with -, put in centre, 30 characters long
    let bar = "|";
    println!("{: <15}{: >15}", bar, bar); // no variable name, pad with space, 15 characters each, one to the left, one to the right
    let a = "SEOUL";
    let b = "TOKYO";
    println!("{city1:-<15}{city2:->15}", city1 = a, city2 = b); // variable names city1 and city2, pad with -, one to the left, one to the right
}
```

```
---------TODAY'S NEWS---------
|                            |
SEOUL--------------------TOKYO
```

## Strings

Rust에는 2가지 string 타입이 있다. `String`과 `&str`인데 서로 무슨 차이가 있을까?

- 우선 `&str`은 simple string이라고 한다. 즉 `let my_variable = "Hello, world!"`이렇게 변수를 선언 했을 때 `&str`타입의 변수를 만든 것이다. 그리고 `&str`은 매우 빠르다.
- `String`은 좀 더 복잡한 string이다. `&str` 보다 더 느리지만 그만큼 가지고 있는 함수도 많다. `String`은 pointer 이고 data는 heap에 있다.

또한 `&str`도 앞에 `&`가 있는 걸 볼 수 있는데 그 이유는 str을 사용하기 위해선 reference가 필요하기 때문이다. stak을 사용하려면 사이즈를 알아야 하는데 그것 때문에 `&`를 붙여준다.

`&str`과 `String`은 둘다 UTF-8 형식이다. 사용은 아래와 같이 한다.

```rs
fn main() {
    let name = "서태지"; // This is a Korean name. No problem, because a &str is UTF-8.
    let other_name = String::from("Adrian Fahrenheit Țepeș"); // Ț and ș are no problem in UTF-8.
}
```

비록 달라 보이지만 둘은 연결되어 있다.

출력 할 때 안에 이모지까지 넣을 수 있다

```rs
fn main() {
    let name = "😂";
    println!("My name is actually {}", name);
}
```

`str`앞에 `$`를 붙이는 이유를 다시 이해해 보자면<br>
str은 dynamically sized 타입이다 즉 사이즈가 다를 수 있다는 뜻<br>
예를 들어 `Teemo`와 `I love Teemo`의 사이즈는 서로 다르다.

```rs
fn main() {
    println!("A String is always {:?} bytes. It is Sized.", std::mem::size_of::<String>()); // std::mem::size_of::<Type>() gives you the size in bytes of a type
    println!("And an i8 is always {:?} bytes. It is Sized.", std::mem::size_of::<i8>());
    println!("And an f64 is always {:?} bytes. It is Sized.", std::mem::size_of::<f64>());
    println!("But a &str? It can be anything. 'Teemo' is {:?} bytes. It is not Sized.", std::mem::size_of_val("Teemo")); // std::mem::size_of_val() gives you the size in bytes of a variable
    println!("And 'I love Teemo' is {:?} bytes. It is not Sized.", std::mem::size_of_val("I love Teemo"));
}
```

```
A String is always 24 bytes. It is Sized.
And an i8 is always 1 bytes. It is Sized.
And an f64 is always 8 bytes. It is Sized.
But a &str? It can be anything. '서태지' is 9 bytes. It is not Sized.
And 'Adrian Fahrenheit Țepeș' is 25 bytes. It is not Sized.
```

`&`는 포인터로 만들어 주고 Rust는 포인터의 사이즈를 알고 하기 때문에 필요한 것이다.<br>
그래서 포인터는 stack으로 간다. 만약 `&`를 없이 적어버리면 Rust는 사이즈를 몰라 문제가 생긴다.

`String`으로 만드는 여러가지 방법들

- `String::from("This is the string text");`
- `"This is the string text".to_string()`
- 마지막으로 `format!` 인데 이건 `println!`와 비슷하게 사용이 가능하다. 그래서 아래와 같이 사용이 가능하다.

```rs
fn main() {
    let my_name = "Billybrobby";
    let my_country = "USA";
    let my_home = "Korea";

    let together = format!(
        "I am {} and I come from {} but I live in {}.",
        my_name, my_country, my_home
    );
}
```

내 영어 실력이 좋지 않아 다음은 뭔 소린지 모르겠지만

```rs
fn main() {
    let my_string = "Try to make this a String".into(); // ⚠️
}
```

이렇게 하면 에러가 나는데 `&str`에서는 `into()` 쓸 수 없으니 다른걸로 바꾸라는 것 같다.<br>
그래서 아래와 같이 바꾸면

```rs
fn main() {
    let my_string: String = "Try to make this a String".into();
}
```

에러를 지울 수 있다

## const and static

변수를 선언하는 방법에는 `let` 뿐만 아니라 2가지가 있다. `const`랑 `static`이라는 것들이다.

- `const`는 절대 변하지 않는 변수이다. 그러므로 변수의 값을 절대 바꿀 수 없다<br>
  그러다 보니 shadowing도 안 된다.
- `static`은 `const`와 많이 비슷하다. 하지만 `static`은 메모리 위치가 고정되어 있고 전역 변수로도 사용이 가능하다.

이 둘은 진짜 비슷하다. 그래도 Rust programmers는 `static` 보단 `const`를 더 자주 사용한다고 한다.

`const`와 `static`은 변수명을 대문자로 해주는 게 좋다. 그래야지 절대 변하지 않는 상수인 것을 쉽게 구분 할 수 있기 때문이다.<br>
그리고 보통 main 바깥에다 선언을 한다고 한다.

사용 예시:

```rs
const NUMBER_OF_MONTHS: u32 = 12;
static SEASONS: [&str; 4] = ["Spring", "Summer", "Fall", "Winter"];
```

또 이 둘을 선언 할 때는 타입을 생략할 수 없는 것 같다.

## More on references

reference는 Rust에서 굉장히 중요하다. Rust는 reference를 사용해서 메모리 접근이 안전한지 확인한다. 알다시피 `$`는 reference를 만든다.

```rs
fn main() {
    let country = String::from("Austria");
    let ref_one = &country;
    let ref_two = &country;

    println!("{}", ref_one);
}

//결과
// Austria
```

코드에서 `country`는 String이고 그 다음에 있는 reference 두개는 `country`이다.<br>
이것들은 `&String`타입을 가지고 있는데 이것들은 "문자열의 참조"이다.<br>
reference는 100개를 만들어도 문제가 없다.

하지만 아래 예제에는 문제가 있다.

```rs
fn return_str() -> &str {
    let country = String::from("Austria");
    let country_ref = &country;
    country_ref // ⚠️
}

fn main() {
    let country = return_str();
}
```

`return_str`는 String을 만드는 함수이다. 하지만 `return_str`는 String의 reference를 return 하고 있다. 그리고 문자열 `country`는 오직 함수 안에만 있다 사라진다. 저 변수는 컴퓨터가 메모리를 정리하거나 다시 사용할 때 지워지게 된다. 따라서 `country_ref`는 사라진 메모리를 참조하게 된다. 그래서 Rust는 메모리 실수하는 것을 방지한다.

즉 `&String`은 `String`이 사라질 때 같이 사라지게 된다. 그러므로 `소유권`을 전달하지 않는다.

## Mutable references

만약 reference의 데이터를 바꾸고 싶다면 `mutable reference`를 사용하면 된다.<br>
`mutable reference`를 사용하려면 `&` 대신 `&mut`를 사용하면 된다.

```rs
fn main() {
    let mut my_number = 8; // don't forget to write mut here!
    let num_ref = &mut my_number;
}
```

위 예제에서 `my_number`의 타입은 `i32`가 되고 `num_ref`의 타입은 `&mut i32`가 된다. (우리는 이걸 `i32`에 대한 변경가능한 참조 라고 한다)

여기서 그냥 변수를 바꾸듯이 reference를 바꿔버리면 안 된다. 이유는 이 reference의 타입은 `&i32`이기 때문이다. 그러므로 `*`을 사용해서 반대로 참조하면 된다. 그리고 `*`은 `&`를 지울 수 있다.

```rs
fn main() {
    let mut my_number = 8;
    let num_ref = &mut my_number;
    *num_ref += 10; // Use * to change the i32 value.
    println!("{}", my_number);

    let second_number = 800;
    let triple_reference = &&&second_number;
    println!("Second_number = triple_reference? {}", second_number == ***triple_reference);
}
// 결과
//18
//Second_number = triple_reference? true
```

`&`를 사용하면 `referencing`이라 부르고 `*`을 사용하면 `dereferencing`이라 부른다.

Rust에는 immutable reference와 mutable reference를 위한 2가지 규칙이 있다.

- Rule 1 : immutable reference는 100개, 1000개를 만들어도 문제가 없다.
- Rule 2 : mutable reference는 오직 하나만 가질 수 있다. 또한 immutable reference와 mutable reference를 함께 가질 수는 없다.

왜냐하면 mutable reference는 값을 변경할 수 있는데 내가 이 값을 변경하면 다른 reference도 그걸 읽을 수 있기 때문이다.

그 다음은 Powerpoint presentation을 예로 들어서 열심히 번역하기 귀찮으니 넘어가자 ^^

대신 아래 예시를 보고 이해하자

보면 number라는 변수를 `number_ref` 와 `number_change`로 둘이 참조를 하고 있는데 하나는 mutable 이고 하나는 immutable 이다.

```rs
fn main() {
    let mut number = 10;
    let number_ref = &number;
    let number_change = &mut number;
    *number_change += 10;
    println!("{}", number_ref); // ⚠️
}
```

실행 결과

```
error[E0502]: cannot borrow `number` as mutable because it is also borrowed as immutable
 --> src\main.rs:4:25
  |
3 |     let number_ref = &number;
  |                      ------- immutable borrow occurs here
4 |     let number_change = &mut number;
  |                         ^^^^^^^^^^^ mutable borrow occurs here
5 |     *number_change += 10;
6 |     println!("{}", number_ref);
  |                    ---------- immutable borrow later used here
```

그런데 아래 코드는 에러 없이 잘 작동 하는 것을 볼 수 있다.

```rs
fn main() {
    let mut number = 10;
    let number_change = &mut number; // create a mutable reference
    *number_change += 10; // use mutable reference to add 10
    let number_ref = &number; // create an immutable reference
    println!("{}", number_ref); // print the immutable reference
}
```

이 결과는 `20`이 잘 나오는 걸 볼 수 있다.

그 이유는 컴파일러가 충분히 똑똑하지만 이해하지 못하는 코드도 있기 때문이다.<br >
일단 `number`를 수정하기 위해서 `number_change`를 사용했는데 number_ref를 사용한 후로는 `number`를 수정하지 않아서 문제를 잡지를 못하고 있다. 그래도 어찌 됐든 immutable reference와 mutable reference를 같이 쓰면 안 된다.

## Giving references to functions

reference는 함수에 매우 유용하다. 그 이유는 Rust에서 변수는 오직 하나의 주인 만을 갖기 때문이다.

아래 코드는 동작하지 않는다.

```rs
fn print_country(country_name: String) {
    println!("{}", country_name);
}

fn main() {
    let country = String::from("Austria");
    print_country(country); // We print "Austria"
    print_country(country); // ⚠️ That was fun, let's do it again!
}
```

그 이유는 `country`가 파괴되었기 때문이다. 일단 순서대로 설명을 하자면

- step 1: `country`라는 문자열 변수를 만들었고 그 문자열의 주인은 `country`이다.
- step 2: 그 다음 `country`를 `print_country`에 보낸다. 그런데 `print_country`에서는 `->`가 없어서 아무것도 반환하지 않는다. 그러므로 `country`는 죽는다.
- step 3: 마지막으로 `country`를 `print_country`로 보낸다. 하지만 `country`는 이미 죽었기 때문에 에러가 발생한다

이 에러를 막기 위해 `print_country`에서 `country`를 반환합니다.

```rs
fn print_country(country_name: String) -> String {
    println!("{}", country_name);
    country_name // return it here
}

fn main() {
    let country = String::from("Austria");
    let country = print_country(country); // we have to use let here now to get the String back
    print_country(country);
}

// 출력 결과
//Austria
//Austria
```

이걸 좀더 좋은 방법으로 고치려면 `&`를 붙이면 된다

```rs
fn print_country(country_name: &String) {
    println!("{}", country_name);
}

fn main() {
    let country = String::from("Austria");
    print_country(&country); // We print "Austria"
    print_country(&country); // That was fun, let's do it again!
}
```

이러면 이제부턴 `print_country`에서 string reference를 받게 하고 값을 넘겨 줄 때 reference를 넘겨주면 문제 없이 동작한다.

또 mutable reference로도 보내도 된다.

```rs
fn add_hungary(country_name: &mut String) { // first we say that the function takes a mutable reference
    country_name.push_str("-Hungary"); // push_str() adds a &str to a String
    println!("Now it says: {}", country_name);
}

fn main() {
    let mut country = String::from("Austria");
    add_hungary(&mut country); // we also need to give it a mutable reference.
}

// 결과
//Now it says: Austria-Hungary
```

결론

- `fn function_name(variable: String)`은 변수의 주인은 넘겨주는 거고
- `fn function_name(variable: &String)`은 변수를 빌리는 것이다
- `fn function_name(variable: &mut String)`은 변수를 빌리면서 수정 할 수 있다.

마지막으로 아래 예제를 보자

```rs
fn main() {
    let country = String::from("Austria"); // country is not mutable, but we are going to print Austria-Hungary. How?
    adds_hungary(country);
}

fn adds_hungary(mut country: String) { // Here's how: adds_hungary takes the String and declares it mutable!
    country.push_str("-Hungary");
    println!("{}", country);
}
```

실패 할 것 같지만 잘 동작한다. 이유는 `mut country`는 참조 가 아니기 때문이다.<br>
사실은 나도 잘 몰라서 여기까지만 하겠다.

## Ownership

아래 예제를 보면 잘 실행될 것 같지만 에러가 나게 된다.

```rs
fn main() {
    let s1 = String::from("Hello");
    let s2 = s1;
    println!("{}", s1);
}
```

이 문제는 바로 위에서 배웠던 `Giving references to functions`와 같은 문제이다<br>
이유를 간단하게 설명하자면 `s1`에서 `s2`로 값이 갈 때 copy된게 아닌 move가 됐기 때문이다. `String::from()`으로 변수를 만들면 이 값이 heap에 저장이 되는데 heap에 저장이 된 변수들은 위 예제와 같은 상황이 주어질 때 copy가 아닌 move를 하게 된다. 그러면서 원래 있던 자리인 `s1`은 사라지게 된다.

하지만 move가 되지 않는 type도 있는데 그것들은 simple type들이다<br>
대충 `i32`, `i64`, `str`, `f32`, `bool` 등 여러가지가 있다.

이것들의 type의 크기는 예측하기 쉬워서 stack에 저장이 되는데 stack은 기본적으로 move가 아닌 copy를 한다. 그래서 아래 예제는 정상작동을 한다.

```rs
fn main() {
    let s1 = S10
    let s2 = s1;
    println!("{}", s1);
}
```

마지막으로 `Ownership Rules`을 설명하자면

- Rust에서 각각의 변수들은 owner라 불리는 변수를 가지고 있다.
- 한 번에 하나의 owner만 존재할 수 있다
- scope을 벗어날 때 그 변수는 자동으로 drop된다.

## Variables without values

Rust에는 값이 없는 변수를 만들 수 있다

```rs
fn main() {
    let my_variable; // ⚠️
}
```

하지만 이렇게 변수를 만들면 아직은 사용할 수 없는 상태가 되고 Rust컴파일러는 오류를 계속해서 발생시킨다. 그래도 이것들은 가끔씩은 유용할 수 있다

코드블럭이 있고 그 안에 변수 값이 있고 그 변수를 코드블럭 바깥에서도 사용하고 싶을 때 굉장히 유용하다

```rs
fn loop_then_return(mut counter: i32) -> i32 {
    loop {
        counter += 1;
        if counter % 50 == 0 {
            break;
        }
    }
    counter
}

fn main() {
    let my_number;

    {
        // Pretend we need to have this code block
        let number = {
            // Pretend there is code here to make a number
            // Lots of code, and finally:
            57
        };

        my_number = loop_then_return(number);
    }

    println!("{}", my_number);
}

출력 결과
//100
```

`my_number = loop_then_return(number)`부분을 보면 함수 실행이 끝나고 값을 `my_number`로 넘겨주는 걸 볼 수 있는데 이걸 만약 `let my_number = loop_then_return(number)`로 작성했다면 `my_number`는 코드블럭에서 나가자 마자 사라졌을 것이다

그래서 아래와 같은 것도 할 수 있다.

```rs
fn main() {
    let my_number;
    {
        my_number = 100;
    }

    println!("{}", my_number);
}
```

저 변수 선언은 결국 `let my_number = { 100 };`과 같다.

## Collection types

`Collection types`이라고 하는데 그냥 배열이다. 대신 조건이 있다

- 크기를 변경해서는 안되며,
- 동일한 유형만 포함해야 한다.

배열의 타입은 다음과 같다 `[type; number]` 예를 들어 `["Teemo", "is", "best"]`의 타입은 `[&str; 3]`

동일한 값이 들어있는 배열을 원한다면 다음과 같이 쓸 수 있다. 이건 큰 buffer를 만들 때 사용된다.

```rs
fn main() {
    let my_array = ["a"; 10];
    println!("{:?}", my_array);
}

// 출력 결과
// ["a", "a", "a", "a", "a", "a", "a", "a", "a", "a"]
```

다른 언어 처럼 배열의 위치를 이용해 안에 특정 값을 찾을 수 있다

```rs
fn main() {
    let my_numbers = [0, 10, -20];
    println!("{}", my_numbers[1]); // prints 10
}
```

또 `...`을 사용해서 배열을 자를 수 있다.

```rs
fn main() {
    let array_of_ten = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

    let three_to_five = &array_of_ten[2..5];
    let start_at_two = &array_of_ten[1..];
    let end_at_five = &array_of_ten[..5];
    let everything = &array_of_ten[..];

    println!("Three to five: {:?}, start at two: {:?}, end at five: {:?}, everything: {:?}", three_to_five, start_at_two, end_at_five, everything);
}
```

`...`앞에는 시작 index를, 뒤에는 마지막 index를 적어주면 된다. 아무것도 적지 않으면 시작 index 또는 마지막 index가 된다.

## Vectors

`&str`과 `String`이 같이 있는 것 과 같이 이것도 array와 vectors가 있다.<br>
array는 속도가 빠른 대신 기능이 적고 vectors는 기능이 많은 대신 속도가 느리다.

사용방법은 아래와 같다.

```rs
fn main() {
    let name1 = String::from("Windy");
    let name2 = String::from("Gomesy");

    let mut my_vec = Vec::new();
    // If we run the program now, the compiler will give an error.
    // It doesn't know the type of vec.

    my_vec.push(name1); // Now it knows: it's Vec<String>
    my_vec.push(name2);
}
```

Vector의 타입을 작성하고 싶다면 아래와 같이 하면 된다

```rs
fn main() {
    let mut my_vec: Vec<String> = Vec::new(); // The compiler knows the type
                                             // so there is no error.
}
```

Vector를 만드는 또다른 방법은 `vec!`를 사용하는 것이다. 배열 앞에 `vec!`를 적어주면 Vector가 된다.

```rs
fn main() {
    let mut my_vec = vec![8, 10, 10];
}
```

위 예제의 vector의 타입은 `Vec<i32>`이고 읽는 법은 "백터의 i32" 이다. `Vec<String>`은 "백터의 string"이라 부른다 또 `Vec<Vec<String>>`는 "백터의 백터의 string"이라 부른다

백터도 배열과 같이 자를 수 있다

```rs
fn main() {
    let vec_of_ten = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    // Everything is the same as above except we added vec!.
    let three_to_five = &vec_of_ten[2..5];
    let start_at_two = &vec_of_ten[1..];
    let end_at_five = &vec_of_ten[..5];
    let everything = &vec_of_ten[..];

    println!("Three to five: {:?},
start at two: {:?}
end at five: {:?}
everything: {:?}", three_to_five, start_at_two, end_at_five, everything);
}
```

백터는 용량을 가지고 있는데 주어진 용량을 초과하면 자동으로 용량이 올라간다.<br>
그 용량은 `capacity`로 알 수 있다.

```rs
fn main() {
    let mut num_vec = Vec::new();
    println!("{}", num_vec.capacity()); // 0 elements: prints 0
    num_vec.push('a'); // add one character
    println!("{}", num_vec.capacity()); // 1 element: prints 4. Vecs with 1 item always start with capacity 4
    num_vec.push('a'); // add one more
    num_vec.push('a'); // add one more
    num_vec.push('a'); // add one more
    println!("{}", num_vec.capacity()); // 4 elements: still prints 4.
    num_vec.push('a'); // add one more
    println!("{}", num_vec.capacity()); // prints 8. We have 5 elements, but it doubled 4 to 8 to make space
}

/*출력 결과
0
4
4
8
*/
```

한 번에 용량을 크게 잡을 수 있다.

```rs
fn main() {
    let mut num_vec = Vec::with_capacity(8); // Give it capacity 8
    num_vec.push('a'); // add one character
    println!("{}", num_vec.capacity()); // prints 8
    num_vec.push('a'); // add one more
    println!("{}", num_vec.capacity()); // prints 8
    num_vec.push('a'); // add one more
    println!("{}", num_vec.capacity()); // prints 8.
    num_vec.push('a'); // add one more
    num_vec.push('a'); // add one more // Now we have 5 elements
    println!("{}", num_vec.capacity()); // Still 8
}
```

이렇게 하면 재할당이 없어서 더 빠르게 백터를 사용할 수 있습니다

## Tuples

Rust에서는 `()`를 사용해 tuple을 사용할 수 있다. 우리는 이미 많은 tuple을 봤는데 함수 앞에 있는 `()`가 tuple이다. 그래서 저 tuple은 빈 tuple로 아무것도 들어 있지 않는다.

```rs
fn do_something() {}
```

위 아래 함수는 같은 것을 반환한다

```rs
fn do_something() -> () {}
```

tuple 안에는 서로 다른 타입들이 들어갈 수 있고 0, 1, 2로 참조가 가능 하다.

```rs
fn main() {
    let random_tuple = ("Here is a name", 8, vec!['a'], 'b', [8, 9, 10], 7.7);
    println!(
        "Inside the tuple is: First item: {:?}
Second item: {:?}
Third item: {:?}
Fourth item: {:?}
Fifth item: {:?}
Sixth item: {:?}",
        random_tuple.0,
        random_tuple.1,
        random_tuple.2,
        random_tuple.3,
        random_tuple.4,
        random_tuple.5,
    )
}
```

출력 결과

```
Inside the tuple is: First item: "Here is a name"
Second item: 8
Third item: ['a']
Fourth item: 'b'
Fifth item: [8, 9, 10]
Sixth item: 7.7
```

그래서 `random_tuple`의 타입은 `(&str, i32, Vec<char>, char, [i32; 3], f64)`이다.

또한 다음과 같은 것도 할 수 있습니다

```rs
fn main() {
    let str_vec = vec!["one", "two", "three"];

    let (a, b, c) = (str_vec[0], str_vec[1], str_vec[2]); // call them a, b, and c
    println!("{:?}", b);
}

//결과
//two
```

a, b, c로 받을 때 이중에 필요없는 값이 있다면 `_`로 받으면 된다

```rs
fn main() {
    let str_vec = vec!["one", "two", "three"];

    let (_, _, variable) = (str_vec[0], str_vec[1], str_vec[2]);
}
```

## Control flow

`Control flow`는 코드가 어떤 상황인지 말해준다 즉 `if`문 같은 걸 말한다.

```rs
fn main() {
    let my_number = 5;
    if my_number == 7 {
        println!("It's seven");
    }
}
```

c에서 배운 if문에서 소괄호만 없다고 생각하면 된다. 이거 말고 다른 것도 있는데<br>
`match` 라고 c언어의 `swich case` 격인 문법이 있다

```rs
fn main() {
    let my_number: u8 = 5;
    match my_number {
        0 => println!("it's zero"),
        1 => println!("it's one"),
        2 => println!("it's two"),
        _ => println!("It's some other number"),
    }
}
```

`my_number`가 `0` 일때, `1` 일때, `2`일때를 적었는데 마지막은 `_`인데 이건 `default`로 위에 해당하는 게 아무것도 없을 때 최종적으로 들어온다. 이게 없으면 에러를 낸다.

`match`를 사용해서 변수를 만들 수 있다.

```rs
fn main() {
    let my_number = 5;
    let second_number = match my_number {
        0 => 0,
        5 => 10,
        _ => 2,
    };
}
```

위 예제를 보면 `my_number`가 5이기 때문에 match에서는 10을 반환하게 되고 second_number는 10이 된다.

아래 예제처럼 tuple도 받을 수 있다

```rs
fn main() {
    let sky = "cloudy";
    let temperature = "warm";

    match (sky, temperature) {
        ("cloudy", "cold") => println!("It's dark and unpleasant today"),
        ("clear", "warm") => println!("It's a nice day"),
        ("cloudy", "warm") => println!("It's dark but not bad"),
        _ => println!("Not sure what the weather is."),
    }
}
```

`match` 안에 `if`를 넣을 수도 있다

```rs
fn main() {
    let children = 5;
    let married = true;

    match (children, married) {
        (children, married) if married == false => println!("Not married with {} children", children),
        (children, married) if children == 0 && married == true => println!("Married but no children"),
        _ => println!("Married? {}. Number of children: {}.", married, children),
    }
}
```

또한 저 부분에 \_를 넣고 싶은 만큼 넣을 수 있다

```rs
fn match_colours(rbg: (i32, i32, i32)) {
    match rbg {
        (r, _, _) if r < 10 => println!("Not much red"),
        (_, b, _) if b < 10 => println!("Not much blue"),
        (_, _, g) if g < 10 => println!("Not much green"),
        _ => println!("Each colour has at least 10"),
    }
}

fn main() {
    let first = (200, 0, 0);
    let second = (50, 50, 50);
    let third = (200, 50, 0);

    match_colours(first);
    match_colours(second);
    match_colours(third);

}
```

출력 결과

```
Not much blue
Each colour has at least 10
Not much green
```

하지만 `match`에는 반환 타입이 전부 같아야 한다

```rs
fn main() {
    let my_number = 10;
    let some_variable = match my_number {
        10 => 8,
        _ => "Not ten", // ⚠️
    };
}
```

아래와 같은 예제도 마찬가지다

```rs
fn main() {
    let my_number = 10;
    let some_variable = if my_number == 10 { 8 } else { "something else "}; // ⚠️
}
```

하지만 아래 if문은 에러가 나지 않는다.<br>
이유는 위 예제에서는 if문에서 바록 값을 반환해서 그러고 아래는 if문에서 반환하지 않고 변수에 저장해서 이다.

```rs
fn main() {
    let my_number = 10;

    if my_number == 10 {
        let some_variable = 8;
    } else {
        let some_variable = "Something else";
    }
}
```

여기서 `@`도 사용할 수 있는데 왜 사용하는지는 잘 모르겠지만 사용 방법은 아래와 같다

```rs
fn match_number(input: i32) {
    match input {
    number @ 4 => println!("{} is an unlucky number in China (sounds close to 死)!", number),
    number @ 13 => println!("{} is unlucky in North America, lucky in Italy! In bocca al lupo!", number),
    _ => println!("Looks like a normal number"),
    }
}

fn main() {
    match_number(50);
    match_number(13);
    match_number(4);
}
```

출력 결과

```
Looks like a normal number
13 is unlucky in North America, lucky in Italy! In bocca al lupo!
4 is an unlucky number in China (sounds close to 死)!
```

`input` 값을 `number`에 넣어 안에서 사용할 수 있다.

## Structs

`structs`는 `struct`라는 키워드로 만들 수 있으며 structs 이름은 꼭 `UpperCamelCase`를 사용해야 한다. 만약 그렇게 사용하지 않으면 컴파일러가 경고를 띄울 것이다.

struct에는 3가지 타입이 있는데 첫 번째는 `unit struct`이다. "unit의 의미는 아무것도 갖고 있지 않다." 라는 뜻으로 `unit struct`를 생성할 때 아래와 같이 해주면 된다.

```rs
struct FileDirectory;
fn main() {}
```

다음은 `tuple struct`또는 `unnamed struct`이고 `unnamed struct`인 이유는 오직 타입만 적고 이름은 적지 않기 때문이다. `tuple struct`는 `simple struct`와 struct의 이름을 기억할 필요가 없을 때 좋다

```rs
struct Colour(u8, u8, u8);

fn main() {
    let my_colour = Colour(50, 0, 50); // Make a colour out of RGB (red, green, blue)
    println!("The second part of the colour is: {}", my_colour.1);
}

// 출력 결과
//The second part of the colour is: 0
```

세 번째 타입은 `named struct`로 아마 이게 가장 흔한 struct 일 것이다. 이 struct는 코드블럭 안에 이름과 타입을 적어서 선언한다. 이러한 struct에서는 세미콜론을 적지 않는다.

```rs
struct Colour(u8, u8, u8); // Declare the same Colour tuple struct

struct SizeAndColour {
    size: u32,
    colour: Colour, // And we put it in our new named struct
}

fn main() {
    let my_colour = Colour(50, 0, 50);

    let size_and_colour = SizeAndColour {
        size: 150,
        colour: my_colour
    };
}
```

`named struct`에서는 콤마에 의해 필드가 나눠진다

```rs
struct Colour(u8, u8, u8); // Declare the same Colour tuple struct

struct SizeAndColour {
    size: u32,
    colour: Colour, // And we put it in our new named struct
}

fn main() {}
```

하지만 struct의 마지막 필드의 콤마는 굳이 쓸 필요는 없다. 그래도 나중에 아래에 필드를 계속 추가할 것이다 마지막 콤마는 찍어주는 게 좋다.

```rs
struct Colour(u8, u8, u8); // Declare the same Colour tuple struct

struct SizeAndColour {
    size: u32,
    colour: Colour // No comma here
}

fn main() {}
```

만약 마지막 필드가 아닌 곳에 콤마가 없다면 에러가 나게 된다

```rs
struct SizeAndColour {
    colour: Colour // ⚠️ Whoops! Now this doesn't have a comma.
    size: u32,
}

fn main() {}
```

예를 들어 `Country` struct를 만든다면 안에 `인구`, `수도`, `대통령` 같은 것들이 들어갈 것이다.

```rs
struct Country {
    population: u32,
    capital: String,
    leader_name: String
}

fn main() {
    let population = 329_500_000;
    let capital = String::from("USA");
    let leader_name = String::from("Joe Biden");

    let USA = Country {
        population: population,
        capital: capital,
        leader_name: leader_name,
    };

    println!("{:?}", USA);
}
```

그런데 여기서 필드 명과 변수명이 같으면 그냥 적을 수 있다

```rs
struct Country {
    population: u32,
    capital: String,
    leader_name: String
}

fn main() {
    let population = 500_000;
    let capital = String::from("Elista");
    let leader_name = String::from("Batu Khasikov");

    let kalmykia = Country {
        population,
        capital,
        leader_name,
    };
}
```

## Enums

`enum`은 짧은 목록이다. 그냥 봤을 때는 `struct`와 비슷해 보이지만 둘은 서로 다르다

`struct`는 많은 것들과 함께 사용하는 반면 `enum`은 많은 선택을 함께 사용합니다.

`enum`을 사용하려면 `enum`이라는 키워드를 적고 코드블럭 안에 콤마로 나누어서 단어를 적으면 된다

```rs
enum ThingsInTheSky {
    Sun,
    Stars,
}

fn main() {}
```

아래는 enum을 사용한 예제입니다

```rs
// create the enum with two choices
enum ThingsInTheSky {
    Sun,
    Stars,
}

// With this function we can use an i32 to create ThingsInTheSky.
fn create_skystate(time: i32) -> ThingsInTheSky {
    match time {
        6..=18 => ThingsInTheSky::Sun, // Between 6 and 18 hours we can see the sun
        _ => ThingsInTheSky::Stars, // Otherwise, we can see stars
    }
}

// With this function we can match against the two choices in ThingsInTheSky.
fn check_skystate(state: &ThingsInTheSky) {
    match state {
        ThingsInTheSky::Sun => println!("I can see the sun!"),
        ThingsInTheSky::Stars => println!("I can see the stars!")
    }
}

fn main() {
    let time = 8; // it's 8 o'clock
    let skystate = create_skystate(time); // create_skystate returns a ThingsInTheSky
    check_skystate(&skystate); // Give it a reference so it can read the variable skystate
}

```

> 결과<br>
> I can see the sun!

다음 예제는 Mood라는 enum에서 ::을 사용해 enum 안에 값을 가져올 수 있는데 match를 통해서 swich case 같은 걸 만들었다

```rs
enum Mood {
    Happy,
    Sleepy,
    NotBad,
    Angry,
}

fn match_mood(mood: &Mood) -> i32 {
    let happiness_level = match mood {
        Mood::Happy => 10, // Here we type Mood:: every time
        Mood::Sleepy => 6,
        Mood::NotBad => 7,
        Mood::Angry => 2,
    };
    happiness_level
}

fn main() {
    let my_mood = Mood::NotBad;
    let happiness_level = match_mood(&my_mood);
    println!("Out of 1 to 10, my happiness is {}", happiness_level);
}
```

하지만 이것 보다 더 쉽게 쓸 수 있는데

`*`을 사용해서 Mood 안에 있는 값들을 전부 가져올 수 있다.

```rs
enum Mood {
    Happy,
    Sleepy,
    NotBad,
    Angry,
}

fn match_mood(mood: &Mood) -> i32 {
    use Mood::*; // We imported everything in Mood. Now we can just write Happy, Sleepy, etc.
    let happiness_level = match mood {
        Happy => 10, // We don't have to write Mood:: anymore
        Sleepy => 6,
        NotBad => 7,
        Angry => 2,
    };
    happiness_level
}

fn main() {
    let my_mood = Mood::Happy;
    let happiness_level = match_mood(&my_mood);
    println!("Out of 1 to 10, my happiness is {}", happiness_level);
}
```

> 결과<br>
> 0<br>
> 1<br>
> 2<br>
> 3

enum에는 다른 숫자도 줄 수 있다

```rs
enum Star {
    BrownDwarf = 10,
    RedDwarf = 50,
    YellowStar = 100,
    RedGiant = 1000,
    DeadStar,
}

fn main() {
    use Star::*;
    let starvec = vec![BrownDwarf, RedDwarf, YellowStar, RedGiant];
    for star in starvec {
        match star as u32 {
            size if size <= 80 => println!("Not the biggest star."),
            size if size >= 80 => println!("This is a good-sized star."),
            _ => println!("That star is pretty big!"),
        }
    }
    println!("What about DeadStar? It's the number {}.", DeadStar as u32);
}
```

```
결과
Not the biggest star.
Not the biggest star.
This is a good-sized star.
This is a good-sized star.
What about DeadStar? It's the number 1001.
```

`DeadStar`의 값은 `RedGiant`의 다음 값인 1001이 된다.
