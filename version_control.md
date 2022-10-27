## 3. Version control

1. *Location of repositories*  
QB repositories are hosted on Gitlab, within [the qbau organisation](https://gitlab.com/qbau).

2. *Structure of qbau repository tree on Gitlab*  
    - Each team creates and maintains a folder within the qbau organisation, named after the team and containing the software repositories maintained by that team.  This is referred to in Gitlab as a 'group'.  These folders may contain subfolders at the respective teams' discretion (e.g. for specific projects).  
    - An additional [company-wide](https://gitlab.com/qbau/company-wide) folder also exists, and contains repositories maintained by the company as a whole (such as the repository in which this handbook resides).  
    - No repositories should exist outside of these folders, i.e. in the root of the qbau organisation.

3. *Gitlab organisation membership and group/repository access levels*  
    - The IT Group is solely responsible for adding new users to the qbau Gitlab organisation.  
    - IT will setting all users' access level at the organisation level to 'Developer'.  This gives all QB employees the ability to read, raise issues and lodge merge requests on all repos of all teams in QB.  
    - IT should give Team Leads 'Owner' level permissions for the folders that they administer.  Team Leads may upgrade any users' access levels within their Gitlab group (folder) or for a specific repository in their folder.

4. repo file tree and source file naming [Pat]
    - preference for out-of-source builds when using cmake (build dir)
    - preference for using include paths as proxy include namespace
    - preference for source files to live in `src` and headers in `include`
    - preference for flat file structures

5. git branch naming [Pat]
    - temp: start with Jira issue code, then username (optional), then an informative name, then date (optional)
    - Name is encouraged when there is no Jira code, or multiple people’s branches for the same Jira ticket (”user-name/branch-name”)

6. development flow with git [Simon]
    - Preference for feature branches + main, with rolling releases done from main
    - Per repo decisions to have longer-term feature-specific development branches
    - No release branch for continuously deployed repos; repos with a longer release & testing process to add a release branch, and apply hotfixes there

7. merges and code review

    a. what is the minimum gate for being able to merge? 1 approval & all CI passes? [Simon]
        1. Review by one team member other than the requester
        2. Approval of the merge by the reviewer
        3. All CI tests pass

    b. merging MR’s - squash or merge? [Simon]
        - MRs should be squashed if and only if there are no unmerged (to main) branches off the feature branch being merged to main
        - It is the MR requester’s responsibility to check on the squash vs merge preference when putting up the MR, and state the intended mode.  Reviewer to check.

    c. standards on style for changelogs generation? [Stefan]
        - All repos that contain releasable software should feature changelogs
        - All merges to main/release must include appropriate updates to changelogs, done by hand rather than automated

    d. who does the merge?  reviewer vs owner vs requester [Simon]
        - Reviewer.  If they forget/get confused about expectations or which of multiple reviewers is last, anyone can (and should!) hit merge if all reviewers have approved.

    e. order of merge requests [Simon]
        - Make use of dependent merge requests

    f. Other decisions: [Stefan]
        1. Turn on GitLab requires approval for merge
        2. Minimum one review, who has responsibility to approve
        3. Any pushes after an approval invalidates the previous approval
        4. Feature must be fully ready for merge before requesting review (i.e. not in ‘Draft’ format)
        5. Review process should involve reading through the code, considering the logic of the feature + unit and integration tests, running the new code (where feasible), and giving feedback
        6. Approval for a merge should be given once the reviewer is satisfied with all responses to review comments and new code pushed in responses, and all conversations are resolved
        7. 1 or more reviewer, all must approve the merge for it to be merged.  The last reviewer to approve does the merge.
        8. Additional inputs can be sought from other devs (preferably using @ ).  The reviewer(s) have responsibility to ensure that the additional inputs have been addressed before approving the merge.
        9. Main must be re-merged if it is updated in the review process
        10. Merge request must contain: a short summary of the solution, any new tests added, statement addressing update of documentation
        11. MR encouraged to contain: statement addressing unit/CI/regression tests added to test new features or bug fix
        12. There needs to be a regular re-assessment of the test battery, with culling to sensible size
        13. A merge template should exist, explaining what is expected in a merge request

7. git merge vs rebase [John]
    - Each branch should consistently follow a merge or rebase strategy, without mixing
    - Rebase is preferred, but requires all authors of the branch to agree to use rebase
    - Rebased branches should be push —force-with-lease in order to prevent accidents
    - Never rebase the main branch (against anything)

