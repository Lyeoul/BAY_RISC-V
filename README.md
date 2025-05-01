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



## RISC-V 레지스터의 기능 및 특징

RISC-V는 총 32개의 레지스터를 가지며, 각각의 레지스터는 특정한 용도와 특징을 가지고 있습니다. 효율적인 사용을 위해 레지스터는 Caller와 Callee가 저장해야 하는지 여부에 따라 구분됩니다.

---

### Caller saved vs. Callee saved

- **Caller**: 함수를 호출하는 주체 (예: Main 함수)
- **Callee**: 호출된 함수 (예: foo 함수)

레지스터는 사용 가능한 개수가 제한되어 있어, Caller와 Callee가 동시에 사용할 경우 데이터가 사라질 수 있습니다. 이를 해결하기 위해 레지스터를 백업하고 복원하는 작업이 필요합니다. 이 작업을 누가 수행하느냐에 따라 아래와 같이 나뉩니다.

| 구분 | 설명 |
|------|------|
| **Caller saved** | Caller가 저장해야 하는 레지스터이며, 호출 전 Caller가 백업하고 호출 후 복원해야 합니다. |
| **Callee saved** | Callee가 저장해야 하는 레지스터이며, 호출된 함수에서 복원 책임이 있습니다. |

---

### 개별 레지스터 설명

#### 1. x0 (Hard-wired zero)
- 항상 값이 '0'으로 고정되어 있으며 변경 불가능합니다.
- 연산에서 '0' 값을 효율적으로 사용하기 위해 활용됩니다.
- 예시:
  - `sw x0, 0(x10)` : 메모리 주소에 '0' 저장
  - `addi x1, x0, 3` : x1에 '3' 저장 (x0 값이 0이므로 가능)
  - Branch 명령어에서 0과 비교 시 활용 (`beqz`, `bnez`, 등)

#### 2. x1 (Return address, Caller saved)
- 함수 호출 후 돌아올 주소(Return address)를 저장합니다.
- 예시:
  ```assembly
  jal x1, offset  # x1 = PC + 4; PC = PC + offset;
  ret             # jalr x0, x1, 0; PC = x1;

#### 3. x2 (Stack pointer, Callee saved)
- 스택의 마지막 주소를 저장하며, 스택은 주소가 작아지는 방향으로 증가합니다.
- 공간 할당 시 주소가 감소하고, 반환 시 주소가 증가합니다.
- 예시:
  ```assembly
  addi sp, sp, -32  # 스택 공간 할당
  addi sp, sp, 32   # 스택 공간 반환
  ```

#### 4. x3 (Global pointer)
- 전역 변수들이 저장된 영역의 Base address를 저장합니다.
- 코드 크기와 명령어 수를 줄이기 위해 사용됩니다.

#### 5. x4 (Thread pointer)
- 멀티스레드 환경에서 각 스레드의 고유한 메모리 공간(Thread-local storage)의 주소를 저장합니다.

---

### Temporary registers와 Saved registers 구분

| 구분 | 레지스터 | 설명 | 저장 책임 |
|------|----------|------|-----------|
| **Temporary registers** | x5-7, x28-31 | 임시로 사용되며 백업할 필요 없음 | Caller |
| **Saved registers** | x8-9, x18-27 | 호출된 함수에서 반드시 백업 및 복원 필요 | Callee |

---

#### 6. x8 (Saved register/Frame pointer, Callee saved)
- Stack frame을 관리하며, 호출된 함수에서 이전 Stack pointer 값을 저장하여 복원할 때 사용합니다.
- Stack frame은 Caller로 돌아가기 위한 정보, Arguments, Local variables 등을 포함합니다.

---

#### 7. x10-11 (Function arguments/Return values, Caller saved)
#### 8. x12-17 (Function arguments, Caller saved)
- Caller가 Callee에게 전달하는 인자(arguments) 값을 저장합니다.
- 함수 수행 후 결과값을 Caller에게 반환할 때 x10과 x11을 사용합니다.

예시:
```assembly
# 함수 호출 시
addi x10, x0, 5  # 첫 번째 인자
addi x11, x0, 10 # 두 번째 인자
jal x1, function # 함수 호출

# 함수 내에서 결과 반환 시
addi x10, x0, 15 # 결과값 반환
ret              # Caller로 복귀
```

---

## 결론

RISC-V 레지스터는 효율적이고 명확한 규칙에 따라 사용됩니다. Caller saved와 Callee saved 개념을 통해 메모리 관리와 데이터 보호를 최적화할 수 있으며, 각 레지스터의 특성을 잘 이해하면 더욱 효율적인 프로그래밍이 가능합니다.
```

이 마크다운 자료를 GitHub의 README.md 파일로 활용하여 효과적인 발표 자료로 사용하실 수 있습니다.
