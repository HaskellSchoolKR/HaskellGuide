## 하스켈 개발환경
하스켈 프로그래밍을 하기 위해선 먼저 컴파일러인 GHC, 패키지 설치 및 빌드를 위한 cabal-install과 Stack, LSP(language server protocol) 구현체인 HLS(haskell-language-server)를 설치해야 합니다. (HLS의 설치는 필수적이지 않습니다.)

설치에는 여러가지 방법이 있지만 GHCup을 이용하는 것이 공식적으로 추천되는 방법입니다.
공식 홈페이지를 참고하세요 : <https://www.haskell.org/downloads/>

### GHCup
GHCup은 비교적 최근에 개발된 하스켈 설치도구로, 이를 통해 GHC, cabal-install, Stack, HLS를 설치할 수 있습니다.

Linux, macOS, FreeBSD, WSL2 등에서 GHCup을 설치하려면 다음 명령을 일반 사용자로 실행하세요
```sh
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

Windows 환경에서 GHCup을 설치하려면 PowerShell에서 다음 명령을 관리자 권한 없이 실행 하세요
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force;[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; try { Invoke-Command -ScriptBlock ([ScriptBlock]::Create((Invoke-WebRequest https://www.haskell.org/ghcup/sh/bootstrap-haskell.ps1 -UseBasicParsing))) -ArgumentList $true } catch { Write-Error $_ }
```

자세한 내용은 GHCup 홈페이지를 참고하세요 : <https://www.haskell.org/ghcup/>

### Nix
NixOS에선 GHCup을 사용할 수 없으며, 대신 nix를 사용해 하스켈 개발환경을 구성할 수 있습니다.
현재 두가지 방법이 있습니다.
* nixpkgs : <https://haskell4nix.readthedocs.io/>
* haskell.nix : <https://input-output-hk.github.io/haskell.nix/>

### 주의사항
* Haskell Platform은 더이상 사용되지 않는 설치방법입니다: <https://www.haskell.org/platform/>
