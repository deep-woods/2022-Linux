# GIT

<br>

- Install
- GIT repositories
- Remote Repositories
- Clone, Pull & Push
- GIT vs GITHUB

<br>

## source Control Management (SCM)

- Version control system (VCS)

0. Install `GIT`
1. Initialise a `GIT` repository
2. Configure files to track
3. `GIT` tracks changes
4. Stage changes
5. Commit changes

<br>
<br>

| `my-application`<br>├── `LICENCE`<br>├── `README.md`<br>├── `requirements.txt`<br>├── `main.py`<br>├── `utils.py`<br>├── `db.py`<br>├── `backend.py`<br>├── `cache.py`<br>└── `notes.txt`   | <br><br><br><br>`main.py`<br><br><br><br><br>   | <br><br><br><br><br>`backend.py`<br>`cache.py`<br>  |    | <br><br><br><br>`utils.py`<br><br><br><br>   |
| -- | -- | -- | -- | -- |
| GIT repository   |   Version 1 | Version 2   | Staging   | Version 3   |        

<br>
<br>

### 0. Install `GIT`

<br>

    sudo apt install git

    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    git is already the newest version (1:2.25.1-1ubuntu3.2).
    0 to upgrade, 0 to newly install, 0 to remove and 76 not to upgrade.

<br>

### 1. Initialise a `GIT` repository

<br>
        git version
        git version 2.25.1

        git init
        Initialised empty Git repository in /home/forest/.git/  ( << hidden directory )

GIT stores all information related to the changes on your files.


<br>

### 2. Configure files to track

    git status

    On branch master

    No commits yet

    Untracked files:
    (use "git add <file>..." to include in what will be committed)
        .bash_history
        .cache/
        .config/
        .gitconfig
        ...

<br>

- `git add <file 1> <file 2> ... <file n>`:  add files to be tracked.

        git add 05\ GIT.md
        git status

<br>

        On branch master

        No commits yet

        Changes to be committed:
        (use "git rm --cached <file>..." to unstage)
            new file:   Documents/GitHub/2022-Linux/2022 DevOps Prerequisites/05 GIT.md

<br>

### 3. `GIT` tracks changes

<br>

After some modifications in the files...

        git status
        On branch master

        No commits yet

        Changes to be committed:
        (use "git rm --cached <file>..." to unstage)
            new file:   Documents/GitHub/2022-Linux/2022 DevOps Prerequisites/05 GIT.md

        Changes not staged for commit:
        (use "git add <file>..." to update what will be committed)
        (use "git restore <file>..." to discard changes in working directory)
            modified:   Documents/GitHub/2022-Linux/2022 DevOps Prerequisites/05 GIT.md


<br>

### 4. Stage changes.

- run `git add <file>` & `git status` again. 

        git add 05\ GIT.md
        git status

<br>

Remember, every time you modify any of these files, they are unstaged and they go back to a modified state; running the git add command on a track, but an unstaged file stages the file and adds it to the list of files to be  committed.

<br>

### 5. Commit changes.

<br>

- `git commit -m "Initial Commit"
  - `-m`: commit message. Without this flag, `Git` will open an editor for the commit message. 

        git commit -m "initial commit"

        [master 187dfe1] initial commit
        1 file changed, 1 insertion(+)



<br>
<br>
<br>

## GIT Remote Repositories

Host a git server to facilitate `pull`s & `push`es.

<br>

GIT comes with a built-in command that runs get as a simple TCP server

- GIT: VCS
- GitHub: a server

<br>

### Commands

- `git init`
- `git add .`
- `git commit -m 'commit message`

<br>

- `git remote add <name> <url>`
- `git remote add github`

- `git push <name> <branch>`
- `git push -u github master`

<br>

- `git clone <url>`:
- `git remote -v`: Origin (original repo) is the default name given to a remote repository when cloned. 

<br>
<br>

## Practice

<br>

    git clone /opt/remoterepo.git /home/forest/remoterepo
    
    Cloning into '/home/thor/remoterepo'...
    done.

<br>

    git add index.html
    
<br>

    sudo git commit -m 'Index aded.'

    [master 6eed256] your commit message
    1 file changed, 1 insertion(+)
    create mode 100644 index.html

<br>

    git push origin master

    Counting objects: 4, done.
    Delta compression using up to 36 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (3/3), 296 bytes | 0 bytes/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To /opt/remoterepo.git
       a548add..6eed256  master -> master