## Create your first CodeRoad tutorial

Follow these instructions carefully to create your first CodeRoad tutorial.

### Prerequisites
- Git
- Github
- [CodeRoad CLI tools](https://www.npmjs.com/package/@coderoad/cli)
Install the CodeRoad CLI tools with `npm install -g @coderoad/cli`

### Create the repo
- Go to GitHub and create a new repository for yourself named `first-tutorial`.
- After you click create, it takes you to the repo. Copy the URL for the repo, it should look like: `https://github.com/your-username/first-tutorial.git`.
- Open a terminal locally and find a place to clone your repo. Enter `git clone https://github.com/your-username/first-tutorial.git` with the repo URL you copied in place of that URL to clone it.

### Create .gitignore
- In your new repository create a `.gitignore` file.
- Add `node_modules` to the file and any other things you want to ignore. Maybe `.DS_Store` if you're on a mac

### Create the markdown
- Create a new file in your repo named `TUTORIAL.md`.

This is the file that describes the structure of a tutorial. It contains all the lessons, lesson titles, descriptions, test text and all the other verbiage that will be displayed to a user. Enter this markdown into the file and save it:

```md
# Introduction 

This is an introduction to your tutorial. It will show up on the first page when your tutorial is started.

## L1 Lesson 1

> Optional summary for L1

Here's where you can put a description, examples, and instructions for the lesson.

### L1S1 Level 1 Step 1

This is the test text for L1S1

#### HINTS

* This is a hint to help people through the test
* And a second hint, don't worry if they don't show up yet.
```

The above tutorial has an introduction page and one lesson. Note that the "lessons" need to start with `## Ln` (e.g. `## L1`) and "steps", or tests, need to start with `### LnSn` (e.g. `### L1S1`).

### Commit to github
- Commit the stuff?

- Back in the terminal create and switch to a new branch with `git checkout -b v0.1.0`.

### Create your project files
- create a new branch
- delete the files
- npm init
- make your package.json file look like this...

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
  },
  "devDependencies": {
    "mocha": "^7.0.1",
    "mocha-tap-reporter": "^0.1.3",
  }
}
```
- npm install --save mocha mocha-tap-reporter
- git add
- git commit -m "INIT"
The commit messages are important, so make sure use that exact commit message in all caps.
- `git push -u origin v0.1.0` to push your new branch to github

### Create the first test
- create a file named `first-tutorial.test.js`
- add this to the file:
```js
const assert = require("assert");

describe("package.json", () => {
  // L1S1
  it('always true test', async () => {
    assert.equal(true, true);
  });
});
```

### Create the YAML file
- Create a new file named `coderoad.yaml`. This is where coderoad 

```yml
version: '0.1.0'
config:
  testRunner:
    command: npm run programmatic-test
    args:
      filter: --grep
      tap: --reporter=mocha-tap-reporter
  repo: 
    uri: https://github.com/moT01/mini-tutorial-1
    branch: v0.1.0
  dependencies:
    - name: node
      version: '>=10'
levels:
  - id: L10
    steps:
      - id: L10S1
        setup:
          commands:
            - npm install
          files:
            - package.json
        solution:
          files:
            - package.json
  - id: L20
    steps:
      - id: L20S1
        setup:
          commands:
            - npm install
          files:
            - package.json
        solution:
          files:
            - package.json
```