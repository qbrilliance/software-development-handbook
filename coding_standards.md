## 2. Coding standards

1. Code formatting & standards

    a. Code format [Johannes]
      
      Code should be formatted at foremost in a consistent way in a project but must be in a file. There are many different formats used (see https://github.com/motine/cppstylelineup for a comparison of the most popular ones) and we stick to google format (https://google.github.io/styleguide/cppguide.html). The guide contains much more than just formatting, e.g. naming, comments, function and class design, header files, scoping, advanced language features/construct and common pitfalls. Feel free to browse the guide once at your startup and use it for later reference, e.g. in code reviews. We will extend the set of C++ guidelines locally to avoid common pitfalls and generally increase code quality - e.g. design patterns, memory management e.g. unique_ptr, common pitfalls e.g. braces after if’s, etc. Our guidelines won’t cover the order of private/public & where things reside - we’ll leave that up to the designer of the module.
      
      Code formatting:
      You do not need to remember the format rules, you can let the IDE enforce it for you (on save). The vscode configuration: 
        - install extension clang-format by xaver under extensions (see https://github.com/xaverh/vscode-clang-format)
        - in settings.json add {"editor.formatOnSave": true}
        - add .clang-format file in folder or parent folder with "BasedOnStyle: Google" (see https://clang.llvm.org/docs/ClangFormatStyleOptions.html)
      The only exception is if there is an upstream project using a particular style, we will follow that style.
      Comment blocks should be compatible with doxygen (https://doxygen.nl/) and not altered by clang-format. To enforce this behavior 
        - in .clang-format add "CommentPragmas:  '^[^ ]'"

      For Python code the same principle is applied as above (https://google.github.io/styleguide/pyguide.html).
      The Google style guide recommends YAPF (https://github.com/google/yapf/), a project based on clang-format which aims to additionally make the code look good instead of only removing linter errors. Black formatter (https://github.com/psf/black) is another option.

    b. linting [Johannes]
      
      A linter is a static code analysis tool used to flag programming errors, bugs, stylistic errors and suspicious constructs. Various programs with specialized search methods exist and it is beneficial to use more than one linter at the same time to increase coverage. Clang-tidy is already included in the C++ extensions for vscode (see https://devblogs.microsoft.com/cppblog/visual-studio-code-c-december-2021-update-clang-tidy/).
      Other linters are available as extensions and must be installed to the system, e.g. cpplint for C++ or pylint and pylance (with type checking) for Python. Installation instructions are given in the extensions pages.
      For inherited code bases it is not feasible to deal with all issues raised but new code should address as many as possible. A linter warning can only be ignored with a documented reason.
      We may enforce linter coverage by merge request CI job on Gitlab (drastically non-conformant code fails CI test).

    c. commenting and documentation [Johannes]
      
      An intro from Google Style guide: Comments are absolutely vital to keeping our code readable. The following rules describe what you should comment and where. But remember: while comments are very important, the best code is self-documenting. Giving sensible names to types and variables is much better than using obscure names that you must then explain through comments. When writing your comments, write for your audience: the next contributor who will need to understand your code. Be generous — the next one may be you!"

      Code is more often read than written. The coding and comment style should respect this to improve efficiency in maintenance, debugging, extension and refactoring processes. 

      In short: 
        - good coding style helps a lot: use meaningful and consistent naming scheme, well encapsulated code, clear control flow etc.
        - try to avoid unnecessary complex structures
        - add comments to lines or blocks where intend may not be obvious
        - class, struct and function declaration should have an accompanying comment that describes what it is for and how it should be used with input/output 
        - Interface documentation & comments to be in the header (params, return types, class comment), cpp files have working code comments (e.g. intent of code block).
        - new python code should use type annotations
        - use Doxygen style (https://doxygen.nl/)

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

    g. compiler switches and warnings [Johannes]

      Modern compilers give useful information (warnings) if standard is violated. These things tend to produce error later down the line and should be addressed immediately. The flags and there interpretation are compiler dependant:
      - for GCC: https://clang.llvm.org/docs/DiagnosticsReference.html
      - for clang: https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html
      Especially in new code we should tread every warning as error. A minimal set of flags should be used:
      * -Werror
      * -Wall
      * -Wextra

      Additionally clang offers an option to enable all (useful) warnings with "-Weverything". GCC does not offer that functionality since it has warnings like "-Wtraditional" which would enforce very old code style. You can check warning status with "gcc -Wall -Wextra -Q --help=warning".

      Any warning you are willing to accept in QB code should be disabled with a push pragma and pop it as soon as practical. Warnings we are willing to accept in dependent packages could instead be suppressed using compiler flags that target specific warnings across the full package.  Both these routes should be accompanied by a comment documenting why we’re OK with this warning.

2. object-oriented programming practices [John]

    - Consider putting setters and getters in a separate header/source file (potentially with pybind bindings too)
    - additional topics to be discussed when John H next joins

3. procedural programming practices [Leigh]

    - Test function inputs for validity! Can be done via
      - assertions (better for internal inputs)
      - raising and catching exceptions (better for dealing with external inputs), and mitigating accordingly
    - additional topics to be discussed when Leigh next joins
    - C++23 offers function contracts (could be used if/when our code supports only more recent compilers)
