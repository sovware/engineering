# Development Process
Steps
-----

The general process for development is:

1.  **Determine MVP for the issue**
    
    Before you start, make sure the issue is broken down to the minimum viable product. If it can be broken up into multiple issues, it should be. This allows better isolation of work, and better tracking for the project managers.
    
    Issues should succinctly describe how to fix them. Vague descriptions make it hard to know when the issue is complete, and can make it harder for other people to get involved in the project.
    
2.  **Take the issue**
    
    When working on an issue, make sure you’re not overlapping with existing work. This is typically handled by assigning the issue to yourself, but could also be via a kanban board (ZenHub or GitHub Projects) depending on the project.
    
3.  **Write the code!**
    
    You’ve done the easy bits, now it’s time to actually do the work.
    
    This work needs to happen on a branch. You can call this whatever you want; common patterns are `{issue_number}-{name_of_feature}` (e.g. `42-add-chicken-nuggets`) or simply `{name-of-feature}` (`add-chicken-nuggets`).
    
4.  **Push early, push often**
    
    As soon as your code is in a usable state, you should push it up. This allows tracking the issue over time a little easier, and avoids cases where you disappear for a while to sort out an issue. It also allows others to watch and potentially correct a wrong assumption before you spend hours on it, and will trigger automated testing if set up for the repo.
    
    You don’t need to push every commit, but be sensible.
    
5.  **File a PR early**
    
    Once you push for the first time, you should also file a PR to make sure it’s tracked. Branches which don’t have a pull request associated may be lost amongst the others.
    
    Pull requests should specifically mention the issues they close at the least. Ideally, have a sentence or two that describes how you solved the issue as well to provide context on the pull request.
    
6.  **Iterate, complete, and submit for review**
    
    Keep working, and when ready, submit the PR for review. Typically, this is handled via a “Review” or “Review & Merge” label, but could also be via a kanban column.
    
    You’ll also need to find someone to review it. This differs depending on the project and the PR, see [the reviews guide](https://lounge.sovware.com/how-we-work/process/reviews/) for more info.
    
7.  **Merge and ship!**
    
    Merging the branch is usually handled by the reviewer of a pull request. The branch should also be deleted after merging via GitHub’s UI.
    
    When you’re ready to deploy the change, it can either be deployed via Vantage for sites on the HM Stack, or through the WP.com VIP submission process.
    

Deployable Master
-----------------

The `master` branch of the repository should be deployable at any time. Deployment for internal projects is handled via Vantage, where the site is essentially updated to the current commit (head) of `master`, and rollbacks are handled by reverting `master` then redeploying.

This means that features should only be merged into `master` when they’re ready to be deployed. Anything in `master` **must** go through code review before merging, and this can be enforced via the GitHub repository settings.

For more complex projects with differing QA processes, additional workflow branches such as `staging` can be created. Features can then be merged into these branches before then merging those into `master`. This workflow only makes sense for more complex projects, and should be avoided if not necessary; keep it as simple as possible.

Features in Isolation
---------------------

With the asynchronous nature of how we work, it’s important that everyone is able to continue making movement without being blocked on anyone else. To ensure smooth flowing development, we create branches for new features or bug fixes. When these changes are ready, they then proceed into review and are merged.

**Each branch should focus on a single issue with the goal of closing it out.** Following the agile methodology, these issues should also be broken down as much as possible, so each branch should be a single focussed change to close out the issue.

Generally, these features should be entirely independent of each other. This allows engineers to grab an issue and work away on it without blocking. However, not all features are truly independent; a branch may provide underlying functionality required by other features. Merging these branches early helps, but if a feature isn’t ready for deployment, there are a few options:

*   **Feature flags:** Feature flags allow merging into `master`, but with the code disabled unless a setting is activated. This can be handled via a constant, a site setting, or a user setting. Constants should be used for development, while settings or user settings should be used when the feature should be deployed to the production site and QA’d there.
*   **Sub-branch:** A main feature branch can be created, with additional PRs as sub-branches. These can then be PR’d into the main feature branch. When using this workflow, all work needs to be done on sub-branches and merged via PRs, otherwise the development process can become super complex.
*   **Workflow branches:** Workflow branches can be used for features by merging the feature into `staging` (or etc). This is not recommended, as it has the possibility of blocking a `staging` merge into `master` if other changes need deploying.

Incremental Improvement
-----------------------

Features should be shipped early in a minimal state, then improved further in future changes. The agile methodology is heavily focussed on iterative improvement, encouraging constant improvement.

This means merging features when they’re ready in a minimal state, and continuing to build on top of this. This makes development much more parallelisable (it’s a word, trust me), as multiple engineers can work on different parts of the same feature once the base of it is in place. This is crucial for asynchronous work, with Human Made being globally-based.
