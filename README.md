
---

# 📌 RISC-V 아키텍처 소개 및 상세 구조

---

## 📖 목차

1. [RISC-V란?](#1-risc-v란)
2. [RISC-V 레지스터(Register)](#2-risc-v-레지스터register)
3. [RISC-V 산술 및 논리 연산](#3-risc-v-산술-및-논리-연산)
4. [CISC vs RISC](#4-cisc-vs-risc)

---

## 1. RISC-V란?

### 🔸 RISC-V의 정의
- **RISC-V**는 **Reduced Instruction Set Computer**의 5번째 버전을 의미
- **Reduced (축소)**: 명령어의 수가 적고 간단함
- **Instruction Set (명령어 집합)**: CPU가 수행 가능한 명령어들의 집합
- **Computer**: 연산을 수행하는 장치 (CPU)

### 🔸 CPU 구조 및 동작 원리
CPU는 **Central Processing Unit**의 약자로, Intel, AMD와 같은 프로세서를 의미합니다.

![CPU 구조](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmXbDO%2FbtsGYzug5pL%2FylYV2fzvz7KyPCFQWS70M1%2Fimg.png)

- **Program Counter (PC)**: 다음 수행할 명령어 주소 저장
- **Register File**: 임시 데이터를 저장하는 공간
  - 예: 중간 연산 결과 저장 (0+1=1, 1+2=3 등)
- **ALU (Arithmetic & Logic Unit)**: 산술 및 논리 연산 수행
  - 예: 더하기(+), 빼기(-) 등
- **Control Logic**: 명령어를 메모리에서 가져오고 해석하여 ALU에 전달

### 🔸 ISA (Instruction Set Architecture)
- ISA는 소프트웨어와 하드웨어 간의 인터페이스 역할
- 소프트웨어: 애플리케이션 프로그램, 컴파일러, 운영체제
- 하드웨어: CPU 설계, 회로 설계 및 칩 구조
- ISA는 소프트웨어가 명령어를 사용하여 프로그램을 동작시키고, 하드웨어는 이를 실제로 수행하는 구조를 정의함

---

## 2. RISC-V 레지스터(Register)

RISC-V는 총 32개의 레지스터를 사용합니다.

![RISC-V Register](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtTRFp%2FbtsGYcF6dv7%2F6pTkmCUoV08VykNKkwPKD0%2Fimg.png)

### 🔸 Caller-saved vs Callee-saved
- **Caller-saved**: 호출자가 저장 및 복원 책임
- **Callee-saved**: 호출받는 함수가 저장 및 복원 책임

### 🔸 주요 레지스터 설명

| Register | Name        | Description                          | Saved By |
|----------|-------------|--------------------------------------|----------|
| x0       | zero        | 항상 0, 변경 불가                     | -        |
| x1       | ra          | Return Address (리턴 주소)            | Caller   |
| x2       | sp          | Stack Pointer (스택 포인터)           | Callee   |
| x3       | gp          | Global Pointer (글로벌 변수 기반 주소)| -        |
| x4       | tp          | Thread Pointer (스레드 로컬 저장소)   | -        |
| x5-7     | t0-t2       | 임시 레지스터                         | Caller   |
| x8       | fp/s0       | Frame Pointer (프레임 포인터)         | Callee   |
| x9       | s1          | 저장 레지스터                         | Callee   |
| x10-11   | a0-a1       | 함수 인자/리턴 값                     | Caller   |
| x12-17   | a2-a7       | 함수 인자                             | Caller   |
| x18-27   | s2-s11      | 저장 레지스터                         | Callee   |
| x28-31   | t3-t6       | 임시 레지스터                         | Caller   |

### 🔸 Stack Pointer (sp) 동작 예시
- RISC-V는 주소가 작아지는 방향으로 스택을 사용
- 공간 할당 및 반환 예시
```assembly
addi sp, sp, -32  # 할당
addi sp, sp, 32   # 반환
```
![Stack Pointer](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FUFWgy%2FbtsGYR9mRgm%2FVUoyan2uafCnhqNuTdVFn1%2Fimg.png)

### 🔸 Frame Pointer (fp) 예시
- 이전 스택 포인터 저장 및 복원
![Frame Pointer](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fw6cOh%2FbtsGYNMP1j9%2FYtVT9OpEvQ8BsaUkWkwUYK%2Fimg.png)

### 🔸 함수 호출 시 인자 전달 예시
![Function Call](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FuREds%2FbtsG0HEd3IR%2FfZS3eRJ14viLOZtGTk8G4K%2Fimg.png)

---

## 3. RISC-V 산술 및 논리 연산

### 🔸 기본 형태 및 특징
- 명령어 형태: `Destination = Source1 op Source2`
- 모든 ALU 연산은 레지스터를 통해서만 가능 (메모리 접근 불가)
- 메모리 접근은 Load/Store 명령어로만 가능

### 🔸 명령어 포맷
- **R-type**: 기본 산술 연산
![R-type](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fddjo4r%2FbtsG0cqYkuW%2FtmSf2eSF4MOz8kBoQVwVJk%2Fimg.png)

- **I-type**: Immediate 값 사용 연산
![I-type](https://github.com/user-attachments/assets/0b49819c-d27a-48cb-8330-8e2db346e0f2)

- **U-type**: 상위 20비트 설정 (lui 명령어)
![U-type](https://github.com/user-attachments/assets/9cc98e95-ef73-4df6-ba5a-65c553572d24)

### 🔸 Multiply 연산 예시 (Shift & Add 조합)
```assembly
slli a5, a1, 1   # y * 2
add  a1, a5, a1  # 2y + y = 3y
slli a5, a1, 4   # 3y * 16 = 48y
```

---

## 4. CISC vs RISC

| 특징                   | CISC (복합)                          | RISC (축소)                            |
|------------------------|-------------------------------------|----------------------------------------|
| 명령어 종류            | 많고 복잡, 길이 다양                | 적고 간단, 길이 동일                  |
| 메모리 접근            | 다양한 명령어에서 가능              | Load/Store 명령어만 가능               |
| 실행 방식              | 일반적으로 스택 기반                | 레지스터 기반                          |
| Condition Code 사용    | 사용 (상태 플래그)                  | 미사용 (별도 명령어로 처리)            |

---
