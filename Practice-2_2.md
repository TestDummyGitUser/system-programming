# Практическая работа №2.2: Анатомия типов данных
## Часть 1. Целые числа: знаковое представление и переполнение
### Задание №1.1
**Цель задания:** 

**int8_t (signed byte)** — тип целочисленных чисел, который содержит 8 бит.
   * Всего возможных комбинаций единиц и нулей: **256** (2 в 8 степени).
   * На отрицательные числа отводится **128** комбинаций (256 : 2).
   * На число `0` отводится **одна** комбинация.
   * На положительные числа — **127** комбинаций (256 : 2 - 1).

   Ошибка не выводится, так как старший бит становится равным 1, что означает изменение знака.
   ```text
   0111 1111 ->  127
   1000 0000 -> -128
   1000 0001 -> -127
   ```
---
### Задание №1.2
**Цель задания:** 

Тип `int8_t` хранит 256 значений: 128 отрицательных, 127 положительных, и 1 приходится на ноль.

---
### Задание №1.3
**Цель задания:** 

 Диапазон асимметричен вследствие того, что ноль тоже занимает одно место.
   ```text
   0000 0000 ->  0

   0000 0001 ->  1
   0111 1111 ->  127

   1000 0000 -> -128
   1111 1111 -> -1
   ```

---

## Задание 2

```nasm
cmp     DWORD PTR [rbp-8], eax
jbe     .L2
mov     edi, OFFSET FLAT:.LC0
call    puts
jmp     .L3
.L2:
mov     edi, OFFSET FLAT:.LC1
call    puts
```

* Из-за неявного приведения типов число `-1` превращается в `4294967295` (максимальное беззнаковое число). Из этого следует, что `4294967295` больше `1`.
* Инструкция `cmp` сравнивает `DWORD PTR [rbp-8]` (b) и `eax` (a).

---

## Задание 3

1. В IEEE 754 `1.0f` выглядит как 32-битное число.
   * Первые 23 бита составляет мантисса.
   * Следующие 8 битов составляет экспонента.
   * И последний бит, старший — знак числа.
   
   ```text
   0 011 1111 1000 0000 0000 0000 0000 0000
   ```

2. Результатом при сложении `0.1f + 0.2f` не будет `0.3f`, так как числа `0.1`, `0.2` и `0.3` в десятичной системе не имеют точного конечного представления в двоичной системе. Они хранятся как приближённые значения.

3. **NaN** — Not a Number
   ```text
   1 111 1111 1100 0000 0000 0000 0000 0000 -> NaN
   ```

4. `+0.0` и `-0.0` существуют из-за старшего бита.
   ```text
   0 000 0000 0000 0000 0000 0000 0000 0000 ->  0.0
   1 000 0000 0000 0000 0000 0000 0000 0000 -> -0.0
   ```

---

## Задание 4

* **Вариант 1 (0.2f):**
  ```c
  float test() {
      return 0.2f;
  }
  ```
  Соответствует код на ассемблере:
  ```nasm
  "test":
          movss   xmm0, DWORD PTR .LC0[rip]
          ret
  .LC0:
          .long   1045220557
  ```
  Десятичное число `1045220557` соответствует шестнадцатеричному представлению `0x3E4CCCCD` для `0.2f`.

* **Вариант 2 (0.3f):**
  ```c
  float test() {
      return 0.3f;
  }
  ```
  Соответствует код на ассемблере:
  ```nasm
  "test":
          movss   xmm0, DWORD PTR .LC0[rip]
          ret
  .LC0:
          .long   1050253722
  ```
  Десятичное число `1050253722` соответствует шестнадцатеричному представлению `0x3E99999A` для `0.3f`.

* **Вариант 3 (2 * 0.3f):**
  ```c
  float test() {
      return 2 * 0.3f;
  }
  ```
  Соответствует код на ассемблере:
  ```nasm
  "test":
          movss   xmm0, DWORD PTR .LC0[rip]
          ret
  .LC0:
          .long   1058642330
  ```
  Десятичное число `1058642330` соответствует шестнадцатеричному представлению `0x3F19999A` для результата вычисления `2 * 0.3f` (то есть `0.6f`).

---

## Задание 5

Код на C:
```c
int test_aliasing(int *ip, float *fp) {
    *ip = 42;
    *fp = 3.14f;
    return *ip;
}

int good_aliasing(int *ip, float *fp) {
    *ip = 42;
    *fp = 3.14f;
    return *ip;
}
```

* **Соответствует код на ассемблере при `-O0`:**
  ```nasm
  "test_aliasing":
          push    rbp
          mov     rbp, rsp
          mov     QWORD PTR [rbp-8], rdi
          mov     QWORD PTR [rbp-16], rsi
          mov     rax, QWORD PTR [rbp-8]
          mov     DWORD PTR [rax], 42
          mov     rax, QWORD PTR [rbp-16]
          movss   xmm0, DWORD PTR .LC0[rip]
          movss   DWORD PTR [rax], xmm0
          mov     rax, QWORD PTR [rbp-8]
          mov     eax, DWORD PTR [rax]
          pop     rbp
          ret
  "good_aliasing":
          push    rbp
          mov     rbp, rsp
          mov     QWORD PTR [rbp-8], rdi
          mov     QWORD PTR [rbp-16], rsi
          mov     rax, QWORD PTR [rbp-8]
          mov     DWORD PTR [rax], 42
          mov     rax, QWORD PTR [rbp-16]
          movss   xmm0, DWORD PTR .LC0[rip]
          movss   DWORD PTR [rax], xmm0
          mov     rax, QWORD PTR [rbp-8]
          mov     eax, DWORD PTR [rax]
          pop     rbp
          ret
  .LC0:
          .long   1078523331
  ```

* **Соответствует код на ассемблере при `-O2`:**
  ```nasm
  "test_aliasing":
          mov     DWORD PTR [rdi], 42
          mov     eax, 42
          mov     DWORD PTR [rsi], 0x4048f5c3
          ret
  "good_aliasing":
          mov     DWORD PTR [rdi], 42
          mov     eax, 42
          mov     DWORD PTR [rsi], 0x4048f5c3
          ret
  ```

**Вывод:** Из этого следует, что типы `int*` and `float*` не могут делить одну память, следовательно, лишнее чтение удаляется.

---

## Задание 6

Код на ассемблере при `-O2`:
```nasm
"float_to_bits_v1":
        movd    eax, xmm0
        ret
"float_to_bits_v2":
        movd    eax, xmm0
        ret
```

---

## Задание 7

### 1. Реализация сборки float через union
```c
#include <stdio.h>
#include <stdint.h>

struct FloatParts {
    uint32_t mantissa : 23;
    uint32_t exponent : 8;
    uint32_t sign : 1;
};

union FloatUnion {
    struct FloatParts parts;
    float result_float;
};

float make_float(uint32_t sign, uint32_t exp, uint32_t mant) {
    union FloatUnion u;
    u.parts.sign = sign;
    u.parts.exponent = exp;
    u.parts.mantissa = mant;
    return u.result_float;
}

int main() {
    float f = make_float(0, 127, 0);
    
    printf("Собранный float: %f\n", f);
    return 0;
}
```

### 2. Проверка на NaN
```c
#include <stdio.h>
#include <stdint.h>
#include <string.h>

int is_nan(float f) {
    uint32_t bits;
    memcpy(&bits, &f, sizeof(float));

    uint32_t exp = (bits >> 23) & 0xFF;
    uint32_t mant = bits & 0x7FFFFF;

    if (exp == 255 && mant != 0) {
        printf("Is NaN");
        return 1;
    }

    printf("Is not NaN");
    return -1;
}

int main() {
    printf("\nAnswer: %d", is_nan(0.0f));
}
```

### 3. Fast inverse square root
1. Переменную типа `float` берут как `int`.
2. Выполняется сдвиг битов этой переменной вправо на 1 (для извлечения корня).
3. Производится вычитание полученного результата из константы `0x5F3759DF`.
4. Полученные биты снова читают как `float`.
5. Применяется формула: `результат * (1.5 - 0.5 * x * результат * результат)`.

**0x5F3759DF:** у этой битовой комбинации есть структура, которая помогает получить хорошее начальное приближение при работе с числами с плавающей запятой.
