### Задание 1

**Код на C:**
```c
long pw1(long a, long b){
    return a / b;
}
```

**Код на assembly gcc:**
```assembly
        mov     rax, rdi
        .loc 1 2 14 view .LVU3
        cqo
        idiv    rsi
        .loc 1 3 1 view .LVU4
        ret
```

**Код на assembly clang:**
```assembly
        mov     rax, rdi
        .loc    1 2 14 prologue_end
        mov     rcx, rdi
        or      rcx, rsi
        shr     rcx, 32
        je      .LBB0_1
        cqo
        idiv    rsi
        .loc    1 2 5 is_stmt 0
        ret
```

---

### Задание 2

**Код на C:**
```c
int pw2(int a){
    return ((a & 0x00FF) << 8) | ((a & 0xFF00) >> 8);
}
```

**Код на assembly gcc:**
```assembly
        push    rbp
        .cfi_def_cfa_offset 16
        .cfi_offset 6, -16
        mov     rbp, rsp
        .cfi_def_cfa_register 6
        mov     DWORD PTR [rbp-4], edi
        .loc 1 2 26
        mov     eax, DWORD PTR [rbp-4]
        sal     eax, 8
        movzx   edx, ax
        .loc 1 2 48
        mov     eax, DWORD PTR [rbp-4]
        sar     eax, 8
        movzx   eax, al
        .loc 1 2 32
        or      eax, edx
        .loc 1 3 1
        pop     rbp
        .cfi_def_cfa 7, 8
        ret
```

**Код на assembly clang:**
```assembly
        push    rbp
        .cfi_def_cfa_offset 16
        .cfi_offset rbp, -16
        mov     rbp, rsp
        .cfi_def_cfa_register rbp
        mov     dword ptr [rbp - 4], edi
        .loc    1 2 14 prologue_end
        mov     eax, dword ptr [rbp - 4]
        .loc    1 2 16 is_stmt 0
        and     eax, 255
        .loc    1 2 26
        shl     eax, 8
        .loc    1 2 36
        mov     ecx, dword ptr [rbp - 4]
        .loc    1 2 38
        and     ecx, 65280
        .loc    1 2 48
        sar     ecx, 8
        .loc    1 2 32
        or      eax, ecx
        .loc    1 2 5 epilogue_begin
        pop     rbp
        .cfi_def_cfa rsp, 8
        ret
```

---

### Задание 3

**Код на assembly ARM64:**
```assembly
        sdiv    x0, x0, x1
.LVL1:
        .loc 1 3 1 view .LVU3
        ret
```

---

### Задание 4

**Код на C:**
```c
// Первый вариант:
long pw1(long a, long b){
    return a*2 + b;
}

// Второй вариант:
long pw1(long a, long b){
    return a*3 + b;
}
```

**Код на assembly gcc:**
```assembly
// Первый вариант:
        lea     rax, [rsi+rdi*2]
        .loc 1 3 1 view .LVU3
        ret

// Второй вариант:
        lea     rax, [rdi+rdi*2]
        .loc 1 2 16 view .LVU3
        add     rax, rsi
        .loc 1 3 1 view .LVU4
        ret
```

Команда `lea` предназначена для быстрого вычисления адресов памяти, поэтому её аппаратная схема жестко привязана к формуле адресации процессора, которая использует операцию сложения и операцию умножения (только на 1, 2, 4 или 8).

---

### Задание 5

*   **O0:** https://godbolt.org/z/h1ebzWavf
*   **O1:** https://godbolt.org/z/9hxe8oYs3
*   **O2:** https://godbolt.org/z/GEbnnW5Ks
*   **O3:** https://godbolt.org/z/f4893fq4q

По возрастанию уровня оптимизации цикл постепенно разворачивается.