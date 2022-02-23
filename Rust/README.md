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

### Primitive Type(원시 타입?)

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
