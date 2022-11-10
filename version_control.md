## 3. Version control

1. *Location of repositories*  
QB repositories are hosted on Gitlab, within [the qbau organisation](https://gitlab.com/qbau).

2. *Layout of qbau folders*  

    - Each team has its own folder within the [qbau](https://gitlab.com/qbau) organisation.  The folder is named after the team and contains the software repositories maintained by that team.  This is referred to in Gitlab as a 'group'.  These folders may contain subfolders, for example for specific projects.  
    - There is also an additional [company-wide](https://gitlab.com/qbau/company-wide) folder. This contains repositories maintained by the company as a whole, such as the repository in which this handbook resides.  
    - No repositories should exist outside of these folders, i.e. in the root of the qbau organisation.

3. *Gitlab organisation membership and group/repository access levels*  

    - The IT Team is solely responsible for adding new users to the [qbau](https://gitlab.com/qbau) Gitlab organisation.  
    - IT will set all users' access level at the organisation level to 'Developer'.  This gives all QB employees the ability to read, raise issues on and lodge merge requests to all repos of all teams in QB.  
    - IT should give Team Leads 'Owner' level permissions for the groups (i.e. folders) that they administer.  Team Leads may upgrade any users' access levels within their Gitlab group, a specific sub-group or a repository in their group.

4. *Folder structure within repositories*  

    - The preferred structure is for source files to be placed in a `src` folder, and headers in an `include` folder. Other source components (tests, examples, data, etc) may have their own dedicated folders at the same level as `src` and `include`.
    - Flat file structures are to be preferred over heavily nested ones. File links (whether hard or symbolic) are strongly discouraged.  
    - The only exception to the above is that headers should always be placed in a folder structure that provides an include namespace.  For example, header files in the QB SDK core reside in core/include/qb/core, and are included in C++ as `#include "qb/core/foo.hpp"`.  
    - When using cmake, `CMakeLists.txt` files are to be designed to allow out-of-source builds.  All builds in CI jobs and in released scripts should be done out-of-source.

5. *Naming of git branches*  

    - `X-Y-Z`, with `X` a Jira or Gitlab issue code addressed by the branch, `Y` an informative name for the branch, and `Z` an optional date in the format YYYYMMDD.
    - The inclusion of a username as a leading namespace (”user-name/branch-name") is also encouraged, especially when either a) there is no Jira/Gitlab issue code, or b) multiple people have branches addressing the same Jira/Gitlab ticket.

6. *Development flow with git* [Simon]

    - Preference for feature branches + main, with rolling releases done from main
    - Per repo decisions to have longer-term feature-specific development branches
    - No release branch for continuously deployed repos; repos with a longer release & testing process to add a release branch, and apply hotfixes there

7. *Merge requests and code review*

    a. Preparation of merge requests   [Simon]  

      - Feature must be fully ready for merge before requesting review (i.e. not in ‘Draft’ format)  
      - New functions and classes should not be considered ready for merge without corresponding unit tests (see [unit tests](devops.md#unit_tests)).  
      - A merge template should exist (should be provided in this handbook), explaining what is expected in a merge request.  A checklist that should form part of that template:
        - [ ] (unit)tests are provided
        - [ ] relevant methods have docstrings
        - [ ] docs/examples/etc in the repository were updated
        - [ ] `README.md` was updated
        - [ ] `CHANGELOG.md` was updated (if target is `main`)
  
      - Merge request must contain: a short summary of the solution, any new tests added, statement addressing update of documentation  
      - MR encouraged to contain: statement addressing unit/CI/regression tests added to test new features or bug fix

    b. Code reviews [Simon]  

      - 1 or more reviewer.  
      - The person submitting the merge request cannot be a reviewer.  
      - Review process should involve reading through the code, considering the logic of the feature and the unit, component and/or integration tests (in particular whether the new code adds things that need new tests added), running the new code (where feasible), and giving feedback  
      - Additional inputs can be sought from other devs (preferably using @ ).  The reviewer(s) have responsibility to ensure that the additional inputs have been addressed before approving the merge.

    c. Approval to merge [Simon]  

      - Approval of the reviewers is required for merging (we need to enforce this by turning on the relevant GitLab feature)  
      - Approval for a merge should be given once the reviewer is satisfied with all responses to review comments and new code pushed in responses, all conversations are resolved, all pipelines pass, and the feature branch is up to date with the main branch.  
      - All reviewers must approve the merge for it to be merged.  The last reviewer to approve does the merge.  If they forget/get confused about expectations or which of multiple reviewers is last, anyone can (and should!) hit merge if all reviewers have approved.  
      - Any pushes after an approval invalidates the previous approval  

    d. merging MR’s - squash or not? [Simon]  

      - MRs should be squashed if and only if there are no unmerged (to main) branches off the feature branch being merged to main  
      - It is the MR requester’s responsibility to check on the squash vs merge preference when putting up the MR, and state the intended mode.  Reviewer to check.

    e. order of merge requests [Simon]  

      - Make use of dependent merge requests

    f. Standards on style for changelogs generation

      - All repositories containing software used by anyone other than the owner must feature a `CHANGELOG.md` file containing release notes. 
      - In contrast to commit messages, release notes are meant for users of the software. They should explain what changed and what the implications of each change are.
      - Updating the changelog is a manual process, i.e. humans must be involved!
      - Whenever merging to `main`, one must include updates to the `CHANGELOG.md`. 
      - Every new release must contain appropriate updates to the changelog. These should typically include at least one of
        - Added
        - Changed
        - Fixed
        - Deprecated
        - Removed
        - Security
      - Changelogs should follow the format proposed [here](https://keepachangelog.com/en):  
    ```markdown
    # Changelog
    
    A brief description of the project.

    ## [1.0.0] - 2022-11-07

    ### Added

    - Feature 1
    - Feature 2

    ### Fixed

    - Bug 1
    - Bug 2
    ``` 


7. *Merge or rebase?*  
Developers are free to choose for themselves whether to use `merge` or `rebase` when bringing their feature branches up to date with the current `main` branch.  This section offers some insights and tips relevant to making that choice.  
    - Rebasing a branch performs a similar function to merging. It incorporates changes from some other branch into the current branch. 
    - As such, rebasing is a useful alternative to merging the `main` branch into a developer's feature branch.
    - Rebasing simply "unhooks" a branch from the git tree, and re-hooks it wherever you like. This lets you move code changes around arbitrarily.
    - In this way, rebasing allows deterministic and continuous resolution of merge conflicts between a feature branch and `main`. This means developers can keep up to date with `main` while retaining the ability to apply their commits elsewhere.
    - **Note:** rebasing re-writes a branch's commit history, in that it creates a new commit for each commit on the branch, with the new commits being made with respect to the new base (eg the current tip of `main`).
    - If a commit from `main` (or any other branch) is merged into the feature branch, then the feature branch can no longer be rebased. The shared merged commit binds the branches together.
    - Therefore *each branch should consistently follow a merge or rebase strategy*. Once `main` has been merged into a feature branch, it can't be rebased anymore.
    - Conversely, once a feature branch has been rebased onto the head of `main` (or any other branch), that feature branch can now be trivially fast-forward merged onto the new base branch. This means developers can merge into `main` with confidence that they have resolved merge conflicts already.
    - Rebased branches should use `push --force-with-lease` to update remotes (*not `--force`*) in order to prevent accidents. The flag is needed because of the aforementioned re-write of the commit history.
    - Never rebase the main branch (against anything). 

