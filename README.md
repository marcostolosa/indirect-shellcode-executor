# Indirect-Shellcode-Executor

## Description

Indirect-Shellcode-Executor expoits the miss-configuration/vulnerability present on the API Windows method *ReadProcessMemory* discovered by *DarkCoderSc*.

It exploits the nature of the in/out pointer param named **lpNumberOfBytesRead*, that enables to write into process memory without calling common API methods to do so such as memcpy, this is perfect for AV and EDR detection evasion

```C++
BOOL ReadProcessMemory(
  [in]  HANDLE  hProcess,
  [in]  LPCVOID lpBaseAddress,
  [out] LPVOID  lpBuffer,
  [in]  SIZE_T  nSize,
  [out] SIZE_T  *lpNumberOfBytesRead      <----------------------------- Vulnerable param
);
```

This is my own implementation written y RUST, with the difference that this is a fully developed tool, that can directly be used on red team operations as a POC.

## Download

Just go the release section of the repo and download the latest version

```
https://github.com/mimorep/Indirect-Shellcode-Executor/releases
```

## Compile your self

Note that the final binary is in fact a x32 bits, so if you want to compile the binary yourself just run the next command with cargo:

To add support for the x32 compilation:

```Powershell
rustup target add i686-pc-windows-msvc
```

To compile the binary:

```Powershell
cargo build --target i686-pc-windows-msvc --release
```

### Credits

Big kudos to Jean-Pierre LESUEUR (DarkCoderSc) for discovering the pointer vulnerability and posting it to the unprotect.it project, you can contact him here:

https://unprotect.it/users/public/profile/darkcodersc/