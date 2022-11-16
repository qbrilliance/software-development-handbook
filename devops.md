## 4. DevOps

1. *Usage of Docker*
    - CI pipelines will use containers in order to certify software product validity across various OSs and architectures. We use Docker containers for this purpose.
    - MacOS is a special case, and can be supported by getting access to a Mac runner (whether via the cloud, or buying the Mac hardware ourselves).
    - Docker best practices:
        - Containers should be designed with a shallow structure with minimal layers, i.e. as few RUN directives as possible. Consider putting multiple commands in each RUN directive in order to facilitate this.
        - RUN directives with multiple commands should be split up, with one line per command.  New lines should preferably begin with `&&` or a similar shell command concatenator.
        - Consider splitting long commands with many flags over multiple lines, placing each flag on its own line.
        - Break long install (e.g. apt install) commands across multiple lines, with one package per line.  Alphabetise package install ordering.
        - Minimise usage of root access and do not distribute images that run the entrypoint as root. *This can be a severe security issue.*
        - Keep Dockerfiles compliant with the OCI (open container initiative).
        - Docker images should be accompanied by a Docker compose file wherever possible, to demonstrate/document proper usage.
        - All Docker images should be version-tagged, preferably in their Docker compose file (with versions that actually increment when changes are made).


2. *Bug reports*
    - Bugs found in non-QB public libraries should be reported directly on those repositories.
    - Bugs found in QB public repositories should be reported directly on the public repository, migrated to internal systems (e.g.\ Jira) for resolution, and then marked as resolved on the public repository once the fix has been released publicly.
    - Bugs found in QB private repositories should be reported directly to the private repository.
    - A template for bug reports to Quantum Brilliance projects is provided below. Using it is encouraged but optional.
```markdown
# [Title]
A concise but specific description of the bug.

### Steps to reproduce
Describe what needs to be done to reproduce the faulty behavior.

### Resulting and expected behavior
A brief description of what you expected and what you actually got.

### Self help
If you found a workaround or tried to fix the bug yourself, please provide your attempt here.

### Technical output
If available, provide a traceback, logs or similar. Console output is helpful here.

### Screenshot
If applicable and helpful, provide a screenshot.

### System information and environment
- Software version:
- Operating system:

Please provide additional information if applicable, for instance on which experimental setup the bug happened.
```

3. *Testing*

    a. *Definitions*  

      - **Verification**: testing that the code satisfies all relevant software / product *design requirements and specifications*.  Examples:  
          - Comparison of library APIs against design specifications  
          - Comparison of code functionality against requirements  
          - Usability testing (internal and external)  
          - Customer testing  
      - **Validation**: testing that the outputs of the code are *correct*.  Tests should compare with known, expected results.  Expected results may be in the form of diagnostic/logged outputs and/or numerical comparison of results to expected values, within appropriate numerical tolerances. Test types:  
          - **Unit tests**: validation that individial code units (usually individual functions or classes) produce expected results.  
          - **Component tests**: validation that package components build and install correctly, and produce expected results.  
          - **Integration tests**: validation that systems comprised of multiple software and/or hardware components produce the expected results when operating as a system.  
          - **Regression tests**: tests that should produce the same results when run using new versions of the software as they did when run using previously validated versions.  

    b. *General rules*  

      - All tests are to be automated to the greatest extent possible.  All validation tests should be automated. Some verification tests may need to be done manually (e.g. usability testing).  
      - Verification and validation tests are both to be done as part of CI (continuous integration), i.e. *continuously* throughout the software development process, at the time of delivery to customers, and thereafter as part of code maintenance.  

    c. *Unit tests* <a name="unit_tests"></a> 

      - All repositories must include unit tests and unit-level regression tests.
      - Addition of new functions and/or classes must include the addition of associated unit tests.  
      - Coverage and reporting:  
          - Coverage metrics should be reported for all repositories on the basis of unit tests, using the [built-in Gitlab CI](https://docs.gitlab.com/ee/ci/testing/test_coverage_visualization.html) as much as possible.  
          - A goal coverage level should be agreed on a per-project basis.  
      - Maintenance:
        - The unit test battery should be culled periodically to a manageable size; older tests should be disabled in automated jobs (but not deleted) in order to achieve a sensible balance between code coverage and CI runtime.
        - Authors adding new tests may disable older ones as a maintenance measure.
        - CI system maintainers should inspect the test battery on a regular (e.g. monthly) basis and perform any culling deemed necessary.

    d. *Component tests*  

      - Component repositories must include component tests (and thus component-level regression tests).  

    e. *Integration tests*

      - Integration tests and integration-level regression tests may be implemented in the repos of top-level components and/or as a standalone integration testing framework residing in a dedicated repository.  
      - Integration tests should include hardware wherever possible.  

    f. *Regression tests*  

      - To be implemented when new code is added by automatically repeating unit, component and/or integration tests used to validate previous code versions.

    g. *Security testing*

      - To the extent possible, regression tests should also aim to target security.
      - [Automated Gitlab dependency scanning for security defects](https://docs.gitlab.com/ee/user/application_security/dependency_scanning/) must be activated for all respositories.  

    h. *CI pipeline design*

      - CI pipelines should be fully automated.  
      - The build and test steps in pipelines should respectively build and perform tests only.  This is to ensure modularity of the testing pipelines, in part to properly facilitate cross-platform testing.  
      - CI pipeline authors are strongly encouraged to borrow liberally from other Teams' CI scripts.  
      - Pipelines are to include multiple tests aimed at different platforms.  Targets should differ in hardware, compilers, OS, library versions or any other relevant characteristic across which diversity of deployments is desirable.  
      - CI pipelines should normally be implemented using Docker.

4. *Dependencies*

    - Dependencies should generally be pinned to either specific versions, or specific minimal required versions, of dependent packages.
    - Any known security vulnerabilities in dependent packages should be mitigated by upgrading/downgrading to more secure versions, or (in extreme cases) switching to an alternative package.
