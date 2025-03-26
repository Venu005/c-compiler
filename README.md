# C compiler

This project implements a lexical analyzer and parser using Flex and Yacc, designed to generate intermediate code (ICG) from C source files. It comes with a set of test cases to validate the implementation. The project includes all necessary source files and a shell script to automate compilation and testing.


## Overview

This project is a complete solution for performing lexical analysis and parsing of C-like source files, ultimately generating intermediate code. The core components are:

- **Flex file (`lexer.l`)**: Defines the tokens and scanning rules.
- **Yacc file (`parser.y`)**: Contains grammar rules and parser actions.
- **Shell script (`run.sh`)**: Automates the build process and sequentially tests all test case files.

The generated executable is run against a series of test cases located in the designated folder.

## Directory Structure

Below is an example of the project layout: <br/>
.├── lexer.l # Flex source file (lexical analyzer) <br/>
.├── parser.y # Yacc source file (parser grammar) <br/>
.├── run.sh # Bash script to compile and run tests.<br/>
.├── testcases/ # Folder containing test case files (e.g., test1.c, test2.c, ...)

---

## Features

1. **Lexical Analysis**: Tokenizes input code and detects invalid identifiers, unmatched comments, etc.
2. **Syntax Parsing**: Implements a context-free grammar (CFG) for parsing C constructs.
3. **Semantic Analysis**:
   - Symbol Table Management (variables, functions, arrays).
   - Type Checking.
   - Scope Management (nesting levels).
   - Duplicate Declaration Checks.
   - Function Parameter Validation.
4. **Intermediate Code Generation**:
   - Arithmetic/Logical Operations.
   - Control Structures (if-else, loops).
   - Function Calls and Returns.
5. **Target Code Generation**:
   - On successful parsing a target code is generated.
6. **Error Handling**:
   - Invalid array sizes.
   - Type mismatches.
   - Undeclared variables/functions.
   - Return type validation.

---

## Installation Steps

### Prerequisites

- Install **Flex** and **Bison**:
  ```bash
  sudo apt-get install flex bison
  ```


## Run the project
```bash
chmod +x run.sh
./run.sh
```


## How It Works

### 1. Lexical Analysis (`lexer.l`)
- **Tokenization:** 
  - Tokenizes keywords (e.g., `int`, `while`), identifiers, constants, and operators.
- **Detection:**
  - **Invalid identifiers:** For example, `123a` in `test4.c` is flagged.
  - **Unmatched comments/strings:** Identifies errors in the source.
  - **Preprocessor directives:** These are detected but ignored in this implementation.

### 2. Syntax Parsing (`parser.y`)
- **Parsing using Bison grammar:** 
  - Handles variable and function declarations.
  - Processes control structures such as `if-else` and loops.
  - Evaluates expressions and assignments.
- **Operator Precedence:** 
  - Implements rules for operator precedence and associativity to correctly parse expressions.

### 3. Semantic Analysis
- **Symbol Table:**
  - Maintains details about variables and functions, including type, scope, and parameters.
- **Type Checking:**
  - Ensures that operations (e.g., `a + b`) involve compatible types.
- **Scope Management:**
  - Manages variable scopes, ensuring that variables are confined to their respective blocks (e.g., within a function body).
- **Error Checks:**
  - **Duplicate declarations:** For example, detecting `int x = 2; int x = 3;`.
  - **Function argument mismatch:** For instance, catching errors like calling `modulus(b)` instead of `modulus(b, a)` as seen in `test3.c`.

### 4. Intermediate Code Generation (ICG)
- **Three-Address Code:**
  - Generates code for arithmetic operations (e.g., `t1 = a + b`).
  - Manages control flow with labels (e.g., `L0`, `L1` for loops and conditionals).
  - Processes function calls (e.g., `call modulus, 2`).
- **Example Output for `test1.c`:** <br/>
```bash
func begin main
 t0 = 3
 t1 = 2 
 L0: IF not (x > y) 
   GoTo L1 ...
func end
````

This code illustrates:
- The beginning of the main function with `func begin main`.
- Temporary variables (`t0`, `t1`) holding computed values.
- A control flow label `L0:` marking the start of a loop or conditional block.
- A conditional jump instruction that evaluates `IF not (x > y)` and branches to label `L1`.
- The end of the function with `func end`.

### 5.Target Code Generation


The target generation phase is responsible for converting the generated intermediate code into target-specific code that can be executed on your machine. This phase is typically performed after intermediate code generation (ICG) and may involve several sub-steps, including analysis, optimization, and code generation.

## Steps in Target Generation

1. **Intermediate Code Analysis**
   - Analyze the generated three-address code to identify opportunities for optimization and to prepare for translation into machine-specific instructions.

2. **Optimization (Optional)**
   - **Local Optimizations:** Simplify arithmetic operations (e.g., constant folding, strength reduction).
   - **Global Optimizations:** Remove dead code, optimize control flow, and streamline variable usage.

3. **Register Allocation**
   - Map temporary variables used in the intermediate code to the physical registers of the target machine.
   - Use techniques such as graph coloring to minimize register spills and maximize efficiency.

4. **Instruction Selection**
   - Translate the intermediate instructions into target-specific assembly instructions.
   - Convert high-level operations (arithmetic, control flow, function calls) into corresponding machine instructions.

5. **Assembly Code Generation**
   - Generate assembly code based on the target machine's instruction set architecture (ISA).
   - Ensure that the assembly code follows the syntax and conventions of the target assembler.

6. **Linking**
   - Assemble the generated assembly code into object files.
   - Link object files with necessary libraries and other object files to produce the final executable.

