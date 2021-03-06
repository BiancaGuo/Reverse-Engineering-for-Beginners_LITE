## PART II 重要的基本概念

## Chapter 22 有符号数字的表示
- 二进制补码是最流行的方式
- 一些字节值如下：
    ![graph](./assets/reverse334.jpg)
    - 无符号情况下：0xFFFFFFFE > 0x00000002
    - 有符号情况下：0xFFFFFFFE < 0x00000002
    - 无符号的条件跳转：JA、JB
    - 有符号的条件跳转：JG、JL

- C/C++ 有符号类型（+范围）
    - int64_t： 0x8000000000000000～0x7FFFFFFFFFFFFFFF
    - int：0x80000000～0x7FFFFFFF),
    - char：0x80～0x7F),
    - ssize_t

- C/C++ 无符号类型（+范围）
    - uint64_t：0～0xFFFFFFFFFFFFFFFF)
    - unsigned int：0～0xFFFFFFFF
    - unsigned char：0～0xFF
    - size_t

- 有符号类型的最高有效位代表符号：1（-）、0（+）
- 求相反数：把所有的位取逆(1->0, 0->1)，然后再加1。我们可以记住，许多负号位于对边，与0的距离相同。需要加1，因为中间是0。
```python
def intToBin32(self, i):
    return (bin(((1 << 32) - 1) & i)[2:]).zfill(32)
```
- x86中，有符号数乘法和除法使用指令——IDIV/IMUL；无符号数使用指令——DIV/MUL



## Chapter 23 内存
- 内存的三种类型：
    - **全局内存**：又称“静态内存分配”。不需要显式分配，只需全局声明变量/数组即可完成分配。这些是全局变量，驻留在data段或constant段中，全局可用。但是这种内存分配方式不适用于缓冲区/数组，因为它们必须有一个固定的大小。如果发生缓冲区溢出，附近内存中的变量或缓冲区将会被覆盖。
    - **Stack**：又名“栈上的内存分配”，通过在函数内部本地声明变量/数组来完成的。这些通常是函数的局部变量。有时，被调函数也可以使用这些局部变量(调用者将指向变量的指针传递给被调用者)。这种内存分配和收回的速度很快，只需要改变SP（栈指针）即可。但是它们对于缓冲区/数组也不方便，因为缓冲区大小必须是固定的，除非使用alloca()动态分配。
    - **Heap**：动态内存分配。可以通过在C++中调用malloc()/ free()或new/delete来完成内存的分配/收回。这是最方便的方法：可以在运行时设置块大小。分配后还可以调整大小（使用realloc()），但速度较慢。这是分配内存最慢的方法：内存分配器在分配和取消分配时必须支持并更新所有控制结构。缓冲区溢出通常会覆盖这些结构。堆分配也是`内存泄漏问题`的根源：必须显式地释放每个内存块，另一个问题是“UAF”漏洞——在调用free()之后仍使用内存块，这非常危险。
