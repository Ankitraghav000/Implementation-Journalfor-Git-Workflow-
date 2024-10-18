# Implementation Journal for Git Workflow Automation Script


## Objective:
The objective of this implementation journal is to provide a clear and concise guide for understanding the purpose, functionality, and structure of the Git workflow automation script.
## Prerequisites for Running the Git Workflow Automation Script on Ubuntu Terminal
Before running the Git workflow automation script, ensure you have the following prerequisites set up on your Ubuntu system:

### Ubuntu Operating System:

Make sure you are running a compatible version of Ubuntu.

- Git Installation:
Git must be installed on your system. You can install it using the following command:
```
sudo apt update
sudo apt install git
```
### Bash Shell:

The script is written for the Bash shell, which is the default shell in Ubuntu. No additional installation is needed.
### Visual Studio Code (VS Code):

For file editing, Visual Studio Code should be installed. You can download and install it from the official website.
Alternatively, you can install it using the terminal:
```
sudo snap install --classic code
```
### Vim Editor:

Vim should be installed for editing files directly in the terminal. Install it using:
```
sudo apt install vim
```
### Basic Command-Line Knowledge:

Familiarity with terminal commands and basic Git commands is helpful but not strictly necessary.
### Internet Connection:

An active internet connection is required for cloning repositories and pushing to remote repositories.
### Access to a Git Repository:

You should have access to a Git repository (either public or private) to test the script.
## Step-by-Step Implementation
### 1. Script Setup
The script is written in Bash and designed to run on Unix-based systems (Linux/macOS). It begins by asking the user to enter a Git repository link for cloning.

```
echo "Enter the Git repository link to clone:"
read -r repo_link
git clone "$repo_link"
repo_name=$(basename "$repo_link" .git)
cd "$repo_name" || exit
```
The git clone command clones the provided repository URL.
The script extracts the repository name from the URL and navigates into the cloned repository's directory using cd.
### 2. Creating and Editing Files
After cloning, the script prompts the user to create a file. If the user chooses to create one, the script asks for the filename and opens it in Visual Studio Code for editing.

```
echo "Do you want to create a new file? (y/n)"
read -r create_file
if [ "$create_file" = "y" ]; then
    echo "Enter the name of the new file (with extension):"
    read -r file_name
    touch "$file_name"
    code "$file_name"
    
    echo "Did you complete adding code in Visual Studio Code? (y/n)"
    read -r completed_code
    if [ "$completed_code" = "y" ]; then
        git add "$file_name"
        echo "Enter a commit message for this new file:"
        read -r commit_message
        git commit -m "$commit_message"
    fi
fi
```
If a file is created, the script opens it in Visual Studio Code with code "$file_name".
Once the user confirms that they’ve finished editing, the script stages the file with git add and commits the changes with a user-provided commit message.
### 3. Creating and Switching Branches
The script then prompts the user to create a new branch. It switches to the new branch and opens a file in Vim for further edits.
```
echo "Do you want to create a new branch? (y/n)"
read -r create_branch
if [ "$create_branch" = "y" ]; then
    echo "Enter the name of the new branch:"
    read -r branch_name
    git checkout -b "$branch_name"
    edit_file_in_vim
    git add .
    echo "Enter a commit message for changes in $branch_name:"
    read -r commit_message
    git commit -m "$commit_message"
fi
```
The git checkout -b "$branch_name" command creates and switches to a new branch.
The script uses Vim as the editor in this step, but you can change this to another editor if needed.
### 4. Multiple Branch Creation and Commit Flow
The script loops, allowing the user to create more branches, make changes in the selected files, and commit the changes for each branch.
```
while true; do
    echo "Do you want to create another branch? (y/n)"
    read -r create_another_branch
    if [ "$create_another_branch" = "y" ]; then
        echo "Enter the name of the new branch:"
        read -r new_branch_name
        git checkout -b "$new_branch_name"
        edit_file_in_vim
        git add .
        echo "Enter a commit message for changes in $new_branch_name:"
        read -r commit_message
        git commit -m "$commit_message"
    else
        break
    fi
done
```
This loop keeps offering the user the chance to create more branches and work on different files.
### 5. Merging Branches
After the branching process, the script asks whether the user wants to merge branches. It lists all available branches and merges the specified branch into the base branch (usually main or master).

```
echo "Do you want to merge branches? (y/n)"
read -r merge_branches
if [ "$merge_branches" = "y" ]; then
    list_branches
    echo "Enter the branch you want to merge into (typically 'main' or 'master'):"
    read -r base_branch
    git checkout "$base_branch"
    echo "Enter the branch you want to merge from:"
    read -r branch_to_merge
    git merge "$branch_to_merge" -m "Merging $branch_to_merge into $base_branch"
fi
```
The git merge command is used to integrate changes from one branch into another.
### 6. Pulling and Pushing Changes
The final steps involve pulling updates from the remote repository and pushing the changes made locally back to the remote.

```
# Pull latest changes
echo "Do you want to pull the latest changes from remote? (y/n)"
read -r pull_changes
if [ "$pull_changes" = "y" ]; then
    git pull origin "$(git branch --show-current)"
fi
```
- Push changes to remote
```
echo "Do you want to push your changes to the remote repository? (y/n)"
read -r push_to_remote
if [ "$push_to_remote" = "y" ]; then
    git push origin "$(git branch --show-current)"
fi
```
The git pull command updates the local repository with any new changes from the remote repository.
The git push command uploads the local branch and its commits to the remote repository.

## Understanding Key Components
- Git Commands:
 The script covers basic Git commands such as git clone, git add, git commit, git checkout, git branch, git merge, git pull, and git push.
- Bash Scripting:
   By using Bash scripting, the user can interact with the terminal and automate the entire Git workflow.
- Editors:
  The script opens files in both Visual Studio Code and Vim based on the task at hand, but you can replace these editors with your preferences.

## Conclusion
This journal explains the purpose and step-by-step implementation of the Git workflow automation script. By following the process outlined here, even someone with limited Git knowledge will be able to understand how the script works and create similar code for their Git workflows.


