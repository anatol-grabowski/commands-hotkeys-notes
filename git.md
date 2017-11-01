git config -l, --list // list config options from system, global and local config files
git config -l --show-origin
git config --global user.name "username" // set user.name in $HOME/.gitconfig
git config --system user.email email@mail.com // set user.email in git/mingw/etc/gitconfig
git config --system --unset user.email // unset user.email in git/mingw/etc/gitconfig
git config --system --remove-section user
git config --global alias.lo "log --oneline --graph"
git config --system http.sslverify false

git init [<proj_dir>] // init repo in proj_dir or cwd (create $GIT_DIR=.git subdir)
git init --template <templ_dir> // templ_dir must be absolute path
git init --separate-git-dir <gitdir> // gitdir can be relative to cwd
git init --shared[=<permissions>]
git init --bare [<proj_dir.git>] // repo only for push/pull (no gitdir)
git init -q, --quiet

git clone <src_repo> [<dst_proj_dir>]
git clone --depth <depth> // 1 min depth
git clone --progress

git status // show changes between working-tree (proj_dir), index (staging area) and HEAD (current checked out commit)
git show <commit || tag> // show info and diff for commit
git show --word-diff // difference by word (default by line)
git reflog // log of actions
git show-ref // list .git/refs and packed_refs
git log <commit | branch> // list commits
git log -g, --walk-reflogs [<log file from .git/logs>] // log for commits from git reflog
git log -p // git show for each commit listed
git log --oneline // shorthand for git log --pretty=oneline --abbrev-commit
git log --oneline -- <filepath> // list all commits that have changes on file
git log --graph

git diff // diff -u between workiking-tree and index
git diff HEAD // diff between workiking-tree and HEAD
git diff --cached <named_commit> -- <file> // diff between index and named_commit or HEAD
git diff --no-index -- <file1> <file2> // same as diff -u <file1> <file2>

git add <glob || file || dir> // add files to index (staging area)
git add -A, --all // stage all files (tracked or untracked) in working tree
git add -u, --update // stage all changed tracked files in working tree
git add -f, --force // forcefully add ignored files
git add -p, --patch // select hunks interactively?

git rm <file> // remove file from working tree and index (must be in index)
git rm --cached <file> // remove file from index (unstage)
git rm --ignore-unmatch <file> // don't error if file doesn't exist

git reset // same as git reset --mixed HEAD (reset index to HEAD)
git reset --soft <named_commit> // reset HEAD to named_commit (eg HEAD~1 for prev commit)
git reset --mixed <named_commit> // default mode, reset HEAD and index to named_commit
git reset --hard <named_commit> // reset HEAD, index and working tree to named_commit
git reset -- <file> // reset only file (doesn't work with --hard, checkout -- file instead)

git commit // copy 
git commit -a, --all // auto add all tracked (changed/deleted) files, do nothing for untracked
git commit --amend // redo last commit (amended commit is lost)

git branch // list branches
git branch <new_branch_name> <with_tip_at_this_commit> // create branch
git branch -d, --delete // delete branch (must not be checked-out), keeps subbranches
git branch -D // alias for --delete --force, delete even if there are unmerged changes
git branch -f <branch_name> <new_tip> // move branch

git merge <branch> // merge changes from branch to current branch
git merge <br> --no-ff -m <message> // non fast-forward merge, create commit for merging
git merge --no-ff --no-commit <to_branch> // test for conflicts, don't do real merge

git rebase <branch> // apply changes from current branch to branch
git rebase --onto master server client // “Take the client branch, figure out the patches since it diverged from the server branch, and replay these patches in the client branch as if it was based directly off the master branch instead.”

git cherry-pick <commit> // apply changes from commit to current branch

git checkout <branch_name || commit_name || tag_name> // set state as if right after commit
git checkout <commit> -- <file> // revert single file
git checkout --detach <branch> // detach head from branch tip
git checkout --force // throw away changed files
git checkout -b <new_branch>

git tag // list tags
git tag -l "v2.*" // list only v2 tags
git tag <tagname> <commit_name> // add lightweight tag (just commit name alias) to commit
git tag -a <tagname> -m <message> // add annotated tag (message necessary) to HEAD

git stash
git stash list
git stash apply [<stashname>]
git stash drop
git stash pop // apply + drop



git remote -v // list named remote repos with urls
git remote add <name> <url> // add remote to this repo
git remote show <name>

git fetch origin // fetch branches specified in .git/config
git fetch origin master:refs/remotes/origin/mymaster // fetch master from origin to mymaster
git fetch origin +master:refs/remotes/origin/mymaster // fetch master from origin to mymaster even if it isn't a fast-forward

git push <remote || url> <branch_name | tagname> // push branch to remote
git push -f, --force // overwrite remote
git push <remote> --delete <branch> // delete branch
git push origin :topic // delete branch named 'topic' (<src>:<dst>)
git push -u origin master // for the first push, if master doesn't exist at origin (adds [ branch "master" ] ... to .git/config)
git push <remote> --tags // push all tags

git pull // fetch and try to merge


git rev-list --objects --all // list all reachable objs (doesn't list objs referenced in index)
git fsck --unreachable // list unreachable objs (doesn't list objs referenced in index, commits form reflogs)
git fsck --full
git fsck --no-reflogs // consider commits from reflog to be unreachable
git update-ref -d refs/original/HEAD // delete ref
git reflog expire --expire=now --all // empties files at .git/logs, !note expire --expire
git reflog expire --expire=1.minute refs/heads/master // expire entries older than 1min at master branch reflog
git prune --expire now // remove unreachable objects
git gc // pack objects
git gc --prune=now // remove dangling objects and pack
git ls-tree <tree-ish> // list files in commit
git ls-files --cached | --deleted | ...// info about files in index and working tree
git hash-object -w <file> // write obj from file
git hash-object -w --stdin // write obj from stdin
git cat-file -p <obj_hash> // print content
git cat-file -t <obj_hash> // show type
git cat-file -p master^{tree}

git count-objects -v
git verify-pack -v .git/objects/pack/pack-xxxx.idx // show objs in pack (hash type content_size obj_size? offset_in_pack?)
cat xxx.pack | git unpack-objects // .pack has to be outside .git/objects/pack

git filter-branch --index-filter "git rm --ignore-unmatch --cached file.txt" -- <prefrom_commit>..<to_commit> // remove file.txt from history in commits starting from after 'prefrom' to 'to', moves head to new to_commit, doesn't move branch pointer. References to old commits are still in reflog and refs/original, so they have to be removed before git gc --prune=now

git archive --format tar <commit> // make a tar ball of the working tree for a given commit
git archive --format tar <commit> <directory/>

git format-patch -o <dir> <since_commit> // create .patch (separate .patch for each commit)
git format-patch --stdout <since_commit> >> <patchname.patch> // .patch in single file
git apply --stat fix_empty_poster.patch // show changes in patch
git apply --check fix_empty_poster.patch // check if there are any conflicts
git apply <patchname.patch> // apply patch to working tree (don't have to be inside repo)
git apply --index <patchname.patch> // also apply to index
git apply --cached <patchname.patch> // apply only to index
git am --ignore-whitespace < <patchname.patch> // apply and commit patches from stdin
git am --signoff < <patchname.patch> // add 'Signed off by ...' line to the commit message
