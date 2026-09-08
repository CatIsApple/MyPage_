---
layout: default
title: Executable & Bytecode (실행 파일과 바이트코드)
description: 바이트코드와 실행 파일이 만들어지고 실행되는 과정을 정리한 글입니다.
---

# Executable & Bytecode (실행 파일과 바이트코드)

<sub>PROGRAM EXECUTION FLOW (프로그램 실행 전체 흐름) · 2026.09.06</sub>

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/cover.jpg" alt="Executable &amp; Bytecode (실행 파일과 바이트코드)" width="100%">

## Bytecode 바이트코드

바이트코드는 고수준 언어로 작성한 소스 코드를 컴파일하여 만든 중간 코드입니다. CPU가 직접 이해하는 기계어가 아니라 JVM이나 PVM과 같은 가상 머신이 이해하고 실행할 수 있는 형태입니다.

위키백과에서는 바이트코드를 **특정 하드웨어가 아닌 가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법**으로 설명합니다.

바이트코드는 0과 1로 표현될 수 있지만, 특정 CPU에서 바로 실행되는 기계어와는 다릅니다. 플랫폼에 독립적인 명령 형식이며 이를 이해하는 가상 머신이 필요합니다.

### 왜 사용하는가?

소스 코드는 먼저 컴파일을 거쳐 바이트코드로 변환됩니다. 이후 가상 머신은 바이트코드를 현재 컴퓨터가 이해할 수 있는 기계어로 해석하거나 컴파일하여 실행합니다.

바이트코드를 만드는 과정은 컴파일이지만, 만들어진 바이트코드는 가상 머신에서 해석하여 실행할 수 있습니다. 이 중간 단계를 두면 각 운영체제마다 소스 코드를 다시 작성하지 않아도 되고, 가상 머신을 통해 호환성과 실행 효율을 함께 확보할 수 있습니다.

### JVM이란?

JVM(Java Virtual Machine)은 Java 바이트코드를 실행하는 가상 머신입니다. Java 컴파일러가 만든 `.class` 파일을 읽어 현재 시스템의 기계어로 바꾸고 실행합니다.

JVM은 바이트코드를 메모리로 가져오는 Class Loader, 명령을 해석하거나 JIT 컴파일하는 Execution Engine, 사용하지 않는 메모리를 정리하는 Garbage Collector 등으로 구성됩니다.

Java 프로그램이 운영체제에 직접 맞춰지지 않아도 실행될 수 있는 이유는 JVM이 그 사이에서 공통 인터페이스 역할을 하기 때문입니다. **Write Once, Run Anywhere**라는 Java의 방향도 이 구조를 기반으로 합니다.

### Bytecode를 사용하는 이유가 뭘까?

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/01-intermediate-code.png" alt="Intermediate Code" width="100%">

서로 다른 여러 소스 언어를 여러 하드웨어에 직접 대응시키면 변환기의 조합이 빠르게 늘어납니다. 중간 코드를 사이에 두면 각 언어는 하나의 중간 표현으로 변환하고, 각 실행 환경은 그 중간 표현만 처리하면 됩니다.

이 구조는 언어와 플랫폼 사이의 결합을 줄여 구현을 단순하게 만들고, 중간 단계에서 공통 최적화를 적용할 수 있게 합니다.

### Bytecode의 생성 과정

바이트코드도 최종적으로는 CPU가 이해할 수 있는 기계어로 변환되어야 합니다. 실행할 때마다 가상 머신이 바이트코드를 해석하면 미리 변환된 기계어보다 느릴 수 있습니다.

이 비용을 줄이기 위해 자주 실행되는 코드를 실행 중 기계어로 컴파일하는 JIT(Just-in-Time) 방식을 함께 사용합니다. AOT와 JIT의 차이는 [Compilation & Interpretation (컴파일과 해석)](../compilation-interpretation/)에서 이어서 정리했습니다.

### JVM은 바이트코드를 어떻게 실행할까?

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/02-jvm-overview.png" alt="JDK, JRE and JVM Overview" width="100%">

#### JDK 자바 개발 환경 (Java Development Kit)

JDK는 Java 프로그램을 개발하는 데 필요한 도구와 라이브러리의 모음입니다.

- `javac`: Java 소스 코드를 바이트코드인 `.class` 파일로 컴파일합니다.
- `javap`: 컴파일된 클래스 파일의 구조와 바이트코드를 확인합니다.
- `jdb`: Java 프로그램을 디버깅합니다.
- `jdeps`: 클래스와 모듈 사이의 의존성을 분석합니다.

#### JRE 자바 실행 환경 (Java Runtime Environment)

JRE는 Java 프로그램을 실행하는 데 필요한 환경입니다. `java`, `javaw`와 같은 실행 명령, 클래스 로더, 실행 라이브러리와 JVM을 포함합니다.

Java 9부터는 하나의 거대한 `rt.jar`에 실행 라이브러리를 모으는 대신 모듈 시스템을 사용합니다. 필요한 모듈만 구성할 수 있어 불필요한 클래스 로딩을 줄이고 더 작은 실행 환경을 만들 수 있습니다.

#### JVM 자바 가상 머신 (Java Virtual Machine)

JVM은 바이트코드를 현재 운영체제와 CPU에서 실행할 수 있도록 처리합니다.

- Interpreter와 JIT Compiler
- Class Loader와 Linker
- JVM Instruction Set
- Garbage Collector
- Runtime Data Area

JVM이 운영체제별 차이를 추상화하기 때문에 같은 바이트코드를 서로 다른 환경에서 실행할 수 있습니다.

#### Bytecode 실행 과정

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/03-bytecode-execution.png" alt="Bytecode Execution Process" width="100%">

1. JDK의 컴파일러가 Java 소스 코드를 `.class` 바이트코드로 만듭니다.
2. JRE의 실행 명령이 JVM에 프로그램 실행을 요청합니다.
3. JVM이 클래스를 로드하고 바이트코드를 검증한 뒤 해석하거나 JIT 컴파일합니다.
4. 실제 메모리 할당과 회수, 파일 접근, 시스템 호출은 JVM이 운영체제와 상호작용하며 처리합니다.

---

## Executable 실행 파일

### Executable이 무엇인가?

실행 파일은 단순히 데이터를 보관하는 파일과 달리, 컴퓨터가 수행할 명령을 담은 파일입니다.

명령은 CPU가 직접 수행할 기계어일 수도 있고, 인터프리터나 가상 머신이 처리할 스크립트 또는 바이트코드일 수도 있습니다. 일반적으로 실행 파일의 실행 코드 영역에는 프로세서가 수행할 명령이 들어갑니다.

### 실행 파일 생성

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/04-executable-creation.png" alt="Executable Creation" width="100%">

기계어를 직접 작성할 수도 있지만, 일반적으로 C, C++, Java와 같은 고수준 언어로 프로그램을 작성합니다. 작성한 코드는 컴파일 과정을 거쳐 바로 실행할 수 있는 기계 코드 파일이나, 아직 연결 과정이 필요한 객체 파일로 변환됩니다.

#### “실행 가능한 기계 코드 파일”이란?

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/05-executable-machine-code.png" alt="Executable Machine Code File" width="100%">

링킹까지 끝나 운영체제가 곧바로 실행할 수 있는 최종 결과물입니다. 프로그램 실행 전에 기계어로 변환을 마치는 AOT가 대표적인 생성 방식이며, C, C++, Go, Rust 등이 주로 이 형태의 실행 파일을 만듭니다.

#### “실행할 수 없는 기계 코드 객체 파일”이란?

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/06-object-code.png" alt="Object Code File" width="100%">

객체 파일은 프로그램을 구성하는 레고 조각과 같습니다. 기계어를 담고 있지만 필요한 함수와 주소가 아직 완전히 연결되지 않아 그 자체로는 실행할 수 없습니다.

Unix 계열에서는 주로 `.o`, Windows에서는 `.obj` 확장자를 사용합니다. 링커가 여러 객체 파일과 라이브러리를 하나로 연결하면 ELF와 같은 형식의 최종 실행 파일이 만들어집니다.

JVM과 JIT를 사용하는 Java나 Kotlin도 실행 중 필요한 부분을 기계어로 만들 수 있습니다. AOT와 JIT의 생성 시점 차이는 [Compilation & Interpretation (컴파일과 해석)](../compilation-interpretation/)에서 확인할 수 있습니다.

완성된 실행 파일은 일반적으로 SSD나 HDD에 저장됩니다. 반면 실행할 코드를 디스크 파일로 남기지 않고 메모리에서 직접 만들고 실행하는 방식도 있습니다.

#### In-Memory 인메모리

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/07-in-memory.png" alt="In-Memory Execution" width="100%">

인메모리 실행은 코드를 디스크에 별도 실행 파일로 저장하지 않고 RAM에서 바로 생성하거나 불러와 실행하는 방식입니다. 프로세스가 종료되면 메모리에 있던 코드도 함께 사라집니다.

사용하는 이유는 크게 세 가지로 나눌 수 있습니다.

- **속도**: RAM은 SSD보다 빠르기 때문에 실시간 처리나 브라우저 실행 환경에서 유리합니다.
- **흔적을 줄이는 보안**: 디스크의 파일은 삭제해도 덮어쓰기 전까지 데이터가 남을 수 있지만, 메모리의 코드는 프로세스 종료와 함께 사라집니다.
- **유연성**: 실행 중 현재 플랫폼에 맞는 기계어를 동적으로 생성할 수 있습니다.

대표적인 사례는 다음과 같습니다.

- 브라우저가 JavaScript를 JIT 컴파일하여 실시간으로 실행하는 경우: 속도와 유연성
- 셸 프로세스 안에서 `cd`, `exit` 같은 내장 명령을 바로 실행하는 경우: 속도
- 실행 파일을 남기지 않는 보안 프로그램이나 파일리스 코드: 흔적을 줄이는 보안

### 파일은 어떤 방식으로 실행되는가?

#### OS의 규칙 확인 (ABI와 파일 포맷 검증)

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/08-abi-and-file-format.png" alt="ABI and File Format Validation" width="100%">

사용자가 실행 파일을 열면 운영체제의 커널은 먼저 자신이 실행할 수 있는 형식인지 확인합니다.

파일 앞부분의 Magic Number로 형식을 식별합니다. Linux의 ELF는 `0x7F 'E' 'L' 'F'`, Windows 실행 파일은 `MZ`로 시작합니다.

이후 ABI(Application Binary Interface)가 맞는지 확인합니다. ABI에는 함수 호출 방법, 데이터 정렬 방식, 시스템 호출 규칙처럼 프로그램과 운영체제가 맞춰야 할 이진 수준의 약속이 들어 있습니다.

운영체제마다 실행 파일 형식과 ABI가 다르기 때문에 Windows용 실행 파일을 macOS의 Mach-O나 Linux 환경에서 그대로 실행할 수는 없습니다. ABI를 안정적으로 유지하면 소스 코드가 없어도 과거에 빌드한 프로그램이나 외부 업체의 바이너리를 계속 실행할 수 있습니다.

참고: [Application Binary Interface (응용 프로그램 이진 인터페이스)](https://ko.wikipedia.org/wiki/%EC%9D%91%EC%9A%A9_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8_%EC%9D%B4%EC%A7%84_%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4#cite_note-1)

#### 메모리 로딩 (Loader와 가상 메모리 매핑)

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/09-loader-and-virtual-memory.png" alt="Loader and Virtual Memory Mapping" width="100%">

검증을 통과하면 Loader가 실행 파일을 프로세스의 가상 주소 공간에 배치합니다.

- **Code(Text) 영역**: 명령을 담으며 일반적으로 읽기와 실행 권한을 가집니다.
- **Data·BSS 영역**: 전역 변수와 정적 변수를 담으며 읽기와 쓰기 권한을 가집니다.
- **Demand Paging**: 실행에 필요한 페이지를 실제로 접근할 때 메모리에 올립니다. 아직 올라오지 않은 페이지에 접근하면 Page Fault가 발생하고 운영체제가 해당 페이지를 불러옵니다.

#### 시작점으로 점프 (Entry Point와 CPU 레지스터 설정)

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/10-entry-point.png" alt="Entry Point and CPU Register Setup" width="100%">

운영체제는 실행 파일에 기록된 Entry Point를 확인합니다. 일반적으로 이 지점은 개발자가 작성한 `main` 함수가 아니라, 그보다 먼저 실행되는 `_start`입니다.

운영체제는 Program Counter(PC) 또는 x86-64의 RIP 같은 명령어 포인터 레지스터에 시작 주소를 넣습니다. 이후 Context Switch가 일어나면 CPU는 다음 클럭부터 사용자 프로그램의 첫 명령을 가져와 실행합니다.

#### Runtime System과 프로그램 초기화

<img src="https://raw.githubusercontent.com/CatIsApple/MyPage_/main/assets/articles/executable-bytecode/11-runtime-initialization.png" alt="Runtime System and Program Initialization" width="100%">

`_start`에 도착해도 바로 `main`이 실행되는 것은 아닙니다. 먼저 CRT(C Runtime) 또는 `crt0` 같은 런타임 시작 코드가 프로그램을 준비합니다.

1. `argc`, `argv`, 환경 변수처럼 프로그램에 전달할 값을 스택에 준비합니다.
2. BSS 영역을 0으로 초기화합니다.
3. 전역 객체와 정적 생성자를 초기화합니다.
4. 준비가 끝나면 `main` 함수를 호출합니다.

`main`이 `0`을 반환하거나 프로그램이 끝나면 런타임이 남은 정리 작업을 수행합니다. 이후 시스템 호출을 통해 커널에 종료를 알리고, 운영체제가 프로세스에 할당한 자원을 회수합니다.

가상 메모리 매핑과 프로세스 소멸 과정, ELF의 세부 구조는 각각 별도의 글에서 더 깊게 정리할 예정입니다. 지금까지의 과정이 실행 파일을 실제 프로그램으로 시작하고 안전하게 종료하는 전체 흐름입니다.

---

[← All Articles (전체 글로 돌아가기)](../../../)
