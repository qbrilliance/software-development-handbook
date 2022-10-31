## 2. Coding standards

1. Code formatting & standards

    a. Code format [Johannes]

      - C++
        - Use qbOS google or chromium format (publish the clang-format config)
        - Spaces (not tabs) using the google guideline
        - Suggest running format on save, but don’t enforce project’s do it
        - variable naming style: Use the google style guides <insert link>
        - Provide formatting tools in the IDE to make this as easy as possible (e.g. clang-format)
        - If there is an upstream project is using a particular style, we will follow that style.
        - Within a file, the formatting MUST be consistent, within a code base the formatting SHOULD be consistent.
        - We should develop a set of C++ guidelines to avoid common pitfalls and generally increase code quality - e.g. design patterns, memory management e.g. unique_ptr, common pitfalls e.g. braces after if’s etc
        - Our guidelines won’t cover the order of private/public & where things reside - we’ll leave that up to the designer of the module.
        - Come to a decision on a commenting style that works for IDEs, it might be doxygen e.g. “//! \param “ but it might also be something more fundamental - need to investigate what makes the IDEs see the comments (see below, “doxygen preferred”).

      - Python
        - Use Black formatter with line length adjustment
          - Spaces not tabs
          - Suggest format on save
        - New python code should use type annotations

    b. linting [Johannes]

      - C++ - suggest using clang-tidy in the IDE - no rules about enforcing they are dealt with (too much effort for an inherited code base). New code should address as many as possible.
      - Py - Suggest using a linter/language server that includes type checking (e.g. mypy, pylance)
          - Linter warnings should be addressed or ignored with a documented reason.
      - Enforced by merge request CI job on Gitlab (drastically non-conformant code fails CI test)

    c. commenting and documentation [Johannes]

      - Good code helps to comment itself. This means good, meaningful and consistent naming scheme, well encapsulated code, clear control flow etc etc.  This does *not* mean that good code makes commenting redundant.
      - Intent is not present in code. Comments shouldn’t say how the code works, but what function it fulfils. Comments can also be left to say what the code *isn’t* doing. Essentially comments should be used to upload the context of the author’s mindset into the maintainer or reviewer's brain.
      - Interface documentation & comments to be in the header (params, return types, class comment), cpp files have working code comments (e.g. intent of code block).
      - Doxygen preferred

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

      - All compiler warnings should be addressed
      - `-Werror` should be turned on
      - Any warning you are willing to accept in QB code should be disabled with a push pragma and pop it as soon as practical. Warnings we are willing to accept in dependent packages could instead be suppressed using compiler flags that target specific warnings across the full package.  Both these routes should be accompanied by a comment documenting why we’re OK with this warning.

2. object-oriented programming practices [John]

    - Consider putting setters and getters in a separate header/source file (potentially with pybind bindings too)
    - additional topics to be discussed when John H next joins

3. procedural programming practices [Leigh]

    - Test function inputs for validity! Can be done via
      - assertions (better for internal inputs)
      - raising and catching exceptions (better for dealing with external inputs), and mitigating accordingly
    - additional topics to be discussed when Leigh next joins
    - C++23 offers function contracts (could be used if/when our code supports only more recent compilers)
