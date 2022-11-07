## 2. Coding standards

1. Code formatting & standards

    a. Code format
      
      - If we are modifying an upstream project that uses a particular style, we will follow that style; otherwise, all rules in this section apply.
      - Code formatting _must_ be consistent within individual files.  
      - Ideally, formatting should also be consistent across whole projects.  
      - Spaces should always be used instead of tabs.
      - Format rules should be applied automatically, using `clang-format`.  
      - If using an IDE, let it enforce the format rules for you on save. For example, with vscode: 
        - install extension [`clang-format`](https://github.com/xaverh/vscode-clang-format) under "extensions".
        - in `settings.json` add `{"editor.formatOnSave": true}`
        - add a `.clang-format` file in the code folder or its parent, containing the text "`BasedOnStyle: foo`" with `foo` the chosen style (see [https://clang.llvm.org/docs/ClangFormatStyleOptions.html](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)).
      - In order to enforce aherence of the main branch to the formatting standards, repositories should implement relevant post-push hooks or CI jobs that run on merge requests.
      - C++-specific rules:  
        - Use the [Google style](https://google.github.io/styleguide/cppguide.html). 
        - Comment blocks should be compatible with [doxygen](https://doxygen.nl/) and not altered by clang-format. To enforce this behavior, add "`CommentPragmas:  '^[^ ]'`" to `.clang-format`.
        - Rules do not cover relative ordering of private, protected and public members within classes; this is left up to the developer.
        - See later sections of this page for explicit rules regarding file lengths and placement of code in headers and source files.
        [//]: # ([TODO] extend the set of C++ guidelines to avoid common pitfalls and generally increase code quality - e.g. design patterns, memory management, unique_ptr, common pitfalls such as lack of braces after if statement, etc.)  
      - Python-specific rules:
         - Use the [Black formatter](https://github.com/psf/black).

    b. Linting
      
      - For C++, use `clang-tidy`.  This is already included in e.g. the [C++ extensions for vscode](https://devblogs.microsoft.com/cppblog/visual-studio-code-c-december-2021-update-clang-tidy/).
      - For Python, use a linter with type checking, such as `mypy`, `pylint` or `pylance`.
      - Linter warnings should be addressed by default, and only ignored with a documented reason.
      - Some loose conformance to linter recommendations should be enforced by CI jobs that run on merge requests (i.e. drastically non-conformant code fails the CI test).

    c. commenting and documentation

      - A useful comment from the Google Style guide:  
        *When writing your comments, write for your audience: the next contributor who will need to understand your code. Be generous — the next one may be you!*  
      - Good coding style helps a lot: use meaningful and consistent naming schemes, clear control flow, and extensive encapsulation.
      - Whilst good code helps to document itself, this never completely removes the need for good documentation and commenting.
      - Avoid unnecessarily complex structures.
      - Add comments to lines or blocks where the intent or function of the code may not be obvious.
      - Aim for a line of commenting every few lines of code.
      - Class, structure and function declarations should have an accompanying comment that describes what the new construct is for and how it should be used ( including input/output). 
      - Documentation of the API of a C++ class/struct/function should be in the header.  This includes any parameters, return types, and overall comments about the class.  The source file(s) should have comments on the actual working code (e.g. intents of code blocks).
      - New Python code should come with type annotations.
      - Use [Doxygen style](https://doxygen.nl/).

    d. C++ code in headers vs source files

      - Headers (`.hpp`) and sources (`.cpp`) should be used for declarations (i.e., interfaces) and definitions (i.e., implementations) accordingly.
      - A header file should not contain class object definitions (implementation definitions of member functions) or function definitions. This guarantees conformance to one-definition rule (ODR), preventing potential linkage errors.
      - Specifically, a header file should normally only contain:
        - `#include` statements that include other header files
        - templates (including the definitions)
        - class and function *declarations*
        - `inline` function definitions.
      - It is also recommended that the `.cpp` file should include the header file(s) that define the interface(s) to the definitions implemented therein, so that the compiler can check their consistency early on, rather than leaving it to the linker.
      - Exceptions where implementation of code inside header files is acceptable:
        - The whole package is **intended** to be fully `header-only`.
        - The function implementation consists of only a single line.  In this case, the implementation should be preferably placed inline in the declaration.

    e. C++ source / header length

      - We should aim to keep header and source files reasonably short. Long files are an indicator of design problems.
      - As an initial rule of thumb, restrict class declarations and definitions to one per header/source file.
      - If needed, we should consider separating sources and headers into multiple shorter files.
      - It is also recommended to try to keep method bodies relatively short for readability. This could be achieved, for example, by splitting methods into private helper methods.

    f. Source preamble

      - The intent of preamble text (as comments in the code) is a to provide general identification so that if the source is seen out of context, it can be immediately identified as QB code.
      - At a minimum, the preamble should include:
        - Python: "Copyright Quantum Brilliance" (no date) & module intent. 
          *Example*:  
          ```python
          # Copyright Quantum Brilliance
          """
          Arithmetic Utilities
          """
          ```
        - C++: "Copyright Quantum Brilliance" (no date), intent & package. 
          *Example*:  
          ```Cpp
          // Copyright Quantum Brilliance
          // This file implements a singleton thread pool class based on std::thread
          ```

    g. compiler switches and warnings

      - For both gcc and clang, all warnings should be enabled and treated as errors using`-Werror -Wall -Wextra`.
      - Every warning should be treated as bug, and addressed.
      - Any warning that cannot be addressed should be disabled with a push pragma that is popped as soon as practical.
      - Warnings that we deem acceptable in dependent packages should instead be suppressed using compiler flags that target specific warnings across the full package.
      - Both these mitigations should be accompanied by a comment documenting why we have suppressed rather than directly addressed this warning.

2. object-oriented programming practices [John]

    - Consider putting setters and getters in a separate header/source file (potentially with pybind bindings too)
    - additional topics to be discussed when John H next joins

3. procedural programming practices [Leigh]

    - Test function inputs for validity! Can be done via
      - assertions (better for internal inputs)
      - raising and catching exceptions (better for dealing with external inputs), and mitigating accordingly
    - additional topics to be discussed when Leigh next joins
    - C++23 offers function contracts (could be used if/when our code supports only more recent compilers)
