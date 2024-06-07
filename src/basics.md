# 동기 부여

## 하스켈의 특징
* 순수 함수형     -> 함수는 IO를 할 수 없다
* 고차 함수       -> 함수를 입력으로 받거나 결과로 주는 함수를 정의할 수 있다
* 느긋한 평가     -> 필수적인 계산만 수행, 공유의 적극적 활용
* 정적 타입       -> 런타임 타입 에러가 발생하지 않는다
* 타입 추론       -> 타입은 필요할 때만 명시하면 충분하다
* 대수적 자료형   -> 튜플, 합 타입, 재귀적 타입
* 매개변수 다형성 -> 하나의 코드를 여러가지 타입으로 재사용
* 타입클래스      -> 조건부 다형성

## 구현상의 특징 (GHC)
* 네이티브 컴파일, 쓰레기 수집기 탑재
* Java와 비슷한 수준의 퍼포먼스
* 동시성 & 병렬성 지원
* Software Transactional Memory

## 하스켈을 사용할 이유
* 고수준의 추상화로 인해 사용성이 좋고, 견고한 프로그램을 작성할 수 있다.
* 추상화 수준에 비해 높은 퍼포먼스를 보여준다.
* 언어의 특성상 Java와 비슷한 포지션으로 사용하기에 적합하다.

## 하스켈을 배울 이유
하스켈을 실무에서 사용하기 힘든 상황이더라도, 하스켈을 배워볼 이유는 충분하다.
하스켈을 통해 다른 언어에서 경험하기 힘든 프로그래밍 방식을 배울 수 있다.
여러가지 프로그래밍 언어, 특히 다른 패러다임의 프로그래밍 언어를 배우면 보다 폭넓은 사고를 할 수 있다.

# 하스켈 개발환경 설정하기
하스켈 프로그래밍을 하기 위해선 먼저 컴파일러인 GHC, 패키지 설치 및 빌드를 위한 cabal-install과 Stack, LSP(language server protocol) 구현체인 HLS(haskell-language-server)를 설치해야 합니다. (HLS의 설치는 필수적이지 않습니다.)

설치에는 여러가지 방법이 있지만 GHCup을 이용하는 것이 공식적으로 추천되는 방법입니다.
공식 홈페이지를 참고하세요 : <https://www.haskell.org/downloads/>

## GHCup
GHCup은 비교적 최근에 개발된 하스켈 설치도구로, 이를 통해 GHC, cabal-install, Stack, HLS를 설치할 수 있습니다.
아래의 내용은 GHCup 공식 홈페이지에서도 볼 수 있습니다. <https://www.haskell.org/ghcup/>

Linux, macOS, FreeBSD, WSL2 등에서 GHCup을 설치하려면 다음 명령을 일반 사용자로 실행하세요
```sh
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

Windows 환경에서 GHCup을 설치하려면 PowerShell에서 다음 명령을 관리자 권한 없이 실행 하세요
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force;[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; try { Invoke-Command -ScriptBlock ([ScriptBlock]::Create((Invoke-WebRequest https://www.haskell.org/ghcup/sh/bootstrap-haskell.ps1 -UseBasicParsing))) -ArgumentList $true } catch { Write-Error $_ }
```

## Nix
NixOS에선 GHCup을 사용할 수 없으며, 대신 nix를 사용해 하스켈 개발환경을 구성할 수 있습니다.
현재 두가지 방법이 있습니다.
* nixpkgs : <https://haskell4nix.readthedocs.io/>
* haskell.nix : <https://input-output-hk.github.io/haskell.nix/>

## 주의사항
* Haskell Platform은 더이상 사용되지 않는 설치방법입니다: <https://www.haskell.org/platform/>

# 하스켈 써보기
하스켈을 써봅시다!

## GHC (컴파일러)
작업 디렉토리에 `Main.hs` 파일을 만들고 다음 내용을 입력합니다.
```haskell
main = putStrLn "Hello, World!"
```

GHC를 이용해 하스켈 코드를 컴파일 하기 위해서는 명령행에서 `ghc` 명령을 사용합니다.
```
$ ghc Main.hs -o Main
$ ./Main
Hello, World!
```

## GHCi (인터프리터)
GHC는 네이티브 컴파일러이지만 인터프리터 모드로 실행할 수도 있습니다. `ghci`를 입력해 GHCi를 실행해 보세요.
```
$ ghci
GHCi, version 9.0.2: https://www.haskell.org/ghc/  :? for help
ghci> 
```

GHCi의 REPL(readl-eval-print-loop)에서 아래와 같이 간단히 코드를 실행해볼 수 있습니다.
```
ghci> main = putStrLn "Hello, World"
ghci> main
Hello, World
ghci> x = 3
ghci> x^2
9
```

파일에 저장된 내용을 GHCi로 읽어오고 싶을 때에는 `:load`(단축형 `:l`)를 사용합니다.
```
ghci> :l Main.hs
[1 of 1] Compiling Main             ( Main.hs, interpreted )
Ok, one module loaded.
ghci> main
Hello, World
```

로딩 되어있는 파일의 수정사항을 반영하고 싶을 때에는 `:reload`(단축형 `:r`)를 사용할 수 있습니다.
```
ghci> :r
[1 of 1] Compiling Main             ( Main.hs, interpreted )
Ok, one module loaded.
```

## 정의 하기
`Main.hs` 파일을 열고 간단한 코드를 짜봅시다.

등식을 이용해 새로운 정의를 추가할 수 있습니다. 좌변에는 피정의항의 이름이, 우변에는 임의의 표현식이 옵니다.
```haskell
two = 2
answer = 50 - two * 4
```

함수를 정의하기 위해서는 좌변에 함수와 인자의 이름을 나란히 쓰면 됩니다. 수학에서 통용되는 함수 정의의 표기법은 `f(x) = 2*x` 이지만, 하스켈에서는 괄호 대신 공백으로 함수와 인자를 구분합니다.
```haskell
f x = 2 * x
```

인자가 여러개인 함수를 정의하고 싶을 때에는 모든 인자를 공백으로 구분하여 나열하면 됩니다. 아래의 코드는 `x`, `y`, `z` 3개의 인자를 받는 함수 `sum3`를 정의합니다.
```haskell
sum3 x y z = x + y + z
```
