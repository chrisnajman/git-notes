<h1 id="top">Git Notes</h1>
Review of basic processes, resolving simple merge conflicts, to be continued...

[Git Refresher &#9660;](#git-refresher)

[Resolving Merge Conflicts &#9660;](#resolving-merge-conflicts)

[Using Branches to Deal with Merge Conflicts &#9660;](#using-branches-deal-merge-conflicts)

<h2 id="git-refresher">Git Refresher</h2>

### Description
I don't always have to use Git on projects so when I do I often find that I've forgotten the basic processes. Furthermore, I'm often the sole contributor to a project, the repo owner only being responsible for merges to the master branch.

With that in mind, I thought I'd try and simulate a situation where there were a number of contributors to a single project, each responsible for a particular branch.

The folders listed below represent the different contributors, each carrying out separate tasks.

### Set up
Locally, I created four folders:
- A-starter
- B-project
- C-includes
- D-styling

In A-starter I created an index.html file, with <title>Home</title> and h1: Home, then
- `git init`
- `git add index.html`
- `git commit -m "index.html"`

---

On Github I created a new repo, git-refresher.

From A-starter I connected to the repo:
- `git remote add origin <ssh path>`

Then I pushed the index.html to the repo:
- `git push -u origin master`

---

In each of the remaining folders (B-project, C-includes, D-styling) I cloned the git-refresher repo:
- `git clone <ssh path>`
  
The results weren't exactly what I was expecting. These are the structures of the four folders:

A-starter/
  index.html
 
B-project/
  git-refresher/
    index.html
    
C-includes/
  git-refresher/
    index.html
    
D-styling/
  git-refresher/
    index.html
    
I had expected that only the files contained in git-refresher would be downloaded, not the folder itself.

**In order to maintain consistency I could have first created the repo, then index.html, then cloned git-refresher to all four folders.**

### Pushing, pulling and merging files from various branches

#### B-project/git-refresher
- I created a new branch, `git checkout -b B-project`
- I created two new files, about.html and contact.html.

These were marked up with page titles and h1s to identify them.

Add and commit:
- `git add -A` (adds all untracked files)
- `git commit -m "About and Contact pages"`

Then I pushed the branch to the repo:
- `git push origin B-project`.

On github I created a new pull request ([see steps 8 and 9 on this page](https://product.hubspot.com/blog/git-and-github-tutorial-for-beginners)), then merged B-project into master branch.

Then I deleted the branch from the repo and from local, using the command line:
- `git checkout master`
- delete remote branch: `git push origin --delete B-project`
- delete local branch : `git branch --delete B-project`

##### Updating the remaining folders with the about.html and contact.html

In each of the remaining folders I did `git pull origin master`.

---

#### C-includes/git-refresher
- I created a new branch, `git checkout -b C-includes`
- I added page navigation links and a footer message to all three files (index.html, about.html and contact.html).

Add and commit:
- `git add -A` (adds all untracked files)
- `git commit -m "Page navigation and footer"`

Then I pushed the branch to the repo:
- `git push origin C-includes`.

On github I created a new pull request ([see steps 8 and 9 on this page](https://product.hubspot.com/blog/git-and-github-tutorial-for-beginners)), then merged C-includes into master branch.

Then I deleted the branch from the repo and from local, using the command line:
- `git checkout master`
- delete remote branch: `git push origin --delete C-includes`
- delete local branch : `git branch --delete C-includes`

##### Updating the remaining folders with the edited code

In each of the remaining folders I did `git pull origin master`.

---

#### D-styling/git-refresher
- I created a new branch, `git checkout -b D-styling`
- I created two new folders
  * scss/style.scss
  * css/
- I created a .gitignore file and added 'scss/' (so that git would ignore the scss folder).

I then wrote some SCSS and compiled it (thereby creating css/style.css).

Add and commit:
- `git add -A` (adds all untracked files)
- `git commit -m "Styling"`

Then I pushed the branch to the repo:
- `git push origin D-styling`.

On github I created a new pull request ([see steps 8 and 9 on this page](https://product.hubspot.com/blog/git-and-github-tutorial-for-beginners)), then merged D-styling into master branch.


Then I deleted the branch from the repo and from local, using the command line:
- `git checkout master`
- delete remote branch: `git push origin --delete D-styling`
- delete local branch : `git branch --delete D-styling`

##### Updating the remaining folders with the edited code

In each of the remaining folders I did `git pull origin master`.

---

**All folders on the local machine are now identical.**

<h2 id="resolving-merge-conflicts">Resolving Merge Conflicts</h2>

### Description

### Set up
Locally, I created three folders:
- A-location
- B-location
- C-location

The folders represent different contributors/locations i.e. contributors on other computers.

They will each contain one file: README.md.

---

On Github I created a new repo, git-resolve-merge-conflicts.

From A-starter I connected to the repo:

* `git remote add origin <ssh path>`
Then I pushed README.md to the repo:

* `git push -u origin master`

---

In each of the remaining folders I cloned the  git-resolve-merge-conflicts repo:

* `git init`
* `git clone <ssh path>`

### Conflict 1
This is the simplest situation I could think of. It only uses the master branch i.e. no feature branching. This will occur in later tests.

* Conflict will be between a text file in A and B locations only.
* Conflict will consist of a change to the first line of the file (there is only one line).

---

In A-location I added a text file: 

` touch merge-conflict-test.txt`

The first line is: The **road** to hell is paved with good intentions.

* `git add merge-conflict-test.txt`
* `git commit -m "File for merge conflict testing from A-location"`
* `git push origin master`

Then I checked on the github repo that the push had been successful.

---

In B-location I added a text file: 

`touch merge-conflict-test.txt`

The first line is: The **path** to hell is paved with good intentions.

* `git add merge-conflict-test.txt`
* `git commit -m "File for merge conflict testing from B-location"`
* `git push origin master`

This is where I get a message saying that I've got a conflict.

* Gitbash suggests that I do a `git pull origin master`.
* A further message now tells me that I have unmerged paths in merge-conflict-test.txt.
* Unmerged paths: both added: merge-conflict-test.txt

Note: 'both' here refers to the file in B-location and to the one on the remote repository.

---

#### Resolving the conflict

In B-location, I open merge-conflict-test.txt and find the following:

```
<<<<<<< HEAD 
The path to hell is paved with good intentions. 
======= 
The road to hell is paved with good intentions. 
>>>>>>> 2d9c6b8951ce0a00fca655fdd8d7d0fce286c77b
```

The line between `<<<<<<< HEAD` and `=======` is my local file (B-location).

The line after `=======` and before `>>>>>>> 2d9c6b8951ce0a00fca655fdd8d7d0fce286c77b` is the remote file (on Github).

So, all I have to do is delete one line or the other (together with the `<<<<...`, `===...` and `>>>>...` code).

I'll go with the remote version (road) as it's the correct one.

`git status`:

Gitbash still tells me that paths are unmerged, to resolve conflicts, and to run `git commit`.

The correct steps are:

1. `git add merge-conflict-test.txt`
2. `git commit -m "Fix merge conflict in B-location"`
3. `git push origin master`.

Then I checked on the github repo that the push had been successful.

Out of idle curiosity, expecting an 'All files up to date...' message, I did a `git pull origin master` from A-location.
To my surprise (I obviously shouldn't have been surprised) an update came down, even though the files on A-location, B-location and repo were identical). **This requires an explanation**.

### Conflict 2

* Conflict will be between a text file in A, B and C locations.
* Conflict will consist of a change to the first line of the file (there is only one line).

---

In A-location I added a text file: 

` touch merge-conflict-test-2.txt`

The first line is: **Money** is the root of all evil.

* `git add merge-conflict-test-2.txt`
* `git commit -m "File for merge conflict testing 2 from A-location"`
* `git push origin master`

Then I checked on the github repo that the push had been successful.

---

In B-location I added a text file: 

`touch merge-conflict-test-2.txt`

The first line is: **Honey** is the root of all evil.

* `git add merge-conflict-test-two.txt`
* `git commit -m "File for merge conflict testing 2 from B-location"`
* `git push origin master`

and 

In C-location I first did a `git pull origin master`, then added a text file: 

`touch merge-conflict-test-2.txt`

The first line is: **Funny** is the root of all evil.

* `git add merge-conflict-test-two.txt`
* `git commit -m "File for merge conflict testing 2 from C-location"`
* `git push origin master`

I'll get a merge conflict when I try to push from B and C location.

In this test both B and C location will choose their version, i.e:

#### Merge conflict in B-location

```
<<<<<<< HEAD
Honey is the root of all evil.
=======
Money is the root of all evil.
>>>>>>> e4aa5873aee98e8502be71ca749a7a2247e0fa14
```

B-location goes with '**Honey** is the root of all evil.'

1. `git add merge-conflict-test-two .txt`
2. `git commit -m "Fix merge conflict 2 in B-location"`
3. `git push origin master`
4. `git pull origin master`
5. Fix conflict
6. Repeat steps 1,2 and 3.



#### Merge conflict in C-location

```
<<<<<<< HEAD
Funny is the root of all evil.
=======
Money is the root of all evil.
>>>>>>> e4aa5873aee98e8502be71ca749a7a2247e0fa14
```

C-location goes with '**Funny** is the root of all evil.'

1. `git add merge-conflict-test-two .txt`
2. `git commit -m "Fix merge conflict 2 in C-location"`
3. `git push origin master`
4. `git pull origin master`
5. Fix conflict
6. Repeat steps 1,2 and 3.


#### Results

After going through the processes for A, B and C-location,

* The repo version of merge-conflict-test-2.txt says: *Funny* is the root of all evil.
* The C-location version has the same line (because it originates from there).
* The B-location version says: *Honey* is the root of all evil.
* The A-location version says: *Money* is the root of all evil.

So, the last merged and pushed version (C-location) wins.

However, A and B-location are still out of synch, so a `git pull origin master' is required.

Done. Now they all have the same line, "Funny is the root of all evil".


#### Discussion

Both tests raise the following question:

* who decides which version is correct?

In a real-life situation, someone must be in charge of the Git repository.

For example, say that instead of a line of text, three similar lines of HTML were produced in each of the three locations:

##### A-location
```
<div class="red-background">
	Lorem ipsum...
</div>
```

##### B-location
```
<div class="green-background">
	Lorem ipsum...
</div>
```

##### C-location
```
<div class="blue-background">
	Lorem ipsum...
</div>
```

This could arise because each developer was working from a different .psd (if the .psd was part of the git process then this wouldn't occur). Anyway, someone has to decide that a red, green or blue background is required.

What this suggests is that
* someone should be in charge of the repo (Git master)
* each developer should be working on a unique branch
* each time they pushed their branch they should issue a pull request
* the Git master should be responsible for merging pull requests to the master branch
* the Git master should then delete the branches on the repo
* after merging on the repo has taken place, each contributor should receive instructions to delete their local branches and pull from the master branch
* each contributor should create a new local branch before work continues
* branch names should be unique.

A further refinement for a more complex project would be to create (on the repo) a branch called e.g. 'Background-colour'.
Pull requests from developers working on the Background Colour Feature could create branches named e.g. 'Background-colour-location-A etc.

The Git master could then merge pull requests into 'Background Colour' before merging with master.

These ideas will be followed up in the next section.

<h2 id="using-branches-deal-merge-conflicts">Using Branches to Deal with Merge Conflicts</h2>

### Description
In this method of resolving conflicts all work takes place on the remote repo on Github. It is up to the owner/maintainer/adminstrator of the repo to decide which code is correct, so no actual merge conflicts are triggered.

### Setup

Locally, I created three folders:
- A-location
- B-location
- C-location

The folders represent different contributors/locations i.e. contributors on other computers.

They will each contain one file: feature.txt.

---

On Github I created a new empty repo, git-branch-conflict.

From A-starter I initialised a local repo, added and commited my file and connected and pushed to the remote repo:

* `git remote add origin git@github.com:chrisnajman/git-conflict-branch.git`
* `git push -u origin master`

---

In each of the remaining folders (B-location, C-location) I cloned the git-branch-conflict repo:

* `git clone <ssh path>`

In each of the folders I created a new branch:
- A-location:
* `git checkout -b A-feature`
- B-location
* `git checkout -b B-feature`
- C-location
* `git checkout -b C-feature`

### Branch Conflict 1

In the feature.txt file I added the line:
- A -location:
* 2 + 2 = 5
- B-location
* 2 + 2 = 4
- C-location
* 2 + 2 = 3

Then I pushed each branch to the remote repo.

---

On the Github repo page you will see this:

![Recently pushed branches](https://github.com/chrisnajman/git-readme/blob/master/recently-pushed-branches.png?raw=true "Recently pushed branches")

Now it's up to the repo owner/git master to click on the 'Compare and pull request' button for each branch. On the page that is loaded he can scroll down, look at the code (or, in this case, calculation) and decide which is the best one.

In our example branch B-location's feature.txt contains the correct calculation (2 + 2 = 4) so that's the one he wants to merge into the master branch ([see steps 8 and 9 on this page](https://product.hubspot.com/blog/git-and-github-tutorial-for-beginners)).

You'll be invited to delete the branch - do it.

Then click on the 'Branches' link and delete branches A-feature and C-feature.

At this point only the master branch will be left on the remote repo (the 'Compare and pull request' lines, shown in the illustration above, might persist for a while but the branches have actually been deleted).

The developers at A-location, B-location and C-location need to be told to do the following:

* switch to the master branch and `git pull origin master`
* delete branches A, B and C-feature. 

A-location should (while still in master branch) type:
* `git branch --delete B-feature`

The command is slightly different for A-location and C-location as their pull requests were ignored.
* 'git branch -D A-feature'
* 'git branch -D C-feature'

All four locations (Github repo, A, B, C-location) should: 
* contain only one branch (master)
* contain a feature.txt file with the line '2 + 2 = 4'



---
[Back to top &#9650;](#top)

