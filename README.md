# libGitModule
Implements the libGit library as a drop-in module to be used with Torque3D

# Instructions:

In order to utilize libgit, you must first install the module into the data/ directory of your project, and then regenerate and compile the module in.

## Usage in script
Once the executable is updated, you must then initialize the git subsystem in script, like so:
`
git_init();
   
new gitObject(GitConnection){};
`

When you're done with the work, clean it up like so:
`
GitConnection.delete();
   
git_shutdown();
`

To actually grab and work with a git repository, you must first clone the repository:
`
%resultCode = GitConnection.cloneRepo(%targetFolderPath, %gitPath);
`

Once the initial clone is done, you then open the repository with this call:
`
%resultCode = GitConnection.openRepo(%targetFolderPath, %gitPath);
`

And then you can check the status of the repo which will validate the state of the repo vs the origin and a given branch:
`
%resultCode = GitConnection.checkState("origin", %branchName);
`

And if required, you can pull updates once the repository is open:
`
GitConnection.update("origin", %branchName);
`

## Callback functions
The main callback functions to worry about are:
`
function GitConnection::onStart(%this, %stage, %errCode)
`
Which is called when an action is started, such as downloading the repository to local.

`
function GitConnection::onProgress(%this, %stage, %fetchPrct, %checkoutPrct)
`
Which is called when a step of progress is made, allowing one to track the overall progress of the operation

`
function GitConnection::onComplete(%this, %stage, %errCode)
`
And when the operation is complete.

If something goes wrong, you can also at any time call:
`
%errorMessage = GitConnection.getLastError();
`
Which will return the string of the last error message to occur allowing feedback or debugging when something goes wrong.