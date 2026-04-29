# Binary Fusion Tool 🧬

## Overview

Binary Fusion is a sophisticated technique used in malware analysis, reverse engineering, and security research to combine multiple executables into a single binary. This project implements a working example that can merge two statically-linked ELF executables while preserving their functionality and executing them sequentially.

## Features

### What is Binary Fusion?

Binary fusion combines two separate executable files into a single binary that runs both programs in sequence. Instead of running `./program1` and then `./program2`, you get one binary that executes both automatically.

### How It Works

This project implements **TWO different approaches** to binary fusion:

#### **Approach 1: Binary Embedding** (`ultimate_fuser.cpp`)
1. **ELF Header Analysis**: Parses both input binaries to understand their structure
2. **Compatibility Validation**: Ensures both binaries are 64-bit, x86-64 architecture, and statically linked
3. **Binary Embedding**: Embeds both executables as data into a new C loader program
4. **Fusion Code Generation**: Creates C code that extracts and executes both binaries sequentially
5. **Compilation**: Compiles the loader with embedded binaries into a new executable
6. **Optimization**: Uses `strip` to remove debug symbols and minimize size
7. **Error Handling**: Detects incompatible files and reports errors
8. **Comprehensive Testing**: Automated tests prove all requirements

#### **Approach 2: True Concatenation** (`conc_fuser.cpp`)
1. **Source Compilation**: Compiles source files to relocatable objects (ET_REL)
2. **Symbol Renaming**: Renames main functions to avoid conflicts (jules_main, vincent_main)
3. **Wrapper Generation**: Creates C++ wrapper that calls both renamed functions
4. **Linker Concatenation**: Links all objects into single executable using linker
5. **Direct Execution**: Executes both programs in single process without fork/exec
6. **No Temporary Files**: No temporary files in final binary
7. **True Section Concatenation**: Linker concatenates sections at object level

### Technical Approach Comparison

| Aspect | Binary Embedding (`ultimate_fuser`) | True Concatenation (`conc_fuser`) |
|--------|-------------------------------------|-----------------------------------|
| **Input** | Pre-compiled ET_EXEC binaries | Source files (.cpp) |
| **Technique** | fork/exec execution | Direct function calls |
| **Files** | Temporary files during execution | No temporary files |
| **Process** | Multi-process | Single process |
| **Complexity** | Simple and robust | More complex but "proper" |
| **Compliance** | Smart workaround | True section concatenation |

**Binary Embedding Advantages:**
- **Reliable**: No complex memory address recalculations
- **Safe**: No risk of breaking the binary structure
- **Efficient**: Only 21% size overhead vs theoretical maximum
- **Fast**: Simple and quick to implement
- **Robust**: Works with any compatible binaries

**True Concatenation Advantages:**
- **Proper**: True section concatenation as intended
- **Efficient**: No fork/exec overhead
- **Clean**: Single process execution
- **Academic**: Follows theoretical binary fusion principles

### Real-World Applications

- **Malware Analysis**: Combine multiple payloads for analysis
- **Security Research**: Create multi-stage execution environments
- **Reverse Engineering**: Study how binaries interact when combined
- **Educational**: Learn ELF format and binary manipulation techniques


### Project Structure
```
binary-fusion/
├── 📁 alkuperaiset_tiedostot/     # SOURCE CODE
│   ├── jules.cpp                   # "What do they call it?"
│   ├── vincent.cpp                 # "They call it a Royale with Cheese."
│   ├── marco.cpp                   # "Marco"
│   └── polo.cpp                    # "Polo"
│
├── 📁 fusio/                       # FUSION ENGINE
│   ├── ultimate_fuser.cpp          # Binary embedding fusion tool (C++)
│   └── conc_fuser.cpp              # True concatenation fusion tool (C++)
│
├── 📁 kaannetyt_tiedostot/         # COMPILED EXECUTABLES
│   ├── jules                       # Compiled from jules.cpp
│   ├── vincent                     # Compiled from vincent.cpp
│   ├── marco                       # Compiled from marco.cpp
│   ├── polo                        # Compiled from polo.cpp
│   ├── ultimate_fuser              # Binary embedding fusion tool
│   └── conc_fuser                  # True concatenation fusion tool
│
├── 📁 generoidut_yhdistetyt_tiedostot/  # FUSED BINARIES
│   ├── fused_jules_vincent         # jules + vincent combined
│   ├── fused_marco_polo            # marco + polo combined
│   └── fused_jules_marco           # jules + marco combined
│
├── 📁 test_files/                  # ERROR HANDLING TEST FILES
│   ├── corrupted_elf               # Corrupted ELF file
│   ├── empty_file                  # Empty file
│   ├── not_elf.txt                 # Text file (not ELF)
│   ├── test32                      # 32-bit binary (incompatible)
│   ├── libtest.so                  # Shared library (ET_DYN)
│   ├── test32.o                    # Object file (ET_REL)
│   └── epakelpo_testit.sh          # Error handling test script
│
├── 📁 testit/                      # TESTING & VALIDATION
│   └── comprehensive_demo.sh       # Proves all 12 requirements
│
├── 📄 Makefile                     # BUILD SYSTEM
├── 📄 README.md                    # DOCUMENTATION
├── 📄 review.md                    # REVIEW QUESTIONS & ANSWERS (16 questions)
└── 📄 test_results.md              # ACTUAL TEST RESULTS & PROOFS
```

## Usage

### **Step 1: Initial Setup**
```bash
# Clone the project
git clone https://gitea.kood.tech/lauralevisto/binary-fusion.git
# Navigate to the project directory
cd binary-fusion

# Compile all components (source files, fusion tools, test binaries)
make all
```

### **Step 2: Test Individual Executables**
```bash
# Test the first program (jules)
./kaannetyt_tiedostot/jules
# Expected output: What do they call it?

# Test the second program (vincent)  
./kaannetyt_tiedostot/vincent
# Expected output: They call it a Royale with Cheese.

# Test additional programs (optional)
./kaannetyt_tiedostot/marco
# Expected output: Marco

./kaannetyt_tiedostot/polo
# Expected output: Polo
```

### **Step 3: Perform Binary Fusion**

You have **TWO options** for binary fusion:

#### **Option A: Binary Embedding** (`ultimate_fuser`)
```bash
# Parameters
- `binary1`: First executable to fuse (e.g., `./kaannetyt_tiedostot/jules`)
- `binary2`: Second executable to fuse (e.g., `./kaannetyt_tiedostot/vincent`)
- `output`: Name of the resulting fused binary

# Usage
./kaannetyt_tiedostot/ultimate_fuser <binary1> <binary2> <output>

# Examples
./kaannetyt_tiedostot/ultimate_fuser ./kaannetyt_tiedostot/jules ./kaannetyt_tiedostot/vincent ./generoidut_yhdistetyt_tiedostot/fused_jules_vincent
./kaannetyt_tiedostot/ultimate_fuser ./kaannetyt_tiedostot/marco ./kaannetyt_tiedostot/polo ./generoidut_yhdistetyt_tiedostot/fused_marco_polo
```

#### **Option B: True Concatenation** (`conc_fuser`)
```bash
# Parameters
- `source1`: First source file to fuse (e.g., `alkuperaiset_tiedostot/jules.cpp`)
- `source2`: Second source file to fuse (e.g., `alkuperaiset_tiedostot/vincent.cpp`)
- `output`: Name of the resulting fused binary

# Usage
./kaannetyt_tiedostot/conc_fuser <source1> <source2> <output>

# Examples
./kaannetyt_tiedostot/conc_fuser alkuperaiset_tiedostot/jules.cpp alkuperaiset_tiedostot/vincent.cpp ./generoidut_yhdistetyt_tiedostot/fused_concatenation
./kaannetyt_tiedostot/conc_fuser alkuperaiset_tiedostot/marco.cpp alkuperaiset_tiedostot/polo.cpp ./generoidut_yhdistetyt_tiedostot/fused_marco_polo_conc
```
### **Step 4: Run the Fused Binary**

#### **Binary Embedding Results:**
```bash
# Execute the fused binary (embedding approach)
./generoidut_yhdistetyt_tiedostot/fused_jules_vincent

# Expected output:
# What do they call it?
# They call it a Royale with Cheese.
```

#### **True Concatenation Results:**
```bash
# Execute the fused binary (concatenation approach)
./generoidut_yhdistetyt_tiedostot/fused_concatenation

# Expected output:
# === Concatenation Binary Fusion - Sequential Execution ===
# 
# --- Executing first program ---
# What do they call it?
# 
# --- Executing second program ---
# They call it a Royale with Cheese.
# 
# === Fusion Complete ===
# First program exit code: 0
# Second program exit code: 0
```
### **Step 5: Clean Up (Optional)**
```bash
# Remove generated fused binaries to start fresh
rm ./generoidut_yhdistetyt_tiedostot/fused_*

# Or clean all generated and compiled files
make clean

# Recompile everything
make all
```

## Testing and Validation

### **Complete Validation Test**

```bash
# Step 1: Build everything
make all

# Step 2: Run the comprehensive demo (proves all 12 requirements)
./testit/comprehensive_demo.sh

# Step 3: Test error handling with incompatible files
./test_files/epakelpo_testit.sh


### **Dynamic Testing with Custom Binaries**

```bash
# Test with default files (jules + vincent)
./testit/comprehensive_demo.sh

# Test with custom binaries
./testit/comprehensive_demo.sh ./kaannetyt_tiedostot/marco ./kaannetyt_tiedostot/polo ./generoidut_yhdistetyt_tiedostot

# Test with any compatible binaries
./testit/comprehensive_demo.sh <binary1> <binary2> <output_dir>
```

### **Test Features**

- **🔄 Dynamic Analysis**: Automatically detects expected output from individual binaries
- **✅ Real Validation**: Compares actual fused binary output with expected results
- **🎯 Flexible Testing**: Works with any compatible ELF binaries
- **📊 Comprehensive Coverage**: Tests all 12 requirements automatically
- **🛡️ Error Handling**: Validates file existence and compatibility
- **📋 Review Documentation**: Complete answers to 16 review questions in `review.md`
- **📈 Test Results**: Actual test outputs and proofs in `test_results.md`
- **🚫 Incompatible Files**: Tests 7 different incompatible file types

    

## Tools Used

This project uses different tools depending on the fusion approach:

### **Programming Languages**
- **C++** - Main implementation language for both fusion tools (`ultimate_fuser.cpp`, `conc_fuser.cpp`)
- **C** - Fusion loader code generation and system calls (embedding approach)
- **Bash/Shell** - Test scripts (`comprehensive_demo.sh`, `epakelpo_testit.sh`) and automation
- **Makefile** - Build system configuration and dependency management

### **Required Software**
- **Compiler**: GCC 7.0+ with C++17 support
- **System**: Linux with ELF support (Ubuntu 18.04+, Debian 10+, etc.)
- **Build Tool**: `make` for automated compilation and testing

### **Core Libraries & Headers**

#### **Binary Embedding Approach** (`ultimate_fuser.cpp`)
```cpp
// System Libraries (C++)
#include <iostream>      // Standard I/O operations
#include <fstream>       // File stream operations
#include <vector>        // Dynamic array containers
#include <string>        // String handling
#include <cstring>       // C string functions
#include <elf.h>         // ELF format definitions
#include <sys/mman.h>    // Memory mapping
#include <unistd.h>      // System calls (fork, exec, wait)
#include <fcntl.h>       // File control operations
#include <sys/stat.h>    // File status information
#include <cassert>       // Assertion macros
#include <algorithm>     // STL algorithms
#include <map>           // STL map containers
#include <cstdlib>       // C standard library
#include <sstream>       // String streams

// System Libraries (C - Fusion Loader)
#include <stdio.h>       // Standard I/O
#include <stdlib.h>      // Standard library functions
#include <sys/wait.h>    // Process waiting functions
#include <string.h>      // String manipulation
```

#### **True Concatenation Approach** (`conc_fuser.cpp`)
```cpp
// System Libraries (C++)
#include <iostream>      // Standard I/O operations
#include <fstream>       // File stream operations
#include <string>        // String handling
#include <cstring>       // C string functions
#include <cstdlib>       // C standard library
#include <sstream>       // String streams
#include <vector>        // Dynamic array containers
#include <algorithm>     // STL algorithms
#include <unistd.h>      // unlink system call
#include <sys/stat.h>    // chmod system call
```

### **Binary Analysis Tools**

#### **Used by Both Approaches**
- **`readelf`** - ELF file structure analysis and header inspection
- **`objdump`** - Object file disassembly and section analysis
- **`file`** - File type detection and format identification
- **`ldd`** - Library dependency checking for static/dynamic linking
- **`stat`** - File statistics and metadata analysis

#### **Linker Tools (Critical for Binary Fusion)**
- **`ld` (GNU Linker)** - Core linking engine that performs:
  - **Symbol Resolution**: Resolves undefined symbols between object files
  - **Address Assignment**: Assigns absolute addresses to functions and variables
  - **Section Merging**: Combines sections with same names (.text, .data, .rodata)
  - **Relocation**: Updates relative addresses to absolute addresses
  - **Memory Layout**: Creates optimal memory layout for execution
  - **Dead Code Elimination**: Removes unused functions and variables
  - **Padding Optimization**: Eliminates unnecessary padding between sections
- **`gcc/g++ -static`** - Invokes ld with static linking flags
- **`objcopy`** - Symbol manipulation and binary format conversion

#### **Binary Embedding Specific Tools**
- **`objcopy`** - Binary manipulation and embedding (converts executables to object files)
  - `objcopy --input binary --output elf64-x86-64` - Converts executables to embeddable objects
- **`gcc`** - C compiler for fusion loader compilation with static linking
  - **`ld` (via gcc)** - Links C loader with embedded binary objects, resolves embedded data symbols
- **`strip`** - Removes debug symbols for size optimization

#### **True Concatenation Specific Tools**
- **`g++`** - C++ compiler for source compilation and linking
  - `g++ -c -fPIC` - Compiles source files to position-independent relocatable objects
  - `g++ -static` - Creates statically linked executables
- **`objcopy`** - Symbol renaming for conflict resolution
  - `objcopy --redefine-sym main=function_name` - Renames main functions to avoid conflicts
- **`ld` (via g++)** - Performs true section concatenation:
  - Merges .text sections from wrapper.o + jules.o + vincent.o
  - Merges .data and .rodata sections from all object files
  - Resolves symbol references (jules_main, vincent_main)
  - Assigns absolute addresses to all functions
  - Optimizes memory layout and eliminates padding
- **`strip`** - Removes debug symbols for size optimization

### **Development & Debugging Tools**
- **`gdb`** - GNU debugger for binary debugging
- **`chmod`** - File permission management
- **`hexdump`** - Binary content inspection and validation
- **`timeout`** - Test script execution timeouts


## Security Considerations

This tool is designed for **educational and research purposes** in cybersecurity and should be used responsibly. Understanding binary manipulation techniques is crucial for both offensive and defensive security practices.

### **Security Applications**

- **🛡️ Malware Analysis**: Understanding how malicious binaries can be combined or obfuscated
- **🔍 Reverse Engineering**: Learning ELF format internals and binary structure analysis
- **⚔️ Ethical Hacking**: Tools for legitimate security research and penetration testing
- **🛡️ Defense**: Understanding attack vectors to develop better protection mechanisms
- **🔬 Research**: Academic study of binary manipulation techniques

### **Responsible Usage**

- **Educational Purpose**: Use only for learning and research
- **Legal Compliance**: Ensure usage complies with local laws and regulations
- **Ethical Guidelines**: Follow responsible disclosure practices
- **Controlled Environment**: Test only on systems you own or have permission to test

## Author

Laura Levistö       
9 / 2025