# Практическая работа №2.1: Анатомия типов данных
### Дан код на *C*:
``` C
#include <stdio.h>
#include <stdint.h>

void main() {
    uint32_t magic = 0xAABBCCDD;
    uint8_t *byte_ptr = (uint8_t*)&magic;

    printf("Byte 0: 0x%02X\n", byte_ptr[0]);
    printf("Byte 1: 0x%02X\n", byte_ptr[1]);
    printf("Byte 2: 0x%02X\n", byte_ptr[2]);
    printf("Byte 3: 0x%02X\n", byte_ptr[3]);
}
```
---
### Задание №1:
 **Цель задания:** изучить как работает и выглядит инструкция побайтового вывода числа на *C* и *Assembler*.

* **Вывод Byte 0:**

  ```c
  // Код на C
  printf("Byte 0: 0x%02X\n", byte_ptr[0]);
  
  // Соответствующий код на ассемблере:
  mov     rax, QWORD PTR [rbp-8]
  movzx   eax, BYTE PTR [rax]
  movzx   eax, al
  mov     esi, eax
  mov     edi, OFFSET FLAT:.LC0
  mov     eax, 0
  call    "printf"
  ```

* **Вывод Byte 1:**
  ```c
  // Код на C
  printf("Byte 1: 0x%02X\n", byte_ptr[1]);

  // Соответствующий код на ассемблере:
  mov     rax, QWORD PTR [rbp-8]
  add     rax, 1
  movzx   eax, BYTE PTR [rax]
  movzx   eax, al
  mov     esi, eax
  mov     edi, OFFSET FLAT:.LC1
  mov     eax, 0
  call    "printf"
  ```

* **Вывод Byte 2:**
  ```c
  // Код на C
  printf("Byte 2: 0x%02X\n", byte_ptr[2]);
  
  // Соответствующий код на ассемблере:
  mov     rax, QWORD PTR [rbp-8]
  add     rax, 2
  movzx   eax, BYTE PTR [rax]
  movzx   eax, al
  mov     esi, eax
  mov     edi, OFFSET FLAT:.LC2
  mov     eax, 0
  call    "printf"
  ```

* **Вывод Byte 3:**
  ```c
  // Код на C
  printf("Byte 3: 0x%02X\n", byte_ptr[3]);
  
  // Соответствующий код на ассемблере:
  mov     rax, QWORD PTR [rbp-8]
  add     rax, 3
  movzx   eax, BYTE PTR [rax]
  movzx   eax, al
  mov     esi, eax
  mov     edi, OFFSET FLAT:.LC3
  mov     eax, 0
  call    "printf"
  ```


#### Инструкции выведут следующее:
``` C
Byte 0: 0xDD
Byte 1: 0xCC
Byte 2: 0xBB
Byte 3: 0xAA
```

---

### Задание №2:
**Цель задания:** изучить, как компилятор кладет `0xAABBCCDD` в память, и понять, как выглядит эта переменная, в инструкциях *Assembler*.

```c
// Код на C
uint32_t magic = 0xAABBCCDD;

// Соответствующий код на ассемблере:
mov     DWORD PTR [rbp-12], -1430532899
```

**Объяснение инструкции:** 
Происходит копирование значения `-1430532899` переменной `0xAABBCCDD` в адрес `[rbp-12]`, размер которого равен одному двойному слово (`DWORD PTR`, 4 байта).