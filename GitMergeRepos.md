# Merging a Repo with a Different Existing Repo
_NOTE: These instructions are intended to show how to merge one of our repos with a repo from another team while preserving all the history of both repos without creating a separate subdirectory holding our repo._

## Prep the Target Repo
  * Go to your Git repo directory `cd <all-my-repos>` (Ex: `cd C:\MyRepos`)
  * Clone the target repo `git clone <url-to-target-remote-repo>`
  * Go to the new repo `cd <target-repo>`
  * Create a branch and check it out for this migration `git checkout -B <new-branch>`
  * Go to the subdirectory we want our code to get merged into `cd <path-to-target-subdirectory>`

## Branch Out Subdirectory from Source Repo
  * Separately, go to our existing source branch `cd <source-repo>`
  * Create and checkout a new branch for this migration `git checkout -B <new-branch>`
  * Get latest `git pull origin master`
  * Split out the subdirectory into its own branch, which will be our new sub-repo `git subtree split -P <name> -b <name>`
    * This creates a new branch called <name> that only contains the CONTENTS of the subdirectory without holding the subdirectory or parent directory from the repo
  * Check to see all the branches available; one should be called <name> `git branch -a`
  * Checkout this new branch `git checkout <name>`
    * You should now see your repo only containing the contents of the <name> folder with none of the parent directory
  * Push your changes to our remote `git push`
    * If you get a note that there is no upstream, try `git push --set-upstream origin <name>` instead
  * Double-check on the remote that the full history is there for all of the files

## Back to the Target Repo
  * Back in the target repo, point to our repo in order to merge the two `git remote add -f <source-repo> <url-to-source-remote-repo>`
  * MERGE `git merge <source-repo>/<name> --allow-unrelated-histories`
  * You should see all of the source files in the parent folder of the target repo. Let's fix that:
    * Open PowerShell
    * Navigate to the target repo on your system `cd <all-my-repos>/<target-repo>`
    * Run this script, including the name of all subdirectories listed in the parent folder of the target repo (all directories and files you see when you `ls` in the current directory): `dir -exclude <name-of-subdirectory-1>,<name-of-subdirectory-2>,<name-of-file-1>,<etc> | %{git mv $_.Name <path-to-target-subdirectory>}`
  * Push your changes to the target repo and create a PR if everything looks good!! `git push`
    * It's possible you may need to reset the upstream location, depending on which repo you see your branch in. You should just be able to add the remote from the command above for the correct target repo.
