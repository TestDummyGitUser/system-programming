## Задание 1

* **Вывод Byte 0:**
  ```c
  // Код на C
  printf("Byte 0: 0x%02X\n", byte_ptr[0]);
  ```
  Соответствующий код на ассемблере:
  ```nasm
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
  ```
  Соответствующий код на ассемблере:
  ```nasm
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
  ```
  Соответствующий код на ассемблере:
  ```nasm
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
  ```
  Соответствующий код на ассемблере:
  ```nasm
  mov     rax, QWORD PTR [rbp-8]
  add     rax, 3
  movzx   eax, BYTE PTR [rax]
  movzx   eax, al
  mov     esi, eax
  mov     edi, OFFSET FLAT:.LC3
  mov     eax, 0
  call    "printf"
  ```

**Данный код на языке C выведет следующее:**
```text
Byte 0: 0xDD
Byte 1: 0xCC
Byte 2: 0xBB
Byte 3: 0xAA
```

---

## Задание 2

```c
// Код на C
uint32_t magic = 0xAABBCCDD;
```

Соответствующий код на ассемблере:
```nasm
mov     DWORD PTR [rbp-12], -1430532899
```

**Объяснение инструкции:** 
Записать число `-1430532899` в память по адресу `[rbp-12]` размером в одно двойное слово (`DWORD PTR`, 4 байта / 32 бита).