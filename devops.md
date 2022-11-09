## 4. DevOps

1. *Usage of Docker* [John]
    - Docker should be used to support cross-architecture builds
    - Mac is a special case, can be supported by getting Mac runner (via cloud, or buying the metal)
    - Docker best practices
        - Shallow structure
            - Can be achieved using multi-stage build, where binaries produced in one container using installed tools are copied to a new clean one
        - Break long install (e.g. apt install) commands into multiple (alphabetised) commands
        - Minimise usage of root access
        - Keep your scripts OCI (open container initiative) compliant
        - All Docker images should be accompanied by a Docker compose file
        - All Docker images should be version-tagged in their Docker compose file (and versions should actually increment!)

2. issue tracking on GitLab vs Jira and GitLab-Jira integration [tbd]
    - <Simon G and Stefan to test some things out and report back>

3. *Bug reports*
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

4. *Testing*

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

5. *Dependencies*

    - Dependencies should generally be pinned to either specific versions, or specific minimal required versions, of dependent packages.
    - Any known security vulnerabilities in dependent packages should be mitigated by upgrading/downgrading to more secure versions, or (in extreme cases) switching to an alternative package.
