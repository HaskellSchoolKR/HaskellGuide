# Day 1

## 동기 부여

### 하스켈의 특징
* 순수 함수형     -> 함수는 IO를 할 수 없다
* 고차 함수       -> 함수를 입력으로 받거나 결과로 주는 함수를 정의할 수 있다
* 느긋한 평가     -> 필수적인 계산만 수행, 공유의 적극적 활용
* 정적 타입       -> 런타임 타입 에러가 발생하지 않는다
* 타입 추론       -> 타입은 필요할 때만 명시하면 충분하다
* 대수적 자료형   -> 튜플, 합 타입, 재귀적 타입
* 매개변수 다형성 -> 더 좋은 *Java Generic*
* 타입클래스      -> 더 좋은 *Java Interface*

### 구현상의 특징 (GHC)
* 네이티브 컴파일, 쓰레기 수집기 탑재
* Java와 비슷한 수준의 퍼포먼스
* 동시성 & 병렬성 지원
* Software Transactional Memory

### 하스켈을 사용할 이유
* 고수준의 추상화로 인해 사용성이 좋고, 견고한 프로그램을 작성할 수 있다.
* 추상화 수준에 비해 높은 퍼포먼스를 보여준다.
* 언어의 특성상 Java와 비슷한 포지션으로 사용하기에 적합하다.

### 하스켈을 배울 이유
하스켈을 실무에서 사용하기 힘든 상황이더라도, 하스켈을 배워볼 이유는 충분하다.
"언어가 사고를 지배한다"라는 말이 있듯이 여러가지 프로그래밍 언어, 특히 다른 **패러다임**의 프로그래밍 언어를 배우면 보다 폭넓은 사고를 할 수 있다.
하스켈은 전통적인 OOP 언어에 없는 여러 개념을 사용하는 언어이므로 얻어갈 것이 많다.

## GHC/GHCi
하스켈을 배우기에 앞서 컴파일러와 REPL 사용법을 간단하게 알아보자.

### GHC (compiler)
작업 디렉토리에 `Main.hs` 파일을 만들고 다음 내용을 입력한다.
```haskell
main = putStrLn "Hello, World!"
```

다음과 같이 컴파일 및 실행을 할 수 있다.
```
$ ghc Main.hs -o Main
$ ./Main
Hello, World!
```

### GHCi (repl)
GHC는 네이티브 컴파일러지만 REPL 기능 또한 탑재하고 있다. `ghci`를 입력해 GHCi를 실행해 보자. GHC 9.0.2 에서는 다음과 같은 출력이 나온다.
```
$ ghci
GHCi, version 9.0.2: https://www.haskell.org/ghc/  :? for help
ghci> 
```

GHCi 명령행에서 아래와 같이 코드를 실행해볼 수 있다.
```
ghci> putStrLn "Hello, World"
Hello, World
ghci> x = 3
ghci> x^2
9
```

파일에 저장된 내용을 읽어오고 싶을 때에는 `:load`(단축형 `:l`)을 사용한다.
```
ghci> :l Main.hs
[1 of 1] Compiling Main             ( Main.hs, interpreted )
Ok, one module loaded.
ghci> 
```

로딩 되어있는 파일의 수정사항을 반영하고 싶을 때에는 `:reload`(단축형 `:r`)을 사용할 수 있다. 이때 REPL의 상태가 완전히 초기화 된다.
```
ghci> :r
[1 of 1] Compiling Main             ( Main.hs, interpreted )
Ok, one module loaded.
```

GHCi의 나머지 사용법은 앞으로 틈틈이 설명할 것이다.

## 기초 예제 비교
하스켈과 파이썬의 코드를 비교하며 기본적인 문법에 대해 알아보자.

### Hello, World!
```haskell
main = putStrLn "Hello, World!"
```
```python
def main():
  print("Hello, World!")
```
* `main`은 하스켈 프로그램의 진입점이다.
* 하스켈에서 대상을 정의할 때에는 등호 `=`를 사용한다.
* `putStrLn`은 문자열 출력을 수행한다.

단순한 문법적 차이만이 있어보인다. 다른 예시를 더 비교해보자.

### Exam score
```haskell
{- This program prints whether you passed the exam or not -}
main :: IO ()
main = do
  putStrLn "What is your name?"
  name <- getLine
  putStrLn "What is your score? (0-100)"
  score <- readLn :: IO Int -- read score as an integer value
  if score >= 70 then
    putStrLn (name ++ " passed the exam")
  else
    putStrLn (name ++ " failed the exam")
```
```python
# This program prints whether you passed the exam or not
def main():
  print("What is yout name?")
  name = input()
  print("What is your score? (0-100)")
  score = int(input()) # read score as an integer value
  if score >= 70:
    print(name + " passed the exam")
  else:
    print(name + " failed the exam")
```
* `{- comment -}`나 `-- comment` 형태로 주석을 달 수 있다.
* 여러 동작들을 순차적으로 실행할 때에는 `do` 키워드를 사용한다.
* `getLine`은 입력을 문자열로 읽을 때, `readLn`은 입력을 지정한 타입으로 읽을 때 사용할 수 있다.
* 외부에서 읽어온 값은 `<-`를 통해 변수에 대입한다. 함수 등의 정의에 사용되는 `=`와는 구분된다.
* `::`는 타입을 명시하는데에 사용된다. 컴파일러가 자동으로 타입을 추론하지 못한 경우, 혹은 추론이 가능하더라도 타입 명시를 통해 가독성을 높이고 싶은 경우에 사용된다.
* 조건문은 `if .. then .. else ..` 형태로 쓴다.

### Circle area
```haskell
circleArea :: Double -> Double
circleArea r = pi * r^2

main :: IO ()
main = do
  putStr "radius : "
  r <- readLn :: IO Double
  putStrLn ("The area of the circle is " ++ show (circleArea r))
```
```python
def circle_area(r):
  return math.pi * r**2

def main():
  r = float(input("radius : "))
  print("The area of the circle is " + str(circle_area(r)))
```
* 하스켈에선 함수를 정의하거나 사용할 때 괄호를 붙일 필요가 없다. 즉 `f(x)` 대신 `f x`와 같은 표기를 사용한다.
* 함수를 정의할 때에는 `f x = y`와 같은 문법을 사용한다. `f`는 함수이름, `x`는 함수의 인자, `y`는 함숫값이다.
* 함수를 사용할 때에는 `f x`와 같은 표기를 사용한다.
* 함수의 타입은 `A -> B` 꼴이며 `A`는 인자의 타입, `B`는 결과값의 타입이다.
* `main`의 타입에는 화살표가 없음을 알 수 있다. 사실 `main`은 함수가 아니라 IO action이라 불리는 일종의 프로시저이다. 하스켈에서는 함수와 프로시저의 개념을 엄격하게 구분한다.

### Quadratic formula
```haskell
quadRoots :: Double -> Double -> Double -> (Double, Double)
quadRoots a b c = (x1, x2)
  where
    d = b ^ 2 - 4 * a * c
    x1 = (- b + sqrt d) / (2 * a)
    x2 = (- b - sqrt d) / (2 * a)

-- quadRoots 1.0 0.0 (-4.0) == (2.0, -2.0)
```
```python
def quad_roots(a,b,c):
  d = b ** 2 - 4 * a * c
  x1 = (- b + math.sqrt(d)) / (2 * a)
  x2 = (- b - math.sqrt(d)) / (2 * a)
  return (x1, x2)
```
* 함수의 인자가 여러개인 경우에는 인자들을 순서대로 나열하면 된다. `quadRoots a b c = ...`는 `a`, `b`, `c` 세개의 인자를 받는 함수이다.
* 다인자 함수의 타입은 `A1 -> A2 -> ... -> An -> B` 와 같이 쓴다. `A1`..`An`은 n개 인자의 타입이며, `B`가 결과값의 타입이다. 이 이상한 타입 표기법에 대해서는 곧 자세히 설명할 것이다.
* 튜플은 `(x, y)`와 같이 쓴다. 튜플의 타입도 `(A, B)` 처럼 같은 표기를 사용한다.
* 중간 계산결과는 `where` 구문을 사용해 변수에 대입할 수 있다. `where` 구문에서 정의들이 나열된 순서는 중요하지 않다. 하스켈의 함수는 계산을 순차적으로 수행하지 않는다.

## 주제

### 자료형
정수/부동소수점수/문자/불
```haskell
m :: Int
m = 15

n :: Integer
n = 2^80

f :: Float
f = 3.14

d :: Double
d = 1.4142

c :: Char
c = 'A'

b :: Bool
b = True
```
* `Integer`는 big integer를 표현할 수 있는 자료형 이며, `Int`는 고정된 사이즈를 (주로 64bit) 가진다.
* `Float`과 `Double`은 흔히 사용되는 단정밀도/배정밀도 부동소수점수 자료형이다.
* `Char` 타입은 유니코드 문자를 한개 저장할 수 있다.



### 타입, 패턴매칭, 람다식

### Partial application, Currying
하스켈의 모든 함수는 단인자로, 인자를 하나만 받는 함수이다.
다행히도 튜플을 사용하면 단인자 함수로 다인자 함수를 쉽게 흉내낼 수 있다.
예를 들어 두 정수의 합을 계산하는 `add`라는 2인자 함수의 타입은 정수 두개로 이루어진 튜플을 인자로 받는 1인자 함수로 표현할 수 있다.
```haskell
add :: (Int, Int) -> Int
add (x, y) = x + y
```



### IO action
