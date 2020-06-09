## Create a CodeRoad tutorial

Follow these instructions carefully to create your first CodeRoad tutorial.

### Prerequisites
- npm
- Git
- Github
- VS Code
- CodeRoad VS Code extension
Install this in VS Code
- [CodeRoad CLI tools](https://www.npmjs.com/package/@coderoad/cli)
Install the CodeRoad CLI tools with `npm install -g @coderoad/cli`

### Create a repo
- Go to GitHub and create a new repository for yourself named `first-tutorial`.
- After you click create, it takes you to the repo. Copy the URL for the repo, it should look like: `https://github.com/your-username/first-tutorial.git`.
- Open a terminal locally and find a place to clone your repo. Enter `git clone https://github.com/your-username/first-tutorial.git` with the repo URL you copied in place of that URL to clone it.
- Create a `.gitignore` file in your repo and add this in it:
```md
node_modules
package-lock.json
```
Add anything else that may interfere such as `.DS_Store` if you are on a mac.

### Create the markdown
- Create a new file in your repo named `TUTORIAL.md`.

This is the file that describes the structure of a tutorial. It contains all the lessons, lesson titles, descriptions, test text and all the other verbiage that will be displayed to a user. Enter this markdown into the file and save it:

```md

# Introduction 

This is an introduction to your tutorial. It will show up on the first page when your tutorial is started.

## L1 Name of first lesson

> Optional summary for L1

Here's where you can put a description, examples, and instructions for the lesson.

### L1S1 Level 1 Step 1

This is the test text. Create an `index.html` file to pass this lesson.

#### HINTS

* This is a hint to help people through the test
* Second hint for L1S1, don't worry if the hints don't show up yet
```

The above tutorial has an introduction page and one lesson. Note that the "lessons" need to start with `## Ln` (e.g. `## L1`) and "steps", or tests, need to start with `### LnSn` (e.g. `### L1S1`).

### Commit to github
- Back in the terminal, add all your new files to be committed with `git add .`
- Commit them with `git commit -m "create markdown"`
- Push them to your repository with `git push origin master`

### Create a version branch
- Create and checkout a new orphan branch with `git checkout --orphan v0.1.0`.
This will make a branch that isn't create from master, so it has no commit history. It will hold the tests for your tutorial. Each test is its own commit. You can also add an optional commit for a solution to each test.
- Check your `git status` if you need to.
- Delete the tutorial file with `git rm TUTORIAL.md`

### Create your project files
- Make a new folder named `coderoad` on your branch.
- Go to the `coderoad` folder in your terminal and run `npm init`. Press enter until you are through the setup. 
- Open the `package.json` file you just made and make it look like this...

```js
{
  "name": "first-tutorial",
  "version": "0.1.0",
  "description": "",
  "main": "index.js",
  "repository": {
    "type": "git",
    "url": "https://github.com/your-username/first-tutorial-1.git"
  },
  "scripts": {
    "programmatic-test": "mocha --reporter=mocha-tap-reporter",
    "start": "node src/server.js",
    "test": "mocha"
  }
}
```

These scripts will be run by CodeRoad to test things.

- Run this from the terminal in your `coderoad` folder to install mocha: `npm install --save mocha mocha-tap-reporter`
- Add your changes with `git add .`
- Commit your files with `git commit -m "INIT"`
The message of `INIT` in all caps is necessary. This is the title you can use for project setup files, which is what you just added is.
- Push and create a branch on your remote with `git push -u origin v0.1.0` 

### Create the first test
- Create a file named `first-tutorial.test.js`
- add this to the file:
```js
const assert = require('assert');
const fs = require('fs');
const path = require('path');

const filesInDir = fs.readdirSync(path.resolve(__dirname, '..'), 'utf8');

describe("index.html", () => {
  it('should have an index.html file', async () => {
		assert(filesInDir.indexOf('index.html') >= 0)
  });
});
```
This will be the test for the one lesson in your tutorial. You can see that it checks for the existence of an `index.html` file in the parent folder.

- In this folder, run `npm run programmatic-test` from the terminal.
It will fail since the file doesn't exist.
- Create an `index.html` file in the parent folder and run the test again.
It should pass this time.

### Commit your first test
Now that you know your test passes after someone creates the `index.html` file, you can add it as the test.
- Add just your test file to be committed with `git add first-tutorial.test.js`
- Commit it with `git commit -m "L1S1Q"`
That stands for "Lesson 1 Step 1 Question". You can put an additional note after it, but it has to start with those letters so CodeRoad knows that this is the test for the L1S1 step.
- After that, add the index file with `git add index.html`
- Commit the file with `git commit -m "L1S1A"`.
That stands for "Lesson 1 Step 1 Answer", and it's the solution to the test.
- Push your changes to github with `git push -u origin v0.1.0`

### Create the YAML file
- Go back your your master branch with `git checkout master`
Your tutorial file should show back up.
- Create a new file named `coderoad.yaml` and add this to it:

```yml
version: '0.1.0'
config:
  testRunner:
    command: npm run programmatic-test
    args:
      filter: --grep
      tap: --reporter=mocha-tap-reporter
    directory: coderoad
  repo: 
    uri: https://github.com/your-username/first-tutorial
    branch: v0.1.0
  dependencies:
    - name: node
      version: '>=10'
levels:
  - id: L1
    steps:
      - id: L1S1
        setup:
          commands:
            - npm install
```

Replace the `repo.uri` URL with your github repo. This file links everything together. You can see the repo URL and the branch that you created. And the `L1` and `L1S1` id's. You can also add commands that will run when a lesson is started, as well as a host of other things.

- Add this with `git add .`
- Commit it with `git commit -m "create yaml"`
The commit messages on master can be whatever you want.
- Push it to github with `git push origin master`

### Build the config.json file
You created the three things a tutorial needs from you; the markdown, the commits, and the yaml. Now you can build it. If you haven't installed the CodeRoad CLI tools, use `npm install -g @coderoad/cli` to do it.
- Run `coderoad build` from the terminal on the master branch
If you didn't get any errors, it will have created a `tutorial.json` file which is loaded into CodeRoad.

### Open your tutorial
To check out your tutorial:
Install the CodeRoad extension to VS Code if you haven't already
- Open a new VS Code window
- Put a single empty folder in the workspace
- Open the command palette with `ctrl+shift+p`
- Enter `CodeRoad: Start` to start CodeRoad
- Click `Start new Tutorial`
- Click the `file` option
- Click `upload`
- Find the `tutorial.json` file that you created in your repo folder and upload it
- 

### Create a second lesson