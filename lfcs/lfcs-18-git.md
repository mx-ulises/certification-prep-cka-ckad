## Git and authentication

Git is a distributed version control system. It helps to developers to work in code projects together with each file revision being stored in the system. Old versions of files are availables with a log of who made the change in each file. It allow developers to work offline with their our copy of their repository. Git works with a repository that has different development branches. Developers and users can upload and download files from the repository with a Git client. Git has platforms such as GitHub, GitLab and private Git servers.

The workflow to work with git consist on:
 * Clone a repository
 * Make changes in the working tree
 * All changes are in the local respository and can be pushed to the remote repository

The Git repository cosist of 3 trees mantained by Git that are stored in the `.git` file in the directory. The working directory holds the actual files, the index acts as staging area and the HEAD points to the last commit that was made.

There are 3 states where files can be:
 * Modified: The file in the working tree is different from the file in the repository
 * Staged: The modified file was added to a ist of changed files in the index
 * Commited: The modified file has been commited to the HEAD and is ready to be pushed

| Command                                     | Explanation                                                                                       |
|---------------------------------------------|---------------------------------------------------------------------------------------------------|
| `git clone <repository>`                    | Clones a repository into a new directory.                                                         |
| `git pull`                                  | Fetches from and integrates with another repository or a local branch.                            |
| `git add <file>`                            | Adds file contents to the index (staging area).                                                   |
| `git status`                                | Shows the working tree status.                                                                    |
| `git commit -m "commit message"`            | Records changes to the repository with a commit message.                                          |
| `git remote add origin https://server/reponame` | Adds a new remote named `origin` with the specified repository URL.                               |
| `git push origin main`                      | Pushes the local `main` branch to the remote repository named `origin`.                           |

To work with Git you need credentials. You usually set them up in the git platform you use. You can use username and password, or strong credentials based on authentication tokens. Two-factor authentication is another option.

## Create a repository

Some global options are useful to use better Git client. These options are stored in `~/.gitconfig`.

| Command                                      | Explanation                                                                                       |
|----------------------------------------------|---------------------------------------------------------------------------------------------------|
| `git config -l`                              | Lists all the Git configuration settings.                                                         |
| `git config --global user.name "Your name"`  | Sets the global Git username for all repositories.                                                |
| `git config --global user.email "youremail@example.com"` | Sets the global Git email for all repositories.                                                   |
| `git config user.name "Your name"`           | Sets the Git username for the current repository.                                                 |
| `git config user.email "youremail@example.com"` | Sets the Git email for the current repository.                                                    |
| `git init`                                   | Initializes a new Git repository.                                                                 |
| `git add *`                                  | Adds all files in the current directory to the staging area.                                      |
| `git commit -m "First commit"`               | Records the changes to the repository with the commit message "First commit".                     |
| `git branch -M main`                         | Renames the current branch to `main`.                                                             |
| `git remote add origin <repository_url>`     | Adds a new remote named `origin` with the specified repository URL.                               |
| `git push -u origin main`                    | Pushes the `main` branch to the remote repository named `origin` and sets it as the upstream branch. |
| `git push`                                   | Pushes the current branch to the remote repository.                                               |
| `git config --global push.default simple`    | Sets the default push behavior to `simple`, pushing only the current branch.                      |
| `git config --global init.defaultBranch main`| Sets the default branch name to `main` for newly created repositories.                            |

## Use a repository

In some cases, a directory may have files you don't want to be cloned to the git repository, such as files with keys and credentials. You can exclude them by adding the list of files to be ignored in a `.gitignore` file.

| Command                                                      | Explanation                                                                                       |
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `git clone <repository_url>`                                 | Clones a repository into a new directory.                                                         |
| `git rm <filename>`                                          | Removes a file from the working directory and the index (staging area).                           |
| `git restore --staged <filename>`                            | Unstages a file, keeping the changes in the working directory.                                    |
| `git log -- <filename>`                                      | Shows the commit logs that include changes to the specified file.                                 |
| `git checkout <hash_id> -- <filename>`                       | Checks out a specific version of a file from a commit using the commit hash ID.                   |
| `git config --global core.excludesfile ~/.gitignore_global`  | Sets a global `.gitignore` file for all repositories to exclude specified files and directories.  |

## Branches

Branches are used to develop new features in isolation from the main branch. The main branch is the default branch, and other branches can be manually added. Any git repository can have multiple branches and after development is completed, we can merge them with the main branch.

| Command                                        | Explanation                                                                                       |
|------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `git diff <branch1> <branch2>`                 | Shows the differences between two branches.                                                       |
| `git merge <branch>`                           | Merges the specified branch into the current branch.                                              |
| `git checkout -b <branch>`                     | Creates and switches to a new branch.                                                             |
| `git push --set-upstream origin <branch>`      | Pushes the new branch to the remote repository and sets it as the upstream branch.                |
| `git checkout main`                            | Switches to the `main` branch.                                                                    |
| `git branch -d <branch>`                       | Deletes the specified branch.                                                                     |
| `git diff main <branch>`                       | Shows the differences between the `main` branch and the specified branch.                         |
| `git merge <branch>`                           | Merges the specified branch into the current branch.                                              |
