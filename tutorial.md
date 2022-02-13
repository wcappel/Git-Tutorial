# Git Tutorial - by Wilton Cappel
Git is one instance of version control software (VCS). Version control software allows you to monitor and record changes across your code as well as coordinate remotely with other people who may be working on the same project as you. Git is usually already installed on your computer, and can be accessed in whatever command-line interface your operating system provides (e.g. Terminal for Mac).

To illustrate how to use Git, we will be creating a simple Java program that greets a user, along with some other functionalities. 

## Initialization and adding
First, let's create some initial code for our program. In a file named 'Tutorial.java', we have some code:
```
public class Tutorial {
	public static void greeting() {
		System.out.println("Hello!");
	}
	
	public static void main(String[] args) {
		greeting();
	}
}
```
After writing this file, in order to have Git track its changes we must initialize a repository within the directory it is located in. Using our command-line interface, we will first navigate to the directory.

`$ cd <directory path>`

Then we will initialize a Git repository with:

`$ git init`

All of our Git commands will be prefixed by 'git' and then a space. After entering this command, Git will tell you what the name of the initial branch is. We will discuss what branches are later, but keep this name in mind. Once we have initialized a repository, we must add our files to the staging area. We can do this with:

`$ git add -A`

to add all of our files, or:

`$ git add <file name>`

to add individual files at a time. 

## Commits

Now in order to record these changes, we must create a commit. Commits are the singular elements within the timeline that Git links together and records. It is good practice to have messages accompanying commits that display information about what has been changed. In order to make a commit with a message, use:

`$ git commit -m "<a message>"`

Now Git has recorded our files and their initial state in the timeline of changes it is creating. Let's say we want to make some changes to our program. For instance, in the greeting function, I want to add a parameter which takes in a name and prints a greeting with the name provided.
```
public class Tutorial {
	public static void greeting(String name) {
		System.out.println("Hello " + name + "!");
	}
	
	public static void main(String[] args) {
		greeting("Wilton");
	}
}
```
In order to record these new changes, we need to add our files to the staging area again and create another commit within the same directory.
```
$ git add -A
$ git commit -m "Greeting funct. modified to take a name."
```
Now, the latest changes to our program have been recorded. 

## Remote syncing
One thing we may want to do is link our local repository on our computer with a remote repository that is independent of our device. One platform that allows us to do this is GitHub. After you create a GitHub account (and give your device permission to access your repositories with your PAT), create a new repository and copy the link that is provided under "Quick setup". We can then link our local repository to this remote one with:

`$ git remote add origin <paste link here>`

In order to send our changes to the remote repository, we must 'push' our commits. In order to do this for the first time, we will use:

`$ git push -u origin <branch name>`

with the name of your initial branch. The remote repository will now have a branch that will correspond to and track the changes in your initial branch. However, if we want to push any later commits to the same branch, we can just use:

`$ git push`

instead of specifying more arguments.  If we want to download a remote repository, hosted on a site like GitHub, we can use the 'clone' command:

`$ git clone <paste repository url here>`

which will then download a repository into a folder within our working directory.

## Branching
Thinking of a Git repository in terms of a timeline we are creating of the changes to our code, a branch is a split in this timeline. A branch is essentially a different version of the same repository that you can make changes to without affecting the main branch or other branches of the repository. In order to create a branch we can use:

`$ git branch <name of new branch>`

For our project, let's make a branch called 'dev', in which we can code some experimental features for our program.

`$ git branch dev`

In order to switch from the branch we are currently on to the new 'dev' branch, we have to use the 'checkout' command:

`$ git checkout <branch name>`

So in our case, we would type:

`$ git checkout dev`

Now let's make some changes to our program while on this 'dev' branch. Let's add a new function that counts the number of vowels within a string.
```
public class Tutorial {
	public static void greeting(String name) {
		System.out.println("Hello " + name + "!");
	}
	
	public static int vowelCount(String str) {
		str = str.toLowerCase();
		int count = 0;
		for (int i = 0; i < str.length(); i++) {
			char current = str.charAt(i);
			if (current == 'a' || current == 'e' || 
			current == 'i' || current == 'o' || current == 'u') {
				count++;
			}
		}
		return count;
	}
	
	public static void main(String[] args) {
		greeting("Wilton");
		System.out.println(vowelCount("Wilton"));
	}
}
```
Now we can add our changed files and commit them to the 'dev' branch like we would normally, since we are currently on that branch. 
```
$ git add -A
$ git commit -m "Added vowelCount funct."
```
However, these changes will not be reflected on our original branch. Let's  switch back to our original branch.

`$ git checkout <original branch name>`

Now we're going to make some changes to our program back on the original branch. Let's add a function that reverses a string. 

```
public class Tutorial {
	public static void greeting(String name) {
		System.out.println("Hello " + name + "!");
	}
	
	public static String reverseStr(String str) {
		String result = "";
		for (int i = str.length() - 1; i >= 0; i--) {
			result += str.charAt(i);
		}
		return result;
	}
	
	public static void main(String[] args) {
		greeting("Wilton");
		System.out.println(reverseStr("Wilton"));
	}
}
```
Let's commit these changes to the original branch. 
```
$ git add -A
$ git commit -m "Added reverseStr funct."
```
These changes have been recorded only on the original branch, our 'dev' branch will not display these changes. 

## Merging
So now we have a situation where two different branches have different changes to them. However, we would like to incorporate both of these changes, so a solution to this situation would be to merge the branches. We can do this by using the 'merge' command to integrate them into one single branch, which will be the branch you are currently on.

`$ git merge dev`

However, we may get a message like:
```
CONFLICT (content): Merge conflict in Tutorial.java
Automatic merge failed; fix conflicts and then commit the result.
```
which states that we have a merge conflict. This is because both branches changed the same areas of one file. In order to resolve this, we can open the offending file and we will see something that looks like:
```
public class Tutorial {
	public static void greeting(String name) {
		System.out.println("Hello " + name + "!");
	}

<<<<<<< HEAD
	public static String reverseStr(String str) {
		String result = "";
		for (int i = str.length() - 1; i >= 0; i--) {
			result += str.charAt(i);
		}
		return result;
=======
	public static int vowelCount(String str) {
		str = str.toLowerCase();
		int count = 0;
		for (int i = 0; i < str.length(); i++) {
			char current = str.charAt(i);
			if (current == 'a' || current == 'e' || 
			current == 'i' || current == 'o' || current == 'u') {
				count++;
			}
		}
		return count;
>>>>>>> dev
	}
	
	public static void main(String[] args) {
		greeting("Wilton");
<<<<<<< HEAD
		System.out.println(reverseStr("Wilton"));
=======
		System.out.println(vowelCount("Wilton"));
>>>>>>> dev
	}
}
```
Now we should edit the file, informed by the indicators displaying the differences between the HEAD (latest commit on current branch) and the 'dev' branch we are integrating. After cleaning it up based on this information and removing the indicators, it looks like:
```
public class Tutorial {
	public static void greeting(String name) {
		System.out.println("Hello " + name + "!");
	}

	public static String reverseStr(String str) {
		String result = "";
		for (int i = str.length() - 1; i >= 0; i--) {
			result += str.charAt(i);
		}
		return result;

	public static int vowelCount(String str) {
		str = str.toLowerCase();
		int count = 0;
		for (int i = 0; i < str.length(); i++) {
			char current = str.charAt(i);
			if (current == 'a' || current == 'e' || 
			current == 'i' || current == 'o' || current == 'u') {
				count++;
			}
		}
		return count;
	}
	
	public static void main(String[] args) {
		greeting("Wilton");
		System.out.println(reverseStr("Wilton"));
		System.out.println(vowelCount("Wilton"));
	}
}
```
with the changes we made on both branches fully synthesized. In order to fix the conflict, we must add the file again and commit.
```
$ git add Tutorial.java
$ git commit -m "Fixed merge conflict."
```
We can then delete the 'dev' branch.

`$ git branch -d dev`

## Stashing
A convenient feature in Git is stashing. Let's say we've started working on a new file within the same project directory called 'FizzBuzz.java'. 

`$ touch FizzBuzz.java`

However, we now have the desire to make some changes to our original 'Tutorial.java' file, but we don't want any commits we make to include the new 'FizzBuzz.java' file. What we can do is stash the changes we've made, and come back later after we commit, with: 

`$ git stash -u`

The 'u' argument signifies that this is an untracked file, this argument when working with files that are already added. Now if we look within our project directory, the new file is now gone. We can then make changes and commits without including the new file, and if we want to get it back we can run:

`$ git stash pop`

and now the file is back and visible again within our directory.

## Other commands
Here are some other Git commands which you may find useful:

`$ git status` — displays the status of your working tree, whether it is up to date with the remote repository, and if you have untracked files.

`$ git log` — displays the history of commits, with the HEAD tag signifying the latest commit.

`$ git revert <commit ID>` — allows you to revert the commit specified.

`$ git pull` — if synced with a remote repository, allows you to fetch the latest changes and merge them.