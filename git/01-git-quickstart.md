#### Create / Initialise a new repository in a specialised directory
`git init <repo-dir>`

You will be seeing an output similar to the following:
```bash
Initialised empty Git repository in /path/to/repo/.git/
```

#### Add changes in the working directory
- Include updates to a particular file
`git add <filename>`
- Include updates to all files / folder that were created / modified
`git add .`

#### Commit the changes with message
Once the updates were tracked, the updates could be commited to Git with a short message that indicate what the changes are about
`git commit -m <msg>`

#### Upload local repo content to remote repo
`git push <remote> <branch>`


