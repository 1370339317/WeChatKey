# WeChatKey
微信ase密码

所有的特征搜索都来源于：WeChatWin.dll

密码
```ams
48 8B C1 4C 8D 15 ???????? 49 83 F8 0F 0F87 ????0000 第二个参数是key，第三个参数key长度 call:WeChatWin.dll+2C7D590 
```
 WeChatWin.dll地址

调用者特征码
```hex
89 ?? ?? 74 13 ?? 8B C7 ?? 8B D6 ?? 8B C8 E8 ???????? E9
```

上述来源于搜索汇编：
```asm
mov [rbp+*],ebx
je *
mov r8,*
mov rdx,*
mov rcx,*
call *
jmp *

```

```asm
WeChatWin.dll+108EFAE - 4D 8B E6              - mov r12,r14
WeChatWin.dll+108EFB1 - 4C 89 75 58           - mov [rbp+58],r14
WeChatWin.dll+108EFB5 - 44 89 75 60           - mov [rbp+60],r14d
WeChatWin.dll+108EFB9 - B9 04010000           - mov ecx,00000104 { 260 }
WeChatWin.dll+108EFBE - 48 8B 04 DE           - mov rax,[rsi+rbx*8]
WeChatWin.dll+108EFC2 - 8B 0C 01              - mov ecx,[rcx+rax]
WeChatWin.dll+108EFC5 - 39 0D 4D91A902        - cmp [WeChatWin.dll+3B28118],ecx { (-2147483621) }
WeChatWin.dll+108EFCB - 0F8F 71140000         - jg WeChatWin.dll+1090442
WeChatWin.dll+108EFD1 - 48 63 3D 3098A902     - movsxd  rdi,dword ptr [WeChatWin.dll+3B28808] { (32) }
WeChatWin.dll+108EFD8 - 4C 8B 35 2198A902     - mov r14,[WeChatWin.dll+3B28800] { (1F1FAE0D890) }
WeChatWin.dll+108EFDF - 4D 85 F6              - test r14,r14 { R14 解密Key}
WeChatWin.dll+108EFE2 - 0F84 52130000         - je WeChatWin.dll+109033A
WeChatWin.dll+108EFE8 - 85 FF                 - test edi,edi
WeChatWin.dll+108EFEA - 0F8E 4A130000         - jng WeChatWin.dll+109033A
WeChatWin.dll+108EFF0 - BA 01000000           - mov edx,00000001 { 1 }
WeChatWin.dll+108EFF5 - 48 8B CF              - mov rcx,rdi
WeChatWin.dll+108EFF8 - E8 EB19C001           - call WeChatWin.dll+2C909E8
WeChatWin.dll+108EFFD - 4C 8B E0              - mov r12,rax
WeChatWin.dll+108F000 - 48 89 45 58           - mov [rbp+58],rax
WeChatWin.dll+108F004 - 33 DB                 - xor ebx,ebx
WeChatWin.dll+108F006 - 48 85 C0              - test rax,rax
WeChatWin.dll+108F009 - 0F45 DF               - cmovne ebx,edi
WeChatWin.dll+108F00C - 89 5D 60              - mov [rbp+60],ebx
WeChatWin.dll+108F00F - 74 13                 - je WeChatWin.dll+108F024
WeChatWin.dll+108F011 - 4C 8B C7              - mov r8,rdi
WeChatWin.dll+108F014 - 49 8B D6              - mov rdx,r14
WeChatWin.dll+108F017 - 48 8B C8              - mov rcx,rax
WeChatWin.dll+108F01A - E8 71E5BE01           - call WeChatWin.dll+2C7D590
WeChatWin.dll+108F01F - E9 94000000           - jmp WeChatWin.dll+108F0B8
WeChatWin.dll+108F024 - 0F10 05 1DC01002      - movups xmm0,[WeChatWin.dll+319B048] { (0.00) }
WeChatWin.dll+108F02B - 0F29 44 24 70         - movaps [rsp+70],xmm0
WeChatWin.dll+108F030 - 0F29 85 10030000      - movaps [rbp+00000310],xmm0
WeChatWin.dll+108F037 - 0F29 45 C0            - movaps [rbp-40],xmm0
WeChatWin.dll+108F03B - 0F29 45 B0            - movaps [rbp-50],xmm0
WeChatWin.dll+108F03F - 0F29 44 24 60         - movaps [rsp+60],xmm0
WeChatWin.dll+108F044 - 0F29 45 80            - movaps [rbp-80],xmm0
```

获取windows驱动限制规则
```bash
git clone https://github.com/mattifestation/WDACTools.git
set-ExecutionPolicy RemoteSigned
Import-Module .\WDACTools
ConvertTo-WDACCodeIntegrityPolicy C:\Windows\System32\CodeIntegrity\driversipolicy.p7b C:\Windows\System32\CodeIntegrity\driversipolicy.xml
ConvertFrom-CIPolicy -XmlFilePath C:\Windows\System32\CodeIntegrity\driversipolicy.xml -BinaryFilePath C:\Windows\System32\CodeIntegrity\driversipolicy.p7b
```
