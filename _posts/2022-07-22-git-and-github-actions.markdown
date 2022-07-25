---
layout: post
title:  "Github and Github Actions"
date:   2022-07-22 23:11:56 +0000
categories: jekyll update
---

- Git ---> version control system, software responsible fot tracking changes to your files, handles commiting, merging, branches, push pulls etc
- Github ----> online service to store git repos, handles forking, pull requests, rules/settings on branches etc

- commits are immutable snapshots of the code.
- git looks for a .git folder and tracks everything in that folder where .git is present.
- head in git --> will be the most recent commit

    - commit1 ---- commit2(^head)

- git reset --hard will reset the head to the previous commit1 and removes the latest commit2
- Each branch has their own commit history.

  - Git Rebase and Merge: [https://www.atlassian.com/git/tutorials/merging-vs-rebasing]


      ```

      mc1--> mc2 ---> mc3 ----> mc4 ----> mc5 ---> mc6 | this is the main branch with 6 commits. commit history [mc1-mc2-mc3-mc4-mc5-mc6]
                      \
                        \-----> tc4 --> tc5 --- tc6  | this is test branch which was checked out from main branch at commit 3 and has 6 commits. commit history [mc1-mc2-mc3-tc4-tc5-tc6]

      ```

      - we can either merging the test branch to main branch or rebase the test branch into master

          - If we rebase then the history will look like this [mc1-mc2-mc3-mc4-mc5-mc6-tc4-tc5-tc6]
          ```
             git checkout test
             git rebase main
          ```
          - The major benefit of rebasing is that you get a much cleaner project history
          - So basically when we rebase our test branch, we are considering the commits from the test branch as brand new commits and add them at the end of the main branch.
          - We do when the test branch is totally independent of the changes from main branch. [no respect to history]
          - Interactive Rebasing offers complete control over the branches commit history.

          - If we merge the main branch to test branch, then the history will look like this [mc1-mc2-mc3-mc4-mc5-mc6-mergecommit]
            ```
               git checkout test
               git merge main

                  or

               git merge test main
            ```
            - when we merge, we create a new merge commit in the test branch.
            - Merge commit will respect the history. 
            - mergeing with handle merge conflicts more gracefully when compared to rebase

  - Branching statergies:

    - Trunks: isolated work branch for an individual.
    - Feature branches: Each new feature on its own branch.

  - Hooking up Git and Github:

    - SSH Information:
      - public and private key
      - We create both public and priavate key. Private key will be in local .ssh folder where as we need to specify the public key to github to tell our identity.
      - Use ssh-keygen to generate an SSH key for authentication with GitHub.  You will want to name it something unique like github
        ```
        ssh-keygen -t rsa
        ```
      - Once created, it is recommended to add it to your ~/.ssh/ directory
      - Additionally, create a ~/.ssh/config file and add the following to it:
        ```
           Host github.com
           IdentityFile ~/.ssh/<path_to_private_key>
        ```
      - Once set up locally, you will then need to go to GitHub and go to Settings > SSH and GPG Keys and select New SSH Key. From here you will copy your private_key_name.pub into the key field and name it accordingly.  The final step will involve enabling SSO for this ssh-key.
      - Once you enabled the SSO you need to authorize it .
      - Now, going forward, whenever you select to clone a repo, select the SSH option.  This will inform GitHub to use your ssh-key to authenticate with GitHub

    - Personal Access Token (PAT):
      - per repository, very narrow access
      - username and personal access token.



Additional Resources:
- http://justinhileman.info/article/git-pretty/full/
- https://ohshitgit.com
- https://www.atlassian.com/git/tutorials/merging-vs-rebasing
- https://cbea.ms/git-commit/
- https://ohmygit.org
- https://learngitbranching.js.org
