## Create a CodeRoad tutorial

Follow these instructions carefully to create your first CodeRoad tutorial.

### Prerequisites
- npm
- Git
- Github
- VS Code
- CodeRoad VS Code extension (Install this in VS Code)
- [CodeRoad CLI tools](https://www.npmjs.com/package/@coderoad/cli)
Install the CodeRoad CLI tools with `npm install -g @coderoad/cli`

### Create a repo
- Go to GitHub and create a new repository for yourself named `first-tutorial`.
- After you click create, it takes you to the repo. Copy the URL for the repo, it should look like: `https://github.com/your-username/first-tutorial.git`.
- Open a terminal locally and find a place to clone your repo. Enter `git clone https://github.com/your-username/first-tutorial.git` with the repo URL you copied in place of that URL to clone it.
- Create a `.gitignore` file in your repo and add this to it:
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
- Push them to github with `git push origin master`

### Create a version branch
- Create and checkout a new orphan branch with `git checkout --orphan v0.1.0`.
This will make a branch that isn't created from master, so it has no commit history. It will hold the tests for your tutorial. Each test is its own commit. You can also add an optional commit for a solution to each test.
- Check your `git status`.
- Delete the tutorial file from this branch with `git rm -f TUTORIAL.md`

### Create your project files
This branch is also where users create their projects and modify files for a tutorial, and most anything they need to do.
- Make a new folder named `coderoad` on your branch.
This folder will hold as much of the CodeRoad stuff as it can so users aren't confused with extra files in their projects.
- Go to the `coderoad` folder in your terminal and run `npm init`. Press enter until you are through the setup. 
- Open the `package.json` file you just made and make it look like this...

```js
{
  "name": "coderoad",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "programmatic-test": "mocha --reporter=mocha-tap-reporter",
    "test": "mocha"
  },
  "author": "",
  "license": "ISC"
}

```

These scripts will be for CodeRoad and you to test things.

- Run this from the terminal in your `coderoad` folder to install mocha: `npm install --save mocha mocha-tap-reporter`
- Make sure you go back to the main repo folder and add your changes with `git add .`
- Commit your files with `git commit -m "INIT"`
The message of `INIT` in all caps is necessary. This message is used to add project setup files and anthing else you want to add before a user starts the tutorial.
- Push and Create a branch on your remote with `git push -u origin v0.1.0` 

### Create the first test
- Go back in the `coderoad` folder and new folder named `test` in it
- Create a file named `first-tutorial.test.js` in the `test` folder
- Add this to the file:

```js
const assert = require('assert');
const fs = require('fs');
const path = require('path');

const filesInDir = fs.readdirSync(path.resolve(__dirname, '../..'), 'utf8');

describe("first-tutorial folder", () => {
  it('should have an index.html file', async () => {
		assert(filesInDir.indexOf('index.html') >= 0)
  });
});
```

This will be the test for the one lesson in your tutorial. You can see that it checks for the existence of an `index.html` file in the parent folder.

- In the `coderoad` folder, run `npm run programmatic-test` from the terminal.
It will fail since `index.html` doesn't exist.
- Create an `index.html` file in the main repo folder and run the test again.
It should pass this time. So when a user creates the `index.html` file, this test will run, and the lesson will pass.

### Commit your first test
- Go back to the main repo folder and add the test file to be committed with `git add coderoad/.`
- Commit it with `git commit -m "L1S1Q"`
That stands for "Lesson 1 Step 1 Question". You can put an additional note after it, but it has to start with those letters so CodeRoad knows that this is the test for the L1S1 step.
- After that, add the index file with `git add index.html`
- Commit the file with `git commit -m "L1S1A"`.
That stands for "Lesson 1 Step 1 Answer", and it's the solution to the test.
- Take a quick look at the commit history with `git log`. You can see the messages there, they align with the titles you put in the markdown.
- Push your changes to github with `git push -u origin v0.1.0`

### Create the YAML file
- Go back your your master branch with `git checkout master`
Think of these two branches like separate repositories, since the branch will never merge and the files will mostly be different.
- Create a new file named `coderoad.yaml` and add this to it:

```yml
version: '0.1.0'
config:
  testRunner:
    command: npm run programmatic-test
    args:
      filter: --grep
      tap: --reporter=mocha-tap-reporter
    setup:
      commands:
        - npm install
    directory: coderoad
  repo: 
    uri: https://github.com/moT01/first-repo
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

Replace the `repo uri` URL with your github repo, notice that it's just the username and repo. This file links everything together. You can see the repo URL and the branch that you created. And the `L1` and `L1S1` id's. You can also add commands that will run when a lesson is started, as well as a host of other things. You can [find out more here](https://github.com/coderoad/coderoad-cli/blob/master/src/templates/js-mocha/coderoad.yaml).

- Add this with `git add .`
- Commit it with `git commit -m "create yaml"`
The commit messages on master can be whatever you want.
- Push it to github with `git push origin master`

### Build the config.json file
You created the three things a tutorial needs from you; the markdown, the commits, and the yaml. Now you can build it. If you haven't installed the CodeRoad CLI tools, use `npm install -g @coderoad/cli` to do it.
- Run `coderoad build` from the terminal on the master branch
If you didn't get any errors, it will have created a `tutorial.json` file which is what CodeRoad uses to find all the files, commits, and instructions you created.

### Open your tutorial
To check out your tutorial:
Install the CodeRoad extension to VS Code if you haven't already
- Open a new VS Code window
- Put a **single empty folder** in the workspace
- Open the command palette with `ctrl+shift+p`
- Enter `CodeRoad: Start` in the search to start CodeRoad
- Click `Start New Tutorial`
- Click the `File` option
- Click `Upload`
- Find the `tutorial.json` file that you created in your repo folder and upload it

### Review
Success! You can see the introduction page. It may not be a bad idea to take a look at the markdown file again to see text next to the running tutorial. 

Notice that when you click the `start` button, you can see that `npm install` is run in the `coderoad` folder and the `node_modules` are created, and your dependencies are installed.

After you click start, open up any file and press `cmd+s` to save. This will run the tests. They should fail. After you check that, create the `index.html` file, and save it. The tests should run and pass this time :smile:
