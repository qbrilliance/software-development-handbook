## 3. Version control

At QB, we don't have a single version control process that every project follows, due to the breadth of software types represented at the company: from deeply embedded to tiny Python repositories, through to massive monorepos with 10 engineers working concurrently. There are however some common industry best practices that transcend project size and structure.  We follow these in order to ensure a basic level of quality and efficiency in our software development, and to make it easy for people to move between different projects within the company.

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
      - Petalinux-based embedded projects are an exception; these should follow Petalinux and OpenEmbedded conventions, using layers.

5. *Naming of git branches*  

    - `X-Y-Z`, with `X` a Jira or Gitlab issue code addressed by the branch, `Y` an informative name for the branch, and `Z` an optional date in the format YYYYMMDD.
    - The inclusion of a username as a leading namespace (”user-name/branch-name") is also encouraged, especially when either a) there is no Jira/Gitlab issue code, or b) multiple people have branches addressing the same Jira/Gitlab ticket.

6. *Development flow with git*

    The basic development flow at QB follows a simple 6-step process:

      - Create a feature branch based on the ticket that you are working on.
      - Make the changes on that branch.
      - Create a merge request back to the `main` branch.
      - Review, modify
      - Approve
      - Merge

    The preference at QB is for the simple branch structure implied by this process, i.e. a a single `main` branch with feature branches. However, branch structure and development flow are ultimately per-repo decisions to be made by the product/project owner.  More complicated repos can use something like [git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) where there is a `develop` branch in addition to `main`, or long-running feature-specific development branches.

7. *Merge requests and code review*

    Reviews should be conducted for all code produced at QB. We use Gitlab Merge Requests (MRs) as our standard method. For a code review to pass and an MR to become approved, [a set of minimum requirements](#approval_to_merge) must be met. Repository owners may however institute more strict requirements on a per-repo basis if desired.

    a. Preparation of merge requests

      - The feature must be fully ready for merge before requesting review (i.e. not in "Draft" format, not "untested", etc).
      - New functions and classes should not be considered ready for merge without corresponding unit tests (see [unit tests](devops.md#unit_tests)).
      - Each merge request created should follow the [merge request template](merge_request_template.md).

    b. Code reviews

      - Each MR requires at least one formally identified reviewer. At the requester or repo owner's discretion, additional reviewers can be added at any point in the review process.
      - The person submitting the merge request cannot be a reviewer.
      - A review should include:
        - Reading through the code.
        - Critically considering the logic of the feature or bug fix applied by the MR.
        - Critically evaluating the unit, component and/or integration tests (in particular whether the new code adds things that need new tests added).
        - Giving feedback on all these aspects, including specific suggestions for revision.
      - Additional inputs can be sought during a review from other developers, preferably by tagging them using `@`.  The reviewer(s) have responsibility to ensure that the additional inputs have been addressed before approving the merge.

      - Things you cannot rely on the reviewer to do:

        - Run your code. It is not a reviewer's responsibility to test that your code fulfils its purpose. They may still do this, but it's your responsibility to make sure it works before putting it up for review. If you explicitly want the reviewer to test that it works for them and/or on their system, say so in the text of the merge request.
        - Find all the bugs. Code reviews are the [best way to find defects](https://kevin.burke.dev/kevin/the-best-ways-to-find-bugs-in-your-code/), but are not a substitute for a good suite of tests. 
        - Pick up all your spelling mistakes. Use a spell checker. Spelling errors take focus away from the actual code.
        - Pick up all your formatting non-compliances. Use auto-formatting & a linter. Formatting errors take focus away from the actual code. 

    c. Approval to merge <a name="approval_to_merge"></a>

      - Reviewer approval is required for merging. The option to enforce this requirement should be turned on in Gitlab by default for all repositories.             
      - Reviewers should individually approve MRs once:
        - they are satisfied with all responses to comments (both from the reviewer and others),
        - they are satisfied with all new code pushed in response to those comments, 
        - all conversations are resolved, 
        - all CI test pipelines pass, and 
        - the feature branch is up to date with the main branch.  
      - All formally identified reviewers must approve the merge for it to be merged.
      - Any pushes after an approval invalidate the previous approval.  

    e. Merging

      - QB has a slight preference for the `squash` merge strategy, as it keeps the git history smaller. This can become troublesome if any additional branches have been made off the merge candidate. Try to avoid creating branches from branches other than `main`.  Where such branches are up for merge, use the `merge` strategy when merging the candidate rather than  `squash'.
      - It is the requester's responsibility to decide on the `squash` vs `merge` strategy when putting up the MR, and to state the intended mode. The reviewer should do a sanity check on this choice.
      - The last reviewer to approve should perform the merge immediately after giving approval.  If they forget/get confused about expectations or struggle to tell which of multiple reviewers was last, anyone including the requester can (and should!) hit merge if all reviewers have approved.
      - If you do make branches from branches, make use of Gitlab's [dependent merges feature](https://docs.gitlab.com/ee/user/project/merge_requests/dependencies.html) by targeting your MR back to the originating branch, so that reviewers only need to parse the changes that were made in the branch. By putting up a dependent MR and targeting the orginal branch, you end up with an MR that only shows the changes made, but can't be merged until the originating branch's changes have been merged.  Do not `squash` such MRs.

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


8. *Merge or rebase?*  
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

9. *Releases*
    - By default, releases are to be made from the `main` branch, and given a git tag that differs for each release.    
    - Repos with a longer release and testing process may however choose to use a release branch to apply hotfixes after testing on hardware, or to simply make relevant entries in a CHANGELOG.md rather than applying a new tag.
    - Releases are to be made in a "roll forward" style.  That is, defects in one release should be fixed by making new branches and MRs, and eventually a new release. Do not "roll back" changes.

