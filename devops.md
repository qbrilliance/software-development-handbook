## 4. DevOps

    1. Platform support via Docker [John]
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

    3. best practice bug reporting [Stefan]
        - a bug report template should be provided (use is optional but encouraged)
        - should include
            - minimal information about how to reproduce the bug
            - expected output
            - what you got instead
            - platform and software version info about where you saw the bug
            - details of any fixes already attempted and/or workarounds found
        - bugs on public (Gitlab) repos should be encouraged to be raised directly on those (and then migrated internally to whatever private repo / Jira board is most relevant)

    4. Automated testing [Pat]
        a. Unit, component and integration tests should be implemented, and kept separate to each other.  These should target both regression (testing that you get the same result as in previous versions) and validation (testing that you get the right result).  Verification (testing that software requirements are satisfied) as well once we actually have written requirements documents.
        b. Unit test coverage and reporting
            - unit tests should exist in all packages; new merges should add tests for new functionality
            - coverage metrics should be reported for all unit tests, using built-in Gitlab CI as much as possible ([https://docs.gitlab.com/ee/ci/testing/test_coverage_visualization.html](https://docs.gitlab.com/ee/ci/testing/test_coverage_visualization.html))
            - A goal coverage level should be agreed later, or project by project
        c. Package security testing
            - turn on automated Gitlab dependency scanning ([https://docs.gitlab.com/ee/user/application_security/dependency_scanning/](https://docs.gitlab.com/ee/user/application_security/dependency_scanning/))
        d. CI design
            - Build and test steps should do only that (in part to properly facilitate cross-platform testing)
            - We are all encouraged to steal liberally from other groups’ CI scripts
            - Should include hardware wherever possible
            - Multiple tests for different targets (hardware, compilers, OS, library versions).  Automated most effectively via Docker.
