# CppProjectTemplate - Library

This is a template for C++ and C **library** projects.

## File structure
```
.
├── README.md
├── CMakeLists.txt
├── CMakePresets.json
├── examples
│   └── exmaple.cpp
├── include
│   └── MyLib
│       └── mylib.hpp
└── src
    └── mylib.cpp
```

## Dependencies
- [CMake](https://cmake.org/)
- C++ compiler

## Build and Run
> [!NOTE]
> CMake options:
> - `MYLIB_BUILD_SHARED_LIBS`: "OFF"
> - `MYLIB_BUILD_EXAMPLES`: "OFF"
>
> Change the options in [CMakePresets.json](CMakePresets.json)

### Ninja
> Debug
> ```bash
> cmake --preset Ninja-Debug
> ```
> ```bash
> cmake --build --preset Ninja-Debug
> ```

> Release
> ```bash
> cmake --preset Ninja-Release
> ```
> ```bash
> cmake --build --preset Ninja-Release
> ```

> [!NOTE]
> Executables: `build/Ninja/<config type>/bin`<br>
> Lib files: `build/Ninja/<config type>/lib`

### Visual Studio (Windows only)
```bash
cmake --preset Vs22
```

> Debug
> ```bash
> cmake --build --preset Vs22-Debug
> ```

> Release
> ```bash
> cmake --build --preset Vs22-Release
> ```

> [!NOTE]
> Executables: `build/Vs22/<config type>/bin`<br>
> Lib files: `build/Vs22/<config type>/lib`
