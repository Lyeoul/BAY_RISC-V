# BAY_RISC-V

# RISC-V 개념 및 CPU 구조 소개

## 목차
1. [RISC-V의 의미](#risc-v의-의미)
2. [CPU의 기본 구조](#cpu의-기본-구조)
3. [명령어 집합 (Instruction Set)](#명령어-집합-instruction-set)
4. [ISA (Instruction Set Architecture)](#isa-instruction-set-architecture)

---

## RISC-V의 의미

**RISC-V**는 다음과 같은 의미를 가지고 있습니다.

- **RISC**: Reduced Instruction Set Computer
- **V**: 로마 숫자 '5'로, 다섯 번째 버전을 의미합니다.

각각의 키워드를 나누어 설명하면:

- **Reduced**: 작거나 적은 수량을 의미합니다.
- **Instruction Set**: CPU가 사용하는 명령어들의 집합을 의미합니다.
- **Computer**: 연산을 수행하는 장치를 의미합니다.

즉, **RISC-V**는 적은 수의 명령어로 구성된 명령어 집합을 사용하는 컴퓨터를 의미합니다. RISC는 CISC(Complex Instruction Set Computer)와 대비되는 개념으로, 이 둘의 차이는 이후에 구체적으로 다루겠습니다.

---

## CPU의 기본 구조

CPU(Central Processing Unit)는 컴퓨터의 핵심 연산 장치로, Intel이나 AMD의 프로세서가 대표적입니다. CPU가 명령어를 처리하는 과정을 도식화하면 다음과 같습니다.

![CPU 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmXbDO%2FbtsGYzug5pL%2FylYV2fzvz7KyPCFQWS70M1%2Fimg.png)

CPU는 프로그램의 명령어(Instruction)를 순차적으로 처리하며, 다음의 요소들로 구성됩니다.

- **PC (Program Counter)**: 다음에 수행할 명령어의 주소를 저장하는 레지스터입니다.
- **Register File**: 연산 과정에서 임시로 데이터를 저장하는 장소입니다.
  - 예시: 0에서 10까지의 합을 계산할 때, 중간 결과값을 저장하는 용도로 사용됩니다.
- **ALU (Arithmetic & Logic Unit)**: 산술 연산(예: 덧셈 '+')과 논리 연산을 수행합니다.
- **Control Logic**: 명령어를 메모리에서 가져오고, 명령어의 종류를 판별하여 필요한 작업을 수행합니다.

---

