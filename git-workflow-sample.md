# My Common Git Workflow

[Yehuda Katz](https://yehudakatz.com/author/yehuda-katz/) | 12 May 2010 | 5 min read

A [recent post](http://reprog.wordpress.com/2010/05/12/still-hatin-on-git-now-with-added-actual-reasons/) that was highly ranked on Hacker News complained about common git workflows causing him serious pain. While I won't get into the merit of his user experience complaints, I do want to talk about his specific use-case and how I personally work with it in git.

Best I can tell, Mike Taylor (the guy in the post) either tried to figure out a standard git workflow on his own, or he followed poor instructions that tried to bootstrap someone from an svn background while intentionally leaving out **important** information. In any event, I'll step through my personal workflow for his scenario, contrasting with subversion as I go.

## Cloning the Repository

The very first step when working with a repository is to clone it. In subversion, this is accomplished via `svn checkout svn://url/to/repo/trunk`. This retrieves the most recent revision of the trunk branch of the repository.

In git, this is accomplished via `git clone git://url/to/repo` (the http protocol is also possible). This retrieves the entire repository, including other branches and tags.

## Making the Change

In both git and subversion, you make the change using a normal text editor.

## After Making the Change

In git, you make a local commit, marking the difference between the most recent pulled version (master) and the changes you made. In subversion, the normal workflow does not involve making a change, but apparently some people make manual diffs in order to have a local copy of the changes before updating from the remote. Here's an example comment from the Hacker News post:

> I'll tell you what happens when I use svn and there's been an upstream change: I never update my local tree with local modifications. Instead, I extract all my local changes into a diff, then I update my local tree, and then I merge my diff back into the updated tree and commit.
>
> When I need three-way merging, which isn't often - usually patch can resync simple things like line offsets - it's handled by a file comparison tool. I have a simple script which handles this



My personal process for making the commit in git almost always involves the gitx GUI, which lets me see the changes for each individual file, select the files (or chunks in the files) to commit, and then commit the whole thing. I sometimes break up the changes into several granular commits, if appropriate.

## Updating from the remote

Now that we have our local changes, the next step is to update from the remote. In subversion, [you would run svn up](http://svnbook.red-bean.com/en/1.1/ch03s05.html). Here, subversion will apply a merge strategy to attempt to merge the remote changes with the local ones that you made. If a merge was unsuccessful, subversion will tell you that a conflict has occurred. If you did not manually save off a diff file, there is no way to get back to the status from before you made the change.

In git, you would run git pull. By default, git applies the "recursive" strategy, which tries to merge your current files with the remote files at the most recent revision. As with subversion, this can result in a conflict. You can also pass the `--rebase` flag to pull, which is how I usually work. This tells git to stash away your commits, pull the remote changes, and then reapply your changes on top one at a time.

If you use `--rebase`, you may get a conflict for each of your local commits, which is usually easier to handle than a bunch of conflicts all at once.

I definitely recommend using `--rebase` which also provides instructions for dealing with conflicts as they arise.

In either case, in my experience, git's merging capabilities are more advanced than subversion's. This will result in many fewer cases where conflicts occur.

## Resolving Conflicts

From here on, I am assuming you followed my advice and used `git pull --rebase`.

If a conflict has occurred, you will find that if you run git status, all of the non-conflicting files are already listed as "staged", while the conflicting files are listed outside the staging area. This means that the non-conflicting files are already considered "added" to the current commit.

To resolve the conflicts, fix up the files listed outside the staging area and `git add` them. Again, I normally use gitx to move the resolved files into the staging area.

Once you have resolved the conflict, run `git rebase --continue`. This tells git to use the fixed up changes you just made instead of the original commit it was trying to put on top of the changes you got from the remote.

In subversion, if you got a conflict, subversion will create three files for you: `file.mine`, `file.rOLD`, and `file.rNEW`. You are responsible for fixing up the conflicts and getting back a working `file`. Once you are done, you run `svn resolved`.

> **NOTE:** If you had not used `git pull --rebase`, but instead did raw `git pull`, you would fix up the files, add the files using `git add` or gitx, and the run `git commit` to seal the deal

## Yikes! Something went wrong!

In git, if something goes wrong, you just run `git reset --hard`, which will bring you back to your last local commit.

In subversion, it's not always possible unless you manually stored off a diff before you started.

## Pushing

Now that you're in sync with the remote server, you push your changes. In git, you run `git push`. In subversion, you run `svn commit`.

## One Glossed-Over Difference

Subversion allows you to commit changes even if you haven't `svn up`ed and there have been changes to the remote, as long as there are no conflicts between your local files and the remote files.

Git never allows you to push changes to the remote if there have been remote changes. I personally prefer the git behavior, but I could see why someone might prefer the subversion behavior. However, I glossed over this difference because every subversion reference I've found advises running `svn up` before a commit, and I personally always did that in my years using subversion.

## Comparison

Here's a workflow comparison between git and subversion:

| Operation                            | git                                          | svn                                                          |
| :----------------------------------- | :------------------------------------------- | :----------------------------------------------------------- |
| Clone a repository                   | `git clone git://github.com/rails/rails.git` | `svn checkout http://dev.rubyonrails.org/svn/rails/trunk`    |
| Preparing changes                    | `git commit` (using gitx)                    | nothing or create a manual diff                              |
| Update from the remote               | `git pull --rebase`                          | `svn up`                                                     |
| Resolving conflicts                  | `git add` then `git rebase --continue`       | `svn resolve`                                                |
| Resolving conflicts without --rebase | `git add` then `git commit`                  | N/A                                                          |
| Yikes! Rolling back                  | git reset --hard                             | `svn up -rOLD` then apply diff (only if you manually made a diff first) |
| Pushing                              | git push                                     | svn commit                                                   |

Note that I am not attempting to provide an exhaustive guide to git here; there are many more git features that are quite useful. Additionally, I personally do a lot of local branching, and prefer to be able to think about git in terms of cheap branches, but the original poster explicitly said that he'd rather not. As a result, I didn't address that here.

I also don't believe that thinking of git in terms of subversion is a good idea. That said, the point of this post (and the point of the original poster) is that there are a set of high-level version control operations that you'd expect git to be able to handle in simple cases without a lot of fuss.