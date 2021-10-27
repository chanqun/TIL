The Rust Programming Language

## 3. 변수

## 4. Understanding Ownership

Ownership enables Rust to make memory safety guarantees without needing a garbage collector.

스택은 데이터에 접근하는 방식 덕택에 빠르다. - 스택을 빠르게 해주는 특성은 스택에 담긴 모든 데이터가 결정되어 있는 고정된 크기이다.

컴파일 타임에 크기가 결정되어 있지 않거나 크기가 변경될 수 있는 데이터를 위에서 힙에 데이터를 저장할 수 있다. 힙 공간을 할당한 포인터는 고정된 크기의 값이므로, 스택에 저장할 수 있다. 힙에 저장된 데이터에
접근하는 것은 스택에 저장된 데이터에 접근하는 것보다 느린데, 그 이유는 포인터가 가리킨 곳을 따라가야 하기 때문이다.

##### 소유권 규칙

1. 러스트의 각각의 값은 해당값의 오너라고 불리우는 변수를 갖고 있다.
2. 한 번에 딱 하나의 오너만 존재할 수 있다.
3. 오너가 스코프 밖으로 벗아나는 때 값은 버려진다.

##### String 타입

```rust
let mut s = String::from("hello");

s.push_str(", world!");
```

##### 메모리와 할당

String 타임은 변경 가능하고 커질 수 있는 텍스트를 지원하기 위해 만들어졌음

1. 런타임에 운영체제로부터 메모리가 요청
2. String의 사용이 끝났을 때 운영체제에게 메모리를 반납해야한다.

-> rust 는 괄호가 닫힐때 자동적으로 drop을 호출

```rust
let s1 = String::from("hello");
let s2 = s1
```

만약 s2 = s1 이 힙 데이터까지 복사한다면 런타임 상에서 느려질 수 있고 스코프를 벗어날 때 두 번 drop을 해야한다. 러스트는 s1에 메모리만 drop하고 s2는 유효하다.

--> 데이터가 스택에 있으면 clone 깊은 복사와 얕은 복사가 차이가 없다. (Copy 트레잇을 갖고 있다.)

스칼라가 값들의 묶음은 Copy가 가능하고 <-> 할당이 필요한 경우 Copy를 사용할 수 없다.

```rust
! ! ! ! !
fn main() {
    let s = String::from("hello");  // s가 스코프 안으로 들어왔습니다.

    takes_ownership(s);             // s의 값이 함수 안으로 이동했습니다...
    // ... 그리고 이제 더이상 유효하지 않습니다.
    let x = 5;                      // x가 스코프 안으로 들어왔습니다.

    makes_copy(x);                  // x가 함수 안으로 이동했습니다만,
    // i32는 Copy가 되므로, x를 이후에 계속
    // 사용해도 됩니다.
} // 여기서 x는 스코프 밖으로 나가고, s도 그 후 나갑니다. 하지만 s는 이미 이동되었으므로,
// 별다른 일이 발생하지 않습니다.

fn takes_ownership(some_string: String) { // some_string이 스코프 안으로 들어왔습니다.
    println!("{}", some_string);
} // 여기서 some_string이 스코프 밖으로 벗어났고 `drop`이 호출됩니다. 메모리는
// 해제되었습니다.

fn makes_copy(some_integer: i32) { // some_integer이 스코프 안으로 들어왔습니다.
    println!("{}", some_integer);
} // 여기서 some_integer가 스코프 밖으로 벗어났습니다. 별다른 일은 발생하지 않습니다.
```

> 어떤 값을 다른 변수에 대입하면 값이 이동된다. 힙에 데이터를 갖고 있는 변수가 스코프 밖으로 벗어나면, 해당 값은 데이터가 다른 변수에 의해 소유되도록 이동하지 않는한
> drop에 의해 제거된다.

#### 참조자(References)와 빌림(Borrowing)

&는 소유권을 넘기지 않고 참조할 수 있도록 해준다.

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);
}

fn calculate_length(str: &String) -> usize {
    str.len()
}
```

소유권을 넘기지 않았기 때문에 s1을 사용할 수 있다.

따라서 s1을 수정하는 것이 불가능한데 (&mut s1) 을 사용하면 값을 바꿀 수 있다. ! 가변참조자 하지만 스코프 내에서 하나만 만들 수 있다.

##### 데이터 레이스

1. 두 개 이상의 포인터가 동시에 같은 데이터에 접근
2. 그 중 적어도 하나의 포인터가 데이터를 씀
3. 데이터에 접근하는데 동기화를 하는 어떠한 메커니즘도 없음

+ 불변 참조자를 가지고 있다면 가변 참조자를 만들 수 있다!

#### 댕글링 참조자(Dangling References)

```rust

fn dangle() -> &String {
    let s = String::from("hello");

    &s
} BAD!

fn dangle() -> String {
    let s = String::from("hello");

    s
}

```

### 슬라이스

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

&str은 불변 참조자


