# Development Process

At QB we don't have a mandated development process that each project follows due to the fact that we have a huge bredth of software types. From deeply embedded, through tiny python repositories, through to massive monorepos with 10 engineers working concurrently. However there are some common industry practices which we follow that trascend project size & structure so that there is some commonality when moving to different projects within QB.

# Development Flow with Git

The most basic form of development that everything at QB takes is a simple 6 step process:
 - Create a branch based on the ticket you are working on.
 - Make the changes on that branch.
 - Create a merge request back.
 - Review, modify
 - Approve
 - Merge

## Flavours
 - At QB we have a preference for the above simple structure and a single `main` branch with feature branches.
 - Development flow is a per-repo decision. More complicated repos can use something like [git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) where there is a `develop` branch in addition to `main`, or long running feature specific development branches. It's entirely up to the product/project owner. 
 - Feature branches should be linked to a Jira ticket by using the jira code in the branch name e.g. `RAU-16-fix-the-whatsimi-thing` where RAU-16 is the jira ticket number.
 
# Releases
 - Releases are always be made from `main` in a "roll forward" style - we fix defects in one release by making branches and MRs and eventually a new release from `main`. We don't "roll back" changes.
 - Creating a branch for each release is also a per-repo decision. Repos with a longer release & testing process may choose to use a release branch to apply hotfixes after testing on hardware, or make relevant entries in a CHANGELOG.md.

# Code Reviews

Code reviews should be conducted for all code that is produced at QB. We use gitlab Merge Requests (MRs) as our standard method. For a code review to pass it must meet a set of _minimum requirements_, but you may institute more strict requirements per repo if you choose.

## Minimum Requirements Prior to Merge

- Review by one team member other than the requester
- Approval of the merge by the reviewer
- All CI tests pass
- This should be enforced by the CI engine gitlab provides.

## Merging MRs
  
  - QB has a slight preference for the `squash` merge strategy as it keeps the git history smaller. If there have been branches made from branches, this can become troublesome, so avoid creating branches from branches or use the `merge` strategy on those MRs.
  - It is the MR requester's responsibility to decide on the `squash` vs `merge` strategy when putting up the MR, and state the intended mode. Reviewer to sanity check.
  - The reviewer or the owner can hit the merge button so long as the [minimum requirements](#minimum-requirements-prior-to-merge) have been met.
  - If you do make branches from branches, make use of Gitlab's [dependent merges feature](https://docs.gitlab.com/ee/user/project/merge_requests/dependencies.html) by targetting your MR back to the originating branch so that reviewers only need to parse the changes that were made in the branch. By putting on a dependent MR and targetting the orginal branch, you end up with an MR that only shows the changes made, but can't be merged until the originating branch's changes have been merged. 
  