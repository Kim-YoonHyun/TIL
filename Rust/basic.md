## 설치

1. rustup 설치
   ```cmd
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

2. 터미널 재시작
   ```cmd
   source ~/.cargo/env  
   ```

3. 설치 확인
   ```cmd
   rustc --version
   cargo --version
   ```

## 실행

1. Cargo.toml 생성&설정
   ```toml
   [package]
   name = "temp_rs"	# python 에서 import 할때의 명칭
   version = "0.1.0"
   edition = "2021"
   
   [lib]
   crate-type = ["cdylib"]  # Python에서 import 가능하도록 설정
   
   [dependencies]
   pyo3 = { version = "0.20", features = ["extension-module"] }
   
   ```

   ```python
   import temp_rs 로 사용가능케 함
   ```

2. maturin 설치

   - 명령어를 실행하는 작업 디렉토리에 Cargo.toml 이 존재해야함
   - ananconda 가상환경에 rust 설치는 독립적으로 취급되기에 따로 설치 필요

   ```cmd
   pip install maturin
   ```

3. 개발 모드로 설치
   ```cmd
   maturin develop
   ```

4. pyproject.toml

   python 패키지 내부에 rust 모듈이 있을때 maturin develop 을 하는 경우, pyproject.toml 가 상위로 잡혀버려서 이곳에 설정되어있는 [project] name 에 rust 모듈이 종속되어버린다. 이를 해결하기 위해선 따로 설정을 추가해줘야함.
   ```toml
   [build-system]
   # 빌드 시스템 의존성 정의 (패키지를 빌드하기 위해 필요한 것들)
   # requires = ["setuptools>=61.0", "wheel"]
   # build-backend = "setuptools.build_meta"
   # 위는 순수 python 모듈일때만 적용 가능하고 rust 존재시 아래 설정으로 변경
   # 이 변경사항은 python 패키지 build 에 영향을 미치지 않음
   requires = ["maturin>=1.4"]
   build-backend = "maturin"
   
   [project]
   name = "mpalp" # 패키지 이름 (pip install 시 사용될 이름)
   version = "0.1.5" # 버전
   
   [tool.maturin]
   module-name = "mpalp.validator"
   version = "0.1.0"
   ```

   

5. 파이썬에서 사용


1. ```python
   import temp_rs
   
   """
   code...
   """
   ```

## 에러

maturin 을 통해 에러 발생시

```cmd
cargo build
```

를 통해 정확한 에러 원인 규명



```cmd
maturin develop
```



예시 코드

```rust
let batch_size = match py_dict.get_item('batch_size') {
    Some(value) => value.extract::<u32>()
        .map_err(|_| PyErr::new::<pyo3::exceptions::PyValueError, _>(
            "batch_size must be a positive integer",
        ))?,
    None => {
        return Err(PyErr::new::<pyo3::exceptions::PyKeyError, _>(
            "missing required key: 'batch_size'",
        ))
    }
```



## 표현식

### Option\<T>

값이 있는지 없는지 여부를 파악. 값이 존재하면 Some(T) 를 반환한다.
값이 없는 경우 None 을 반환하는데 이는 **값이 없다** 는 하나의 정상적 결과일뿐 에러나 실패를 의미하는 것이 아니다.
즉, Option\<T> 표현식 자체에는 **"실패" 라는 개념이 존재하지 않는다.**

Option\<T> 타입의 값은 **반드시** Some(T) 와 None 중 하나의 형태로만 존재한다.

```rust
let x: Option<i32>
```

x 에 대해 이렇게 Option 을 선언한다면

x 는 Some(T) 또는 None 중 하나만을 가질 수 있게 된다.

상세 설명 예시:

```rust
let x: Option<i32> = Some(10)
```

- x 의 런타임 값은 Some(10)이다
- **x 의 값이 10 을 의미하는 것이 아니다**
- x는 겉면에 "정수 값이 들어있을수도 없을 수도 있다" 고 라벨을 붙여놓았지만 **컴파일러는** 그 상자를 직접 열어보기 전에는 "진짜로" 값이 들었는지 비었는지 알수 없는 "미개봉 상자" 이다
- 상자를 열어서 확인했을때 값이 들어있는 것이 증명되었다면 그때 그 값인 10을 활용 가능하게 된다.
- 열어서 확인해보고 그 값이 "존재한다"고 증명되기 전 까지는 그 정수값이 정말로 10 이냐 아니냐는 논제는 컴파일러가 애초에 완전 배제한다.

상자에 진짜로 값이 들었는지를 증명하는 논리 설명 예시:

```rust
match x {
    Some(v) => {
        println!("{}", v);
    }
    None => {}
}
```

- match 를 통해 Some(v) 를 판단하는 것을 현실+파이썬의 감각으로 풀어 적자면
  *x 가 "어떤값(v)이든 담고있는 나무상자(Some)" 라면 그 안에 있는 값(10)을 쓸수 있다.
   그런데 보니까 x 는 아예 열어본다는 개념 자체가 없는 단단한 네모 플라스틱 덩어리(None) 이라 아무것도 할 수 없다. (이때 플라스틱 상자는 그냥 나무가 아니다 라는걸 명사적으로 표현하기 위한 선택일 뿐임)*
  로 볼 수 있다.
  파이썬으로 굳이 표현하면

  ```python
  x = ("Some", 10)
  if x[0] == "Some":
      v = x[1]
      print(v)
  else:
      pass
  ```

  로 볼 수 있다.

- 즉 None 이라는 것은 "상자를 열어봤더니 값이 없더라" 를 의미하는 것이 아니라 "애초에 열어볼 수 있는 상자조차 아니었다" 를 의미한다.

- 또한 "상자를 열어봤더니 값이 없더라" 의 논리는 기본적으로 **존재하지 않는다.** 이를 표현하려면 `Some(v) if v.is_empty()=>` 같은 레벨을 추가로 넣어서 "상자는 있는데 안이 비었더라" 라는 개념을 아예 추가해야한다.

  









- **match, Some(value), None 는 문법적인 한 세트가 아니다.** Some 이 Option 하고는 반드시 같이 써야하는 것이 맞지만 Option 자체는 다양한 객체와 같이 쓸 수 있음

  ```rust
  match value.extract::<u32>() {
      Ok(v) => {
          println!("변환 성공: {}", v);
          v
      }
      Err(e) => {
          println!("변환 실패: {}", e);
          return Err(PyErr::new::<pyo3::exceptions::PyValueError, _>(
              "batch_size must be a positive integer",
          ));
      }
  }
  ```

  ```rust
  let pair = (1, 2);
  match pair {
      (0, y) => println!("x=0, y={}", y),
      (x, 0) => println!("y=0, x={}", x),
      _ => println!("둘 다 0 아님"),
  }
  ```

  ```rust
  enum Token {
      Number(i32),
      Plus,
      Minus,
  }
  
  match token {
      Token::Number(n) => println!("숫자 {}", n),
      Token::Plus => println!("더하기 기호"),
      Token::Minus => println!("빼기 기호"),
  }
  ```

### Result\<T, E>

성공 또는 실패.

`try/except` 와 비슷함

```rust
value.extract::<u32>()
.map_err(|_| PyErr::new::<pyo3::exceptions::PyValueError, _>(
    "batch_size must be a positive integer",
))?    
// value 의 타입을 Rust u32 타입으로 변환 진행. 
```

- `value.extract::<u32>()` 'value (예시 기준 python dict 의 "epochs" 객체) 에 대한 Rust u32 타입 변환 시도' 자체를 의미하는 Rust 문법이다
- 변환 성공시 `Ok` 타입이 반환되며 실패시 `Err` 타입이 반환된다.
- `Err` 가 반환될 경우 `.map_err` 에 의해 오류가 덧씌워지며 반환되며 ? 에 의해 바로 반환된다.
- ? 가 없는 경우 return 을 통해 Err 자체를 반환해주는 코드가 필요해진다.
- `Ok` 가 반환될 경우 아무일도 일어나지 않는 것이 아니라 위의 코드 기준 `let batch_size` 에 의해 `batch_size` 변수를 실제로 Rust u32 타입으로 변환 성공한것이 됨. 
- `.map_err` 는 `Ok` 및 `Err` 둘 다에 적용이 되기 때문에 (=Result 반환값에 적용가능) 문법적 오류가 있는 것이 아니며 `Ok.map_err` 의 경우 그냥 아무일도 일어나지 않음

### PyResult\<T>

"이 함수는 Python 예외를 발생시킬 수 있다" 는 의미

*주의* : 마치 PyResult 타입이 T 라는값을 받고 그 성공유무를 판단하는 것처럼 보일 수 있지만 그건 python 적인 시선일 뿐이고 실제로는 PyResult 라는 타입 템플릿에서 T 자리에 어떤 타입이 들어갈지를 지정하는 일종의 **선언** 에 불과하다.

PYO3 에 이미 

```rust
pub type PyResult<T> = Result<T, PyErr>;
```

로 지정되어있기 때문에 PyResult\<T> 자체가 성공시 타입 반환, 실패시 **Python의 예외 타입** 반환을 의미한다.

한마디로 PyResult\<T> 는 "이 Result 는 Python 예외를 던진다" 는 것을 명시적으로 보여주는 것과 다름없다.

PyResult<T, E> 를 하지 않는 이유는 E 를 해놓으면 Python 예외 타입이 아닌 다른 실패 타입을 정할 수 있는 것이 되어버려서 에러가 나기 때문

# 문법

## 기본

### : / :: / _

`:` -> 변수/함수 타입 지정

`::` -> 모듈/타입 경로

`_` -> 타입 추론



### <...>

제네릭 : 타입을 나중에 구체적으로 지정할 수 있도록 미리 일반화 해두는 것

<...> 안에 들어가는 것은 "타입" 을 의미함

| 표시 | 의미 | 설명                          |
| ---- | ---- | ----------------------------- |
| `T`  | 성공 | 성공했을 때의 **값의 타입**   |
| `E`  | 실패 | 실패했을 때의 **에러의 타입** |

즉, T 또는 E 라는 것이 어떤 타입을 정확히 명시한다기 보단 <T, E> 는 성공과 실패의 **2개 반환값** 이 존재하고 그 반환값의 타입은 명확하게 정해진 것이 아니다.
이는 호출할때 자동으로 정하는 것에 가깝다.

```rust
Result<Option<T>, E>
```

위의 방식으로 표현될 경우 이는 성공시 반환되는 값의 타입이 Option\<T> 인 것이고
Result 에 의해 "성공시 Option\<T>", "실패시 E" 타입을 반환하는 것을 결정하고
Result 에 의한 성공 이후 Option\<T> 에 의해 값이 존재하면 Some(T) 반환, 없으면 None 반환 이 된다. 
(이때 Option\<T> 는 존재/존재하지 않음 둘 다 엄연히 성공이며 실패하면 None 반환은 틀린 표현이다.)
(Option\<T> 는 존재 여부만 따지며 "실패" 개념 자체가 존재하지 않음)
즉, 한 문장으로 **"연산이 성공했더라도 값이 있을지 없을지는 모른다"** 라는 논리를 구현한 것이다.

### mut

Rust 는 기본적으로 불변을 중요시한다. 이에 어떤 변수든 그 변수를 가변상태로 두려면 mut 을 활용해야 한다.

```rust
let x = 10;
x += 1; // -> 불가능
```

```rust
let mut x = 10;
x += 1; // --> 가능
```

### 정수형 타입

```rust
i32 // signed 32-bit 정수. -2147483648 ~ 2147483648
u32 // unsigned 32-bit 정수. 0 ~ 2147483648
```



## python 연동

### pyo3

설명

```rust
use pyo3::prelude::*;
// pyo3 에서 자주쓰는 PyResult, PyErr 같은 타입, 매크로를 가져옴
```

```rust
use pyo3::types::{PyAny, PyDict}; 
// PyAny: 임의의 python 객체
// PyDict : python 의 dict 객체
```

### \#[pyclass], \#[pymethods]

| 구분           | 설명                                                  |
| -------------- | ----------------------------------------------------- |
| `#[pyclass]`   | 지정한 Rust 타입을 python Class 로 만듦               |
| `#[pymethods]` | python class 로 지정된 해당 객체에 노출될 메소드 정의 |

`#[pymethods]` 는 `#[pyclass]` 없이는 아예 사용 불가능하며 컴파일 에러가 발생하고 반대는 사용 자체는 가능하지만 메소드가 없으므로 의미가 없음.

```rust
#[pymethods]
impl Foo {
    fn bar(&self) {}
}
// Foo 는 python 클래스가 아니므로 컴파일 에러 발생
```

```rust
#[pyclass]
struct Foo;
// Python 에서 Foo() 를 사용가능하지만 아무런 메소드가 없음
```

- 정석적인 활용 예시

```rust
#[pyclass]
pub struct MyClass {
    value1: i32,  // 필드 선언이기에 문장끝내기(;) 대신 필드구분자(,) 만 활용
}

impl MyClass { // impl 은 이 타입이 할 수 있는 행동을 정의한다는 문법
    fn plus(&mut self) { // 메소드 내 객체 상태 변경 가능
        self.value1 += 1;
    }
}

#[pymethods]
impl MyClass { // impl 을 통해 행동을 구분 가능 
    fn def1(&mut self) {
        self.plus();
    }
}
```

#### self, &self, &mut self

사용 방식의 관점에선 python class 의 self 와 유사하지만 근본적 의미는 다름. 
소유권을 보통 의미하며 객체에 대한 빌리는 규칙의 일부

- &self 

읽기전용 접근. 내부 객체에 대한 변경 및 재할당을 일체 허용하지 않는다.

```rust
impl MyClass {
    fn read(&self) {
        let x = self.value1; // 가능(읽기)
        self.value1 = 2;  // 불가능 --> 컴파일 에러 발생        
    }
}
```

- &mut self

수정 가능 접근. 내부 객체에 대한 변경 및 재할당을 허용한다. </br>
단, 사용중 **다른 참조는 불가능** 하다

```rust
impl MyClass {
    fn read(&mut self) {
        self.value1 = 2; // 가능
	    self.value1 += 1; // 가능        
    }
}

let mut a = MyClass{value1: 10};
let r1 = &mut a;
let r2 = &mut a; // 불가능 --> 동시에 두개 불가. 컴파일 에러 발생
```

- self

소유권 이동 접근. 읽기/쓰기와는 근본적으로 다름

```rust
impl MyClass {
    fn read(self) {
        println!("{}", self.value1);
    }
}

let a = MyClass {value1:10};
a.read();
println!("{}", a.value); // --> 불가능. a 가 더이상 존재하지 않음
```

#### #[new], #[staticmethod]

- `#[new]`
  Rust 에 생성자를 명시적으로 정의 하는 것. 이게 있어야 python 에서 import 했을때 

  ```rust
  impl MyClass {
      #[new]
      fn new(py_dict: &PyDict) -> PyResult<Self> {
          Self::from_dict(py_dict)
      }
  }
  ```

  `a = MyClass(...)` 로 `from_dict` 를 쓸 수 있게 된다.

- `#[staticmethod]`
  단순한 클래스 함수이며 이것만으로는 생성자를 명시적 정의할 수 없음

  ```rust
  impl MyClass {
      #[staticmethod]
      fn from_dict(py_dict: &PyDict) -> PyResult<Self> {...}
  }
  ```

  이 있으면 `MyClass.from_dict(...)` 로 쓸 수 있음. 하지만 `MyClass(...)` 로 바로 쓰는 건 불가능



#### \#[pyo3(get)]

rust 클래스 생성시 python 에서 변수에 접근하려면 반드시 `#[pyo3(get)]` 을 해줘야한다.

```rust
#[pyclass]
pub struct MyClass {
    #[pyo3(get)]
    pub values1: u32,
}
```

### \#[pymodule]

Rust 크레이트를 Python 모듈로 초기화해주는 역할을 하며
이 함수가 있어야만 **python  에서 import 해서 사용가능** 하게 해줌.

```rust
#[pymodule]
fn engine_rs(_py: Python, m: &PyMOdule) -> PyResult<()> {
    m.add_class::<MyClass>()?;
    Ok(())
}
```

### \<'py>

수명 매개변수 선언

```rust
fn to_dict<'py>(&self, py: Python<'py>) -> PyResult<&'py PyDict> {...}
```





