# compiler-lab

A collection of Compiler Design course assignments featuring the implementation of essential compiler components: Parser, Scanner, and Semantic Analysis.  
컴파일러 설계 수업에서 진행한 과제들을 담은 레포지토리입니다. Parser, Scanner, Semantic Analysis 구현 과제가 포함되어 있으며, 어휘 분석, 구문 분석, 의미 분석 등 컴파일러 프론트엔드 구성 과정을 다룹니다.

---

- [요구사항 문서 (P1)](./Parser/Parser_2023.pdf)
- [결과 보고서 (P1)](./Parser/2019082279/Report.pdf)

<details>
<summary># 1_Scanner (Scanner)</summary>

이 레포지토리는 **컴파일러 설계** 수업의 과제로 진행한 **C-Minus Scanner 구현 프로젝트**를 포함하고 있습니다.  
C-Minus 언어의 Scanner를 **C 코드**와 **Lex(Flex)** 두 가지 방식으로 구현하여 입력된 소스 코드로부터 토큰을 추출하는 기능을 수행합니다.

## 📁 프로젝트 구성
- `scan.c`: **DFA 기반** C-Minus Scanner 구현 파일  
- `cminus.l`: **Lex(Flex) 기반** Scanner 구현 파일  
- `globals.h`: Scanner에서 사용될 **토큰 타입** 정의 파일  
- `util.c`: **토큰 출력 함수** 구현  
- `Makefile`: 프로젝트 빌드를 위한 Makefile  
- `example/`: 테스트 입력 파일들  
- `Report.pdf`: 과제 구현 결과를 정리한 보고서  

## 🚀 구현 내용
### 1. C 코드 기반 Scanner
- **DFA(Deterministic Finite Automaton)를 이용한 토큰 인식**  
- `scan.c`에서 `getToken()` 함수 구현  
  - `START` 상태에서 입력을 읽고, **예약어, 연산자, ID, 숫자** 등을 구분  
  - `=`, `<`, `>`, `!` 등의 문자를 처리하여 `==`, `<=`, `>=`, `!=` 같은 복합 연산자를 인식  
  - 주석(`/* */`)을 처리하고, 주석이 종료되지 않은 경우 EOF로 처리  
- `util.c`에서 `printToken()`을 수정하여 새로운 토큰 형식에 맞게 출력  

### 2. Lex(Flex) 기반 Scanner
- **정규 표현식(Regex) 기반 Scanner**  
- `cminus.l`에서 **Lex 규칙을 수정**하여 C-Minus 문법에 맞는 토큰을 인식  
  - `/` 이후 `*`가 나오면 주석(`/* */`)으로 처리  
  - 예약어, ID, 숫자 등의 토큰을 패턴 매칭을 이용해 추출  
- `flex`를 이용하여 `cminus_lex` 실행 파일 생성  

## 📌 참고 사항
- `main.c`에서 `getToken()`을 호출하여 토큰을 반복적으로 분석  
- `NO_PARSE`, `TraceScan` 옵션을 `TRUE`로 설정하여 디버깅 가능  
- `example/test.cm`을 테스트하여 정상적으로 동작하는지 확인  

## 🔨 빌드 및 실행 방법
### WSL(Ubuntu 20.04)에서 빌드 및 실행
```bash
make clean
make all
./cminus_cimpl ./example/test.cm  # C 코드 기반 Scanner 실행
./cminus_lex ./example/test.cm    # Lex(Flex) 기반 Scanner 실행
```

## 📝 보고서 요약
- **DFA 기반 C 코드 구현 과정** 및 **Lex(Flex) 사용 방식** 설명  
- **주석 처리, 예약어 인식, 연산자 처리 방식** 등 주요 구현 내용 정리  
- **테스트 결과**: test, test2, test3, test4, test5 파일을 통과하여 정상 동작 확인  

</details>

---

- [요구사항 문서 (P2)](./Scanner/Scanner_2023.pdf)
- [결과 보고서 (P2)](./Scanner/2019082279/Report.pdf)

<details>
<summary># 2_Parser (Parser)</summary>

**컴파일러 설계** 수업의 과제로 진행한 **Parser 구현 프로젝트**를 포함하고 있습니다.  
C-Minus 언어의 파서를 **Yacc (Bison)** 을 사용하여 구현하였으며, 입력된 소스 코드로부터 **추상 구문 트리(AST)** 를 생성합니다.

## 📁 프로젝트 구성
- `cminus.y`: C-Minus 문법을 BNF 형식으로 정의한 파일  
- `globals.h`: AST 노드 구조를 정의하는 헤더 파일  
- `util.c`: AST 노드를 생성 및 출력하는 함수들을 포함  
- `Makefile`: 프로젝트 빌드를 위한 Makefile  
- `example/`: 테스트 입력 파일들  
- `Report.pdf`: 과제 구현 결과를 정리한 보고서  

## 🚀 구현 내용
### 1. 파서 구성 요소
- **함수 선언 (fun_declaration)**  
  - `val_declaration` 구조를 참고하여 함수 선언을 구현하고, `child` 노드를 추가하여 함수의 매개변수와 블록을 설정  
- **매개변수 리스트 (params)**  
  - `param_list`와 `param`으로 구성되며, `declaration_list` 구조를 참고하여 구현  
  - `VOID` 파라미터 처리를 위해 `flag` 값을 설정하여 구분  
- **복합문 (compound_stmt)**  
  - 다이어그램을 참고하여 AST 노드를 생성하고 `local_declarations`와 `statement_list`를 `child`로 설정  
- **선택문 (selection_stmt)**  
  - `IF`와 `IF-ELSE` 문을 구분하여 `child` 개수와 `flag` 값으로 표현  
- **반복문 (iteration_stmt)**  
  - `WHILE` 문을 구현하고, 조건과 본문을 `child`로 설정  
- **반환문 (return_stmt)**  
  - 반환 값이 있는 경우와 없는 경우를 구분하여 처리  
- **이항 연산 (simple_expression, additive_expression, term)**  
  - 연산자 노드를 생성하고 좌측 및 우측 피연산자를 `child`로 설정  
- **함수 호출 (call)**  
  - 이름과 인수를 설정하여 구현  

### 2. 참고 사항
- 과제 요구 사항에 따라 `cminus.y` 파일을 수정하여 구문 분석기를 구현했습니다  
- `globals.h` 및 `util.c` 파일을 참고하여 AST 노드를 정의하고 처리했습니다  
- 테스트 파일(`example/test.1.txt`)을 사용하여 결과를 확인하였으며, 결과는 예제 출력과 동일하게 나왔습니다  

## 🔨 빌드 방법
### WSL(Ubuntu 20.04)에서 빌드
```bash
make clean
make all
./cminus_parser ./example/test.1.txt
```

</details>

---

- [요구사항 문서 (P3)](./SemanticAnalysis/Semantic_Analysis_2023.pdf)
- [결과 보고서 (P3)](./SemanticAnalysis/2019082279/Report.pdf)

<details>
<summary># 3_Semantic_Analysis (Symbol Table & Type Checker)</summary>

**컴파일러 설계** 수업에서 진행한 **Semantic Analysis** 구현 프로젝트를 포함하고 있습니다.  
C-Minus 언어의 의미 분석기를 구현하였으며, **심볼 테이블(Symbol Table)** 과 **타입 체커(Type Checker)** 를 통해 소스 코드의 의미적 오류를 탐지합니다.

## 📁 프로젝트 구성
- `symtab.h` / `symtab.c`: 심볼 테이블의 구조와 기능을 정의 및 구현한 파일  
- `analyze.h` / `analyze.c`: 의미 분석 로직(심볼 테이블 생성 및 타입 체크)을 포함한 파일  
- `globals.h`: AST 노드와 데이터 구조를 정의하는 헤더 파일  
- `main.c`: 프로그램의 실행 흐름을 제어하며 의미 분석만 출력하도록 설정  
- `Makefile`: 프로젝트 빌드를 위한 Makefile  
- `example/`: 테스트 입력 파일들  
- `Report.pdf`: 과제 구현 결과를 정리한 보고서  

## 🚀 구현 내용
### 1. 주요 구성 요소
- **심볼 테이블 생성 (Symbol Table)**  
  - `insertNode()` 함수를 통해 AST를 순회하며 함수, 변수 등의 심볼을 심볼 테이블에 삽입  
  - `ScopeRec`와 `SymbolRec` 구조를 활용하여 스코프와 심볼 정보를 계층적으로 관리  
  - 내장 함수(`input`, `output`)를 전역 스코프에 삽입하며, `lineno`를 0으로 설정  
  - 중복 정의(redefinition) 및 미정의(undeclared) 변수/함수 오류를 탐지  
  - Void 타입 변수 선언 금지 규칙을 적용  

- **타입 체커 (Type Checker)**  
  - `checkNode()` 함수를 통해 AST를 순회하며 타입 일치 여부를 확인  
  - **조건문 (If/While)**: 조건식이 `int` 타입이어야 하며, 그렇지 않을 경우 오류 출력  
  - **반환문 (Return)**: 함수의 반환 타입과 실제 반환 값의 타입을 비교  
  - **함수 호출 (Call)**: 매개변수와 인수의 개수 및 타입을 비교하여 오류 탐지  
  - **배열 인덱싱**: 인덱스가 `int` 타입인지 확인하고, 배열이 아닌 변수에 인덱싱 시 오류 출력  
  - **연산 및 대입**: 좌항(LHS)과 우항(RHS)의 타입이 일치해야 하며, 허용되지 않는 연산(예: `int[] + int[]`)을 탐지  

### 2. 참고 사항
- 과제 요구 사항에 따라 `NO_ANALYZE`를 `FALSE`로 설정하고, `TraceAnalyze`를 활용하여 디버깅  
- C-Minus 언어의 타입 시스템(`void`, `int`, `int[]`)을 준수하며 구현  
- 출력 형식은 과제에서 지정한 포맷(예: `Error: undeclared variable "x" at line 3`)을 엄격히 따름  
- 테스트 케이스(`example/`)를 통해 결과를 검증하였으며, 예상 출력과 일치함을 확인  

## 🔨 빌드 방법
### WSL(Ubuntu 20.04)에서 빌드
```bash
make clean
make all
./cminus_semantic ./example/test.cm
```

### 추가 빌드 옵션
- `TraceAnalyze = TRUE`로 설정하여 심볼 테이블 생성 과정을 출력 가능  
- `testcase_result.sh` 스크립트를 사용해 모든 테스트 케이스의 결과를 `./my_result` 디렉토리에 저장:  
```bash
chmod +x testcase_result.sh
./testcase_result.sh
```

## 📝 보고서
- `Report.pdf`에는 컴파일 환경, 구현 방법, 테스트 예제 및 결과 스크린샷이 포함되어 있습니다  
- 자세한 내용은 보고서를 참고해주세요  

</details>

---
