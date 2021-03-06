# Intro

## Git ignore .next
* If you added `.next ` accidentilly to git you can remove from the cache

`$ git rm -r --cached .next`

## Create your main app folder
`$ take root-app-folder`

## Initialize git
`$ git init`

## Create Feature Branch

`$ git checkout -b intro`

## Create `buy-something` to hold your app
* Inside create 2 folders:
  - `frontend`
  - `backend`

## frontend and backend
* Create a folder for each
* Use `.vscode` folder with `settings.json`

### frontend - .vscode/settings.json
* yellow bg

```json
{
  "workbench.colorCustomizations": {
    "titleBar.activeForeground": "#000",
    "titleBar.inactiveForeground": "#000000CC",
    "titleBar.activeBackground": "#FFC600",
    "titleBar.inactiveBackground": "#FFC600CC"
  }
}
```

### backend - .vscode/settings.json
```json
{
  "workbench.colorCustomizations": {
    "titleBar.activeBackground": "#FF2C70",
    "titleBar.inactiveBackground": "#FF2C70CC"
  }
}
```

## custom
* Make sure you set your VS Code title to `custom`
* By default it is `native`
* `cmd` + `,` search for `title` and change to `custom`

## Open terminal in 2 tabs
* One for `frontend`
* One for `backend`

### Install dependencies in each folder
* TODO: What will we be installing?

`$ cd frontend && npm i`

`$ cd ../ && cd backend && npm i`

## Create these:
* `$ touch .gitignore`

```
# Logs
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Runtime data
pids
*.pid
*.seed
*.pid.lock

# Directory for instrumented libs generated by jscoverage/JSCover
lib-cov

# Coverage directory used by tools like istanbul
coverage

# nyc test coverage
.nyc_output

# Grunt intermediate storage (http://gruntjs.com/creating-plugins#storing-task-files)
.grunt

# Bower dependency directory (https://bower.io/)
bower_components

# node-waf configuration
.lock-wscript

# Compiled binary addons (https://nodejs.org/api/addons.html)
build/Release

# Dependency directories
node_modules/
jspm_packages/

# TypeScript v1 declaration files
typings/

# Optional npm cache directory
.npm

# Optional eslint cache
.eslintcache

# Optional REPL history
.node_repl_history

# Output of 'npm pack'
*.tgz

# Yarn Integrity file
.yarn-integrity

# dotenv environment variables file
*.env

# next.js build output
.next

# vuepress build output
.vuepress/dist

# Serverless directories
.serverless

.DS_Store
```

## Add eslint
`$ touch .eslintrc`

`.eslintrc`

```
{
  "extends": [
    "airbnb",
    "prettier",
    "prettier/react"
  ],
  "parser": "babel-eslint",
  "parserOptions": {
    "ecmaVersion": 8,
    "ecmaFeatures": {
      "experimentalObjectRestSpread": true,
      "impliedStrict": true,
      "classes": true
    }
  },
  "env": {
    "browser": true,
    "node": true,
    "jquery": true,
    "jest": true
  },
  "rules": {
    "no-debugger": 0,
    "no-alert": 0,
    "no-unused-vars": [
      1,
      {
        "argsIgnorePattern": "res|next|^err"
      }
    ],
    "prefer-const": [
      "error",
      {
        "destructuring": "all",
      }
    ],
    "arrow-body-style": [
      2,
      "as-needed"
    ],
    "no-unused-expressions": [
      2,
      {
        "allowTaggedTemplates": true
      }
    ],
    "no-param-reassign": [
      2,
      {
        "props": false
      }
    ],
    "no-console": 0,
    "import/prefer-default-export": 0,
    "import": 0,
    "func-names": 0,
    "space-before-function-paren": 0,
    "comma-dangle": 0,
    "max-len": 0,
    "import/extensions": 0,
    "no-underscore-dangle": 0,
    "consistent-return": 0,
    "react/display-name": 1,
    "react/no-array-index-key": 0,
    "react/react-in-jsx-scope": 0,
    "react/prefer-stateless-function": 0,
    "react/forbid-prop-types": 0,
    "react/no-unescaped-entities": 0,
    "jsx-a11y/accessible-emoji": 0,
    "react/jsx-filename-extension": [
      1,
      {
        "extensions": [
          ".js",
          ".jsx"
        ]
      }
    ],
    "radix": 0,
    "no-shadow": [
      2,
      {
        "hoist": "all",
        "allow": [
          "resolve",
          "reject",
          "done",
          "next",
          "err",
          "error"
        ]
      }
    ],
    "quotes": [
      2,
      "single",
      {
        "avoidEscape": true,
        "allowTemplateLiterals": true
      }
    ],
    "prettier/prettier": [
      "error",
      {
        "trailingComma": "es5",
        "singleQuote": true,
        "printWidth": 100,
      }
    ],
    "jsx-a11y/href-no-hash": "off",
    "jsx-a11y/anchor-is-valid": [
      "warn",
      {
        "aspects": [
          "invalidHref"
        ]
      }
    ]
  },
  "plugins": [
    "prettier"
  ]
}
```

## Create README
`$ touch README.md`

`README.md`

# TODO: Add Instructions for this app

## Add eslint and prettier plugins
* In root create a `package.json`

`/.package.json`

```
$ npm i -D eslint eslint-config-airbnb eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-react react-testing-library prettier eslint-plugin-prettier husky pretty-quick prettier`
```

## Formatting Code Automatically [source](https://github.com/Ihatetomatoes/react-image-slideshow)

Prettier is an opinionated code formatter with support for JavaScript, CSS and JSON. With Prettier you can format the code you write automatically to ensure a code style within your project. See the [Prettier's GitHub page](https://github.com/prettier/prettier) for more information, and look at this [page to see it in action](https://prettier.github.io/prettier/).

To format our code whenever we make a commit in git, we need to install the following dependencies:

```sh
npm install --save husky lint-staged prettier
```

* `husky` makes it easy to use githooks as if they are npm scripts.
* `lint-staged` allows us to run scripts on staged files in git. See this [blog post about lint-staged to learn more about it](https://medium.com/@okonetchnikov/make-linting-great-again-f3890e1ad6b8).
* `prettier` is the JavaScript formatter we will run before commits.

Now we can make sure every file is formatted correctly by adding a few lines to the `package.json` in the project root.

Add the following line to `scripts` section:

```diff
  "scripts": {
+   "precommit": "lint-staged",
    "start": "react-scripts start",
    "build": "react-scripts build",
```

Next we add a 'lint-staged' field to the `package.json`, for example:

```diff
  "dependencies": {
    // ...
  },
+ "lint-staged": {
+   "src/**/*.{js,jsx,json,css}": [
+     "prettier --single-quote --write",
+     "git add"
+   ]
+ },
  "scripts": {
```

Now, whenever you make a commit, Prettier will format the changed files automatically. You can also run `./node_modules/.bin/prettier --single-quote --write "src/**/*.{js,jsx,json,css}"` to format your entire project for the first time.

### Update on husky
* This has changed and you will get this error in the terminal

```
Warning: Setting pre-commit script in package.json > scripts will be deprecated
Please move it to husky.hooks in package.json, a .huskyrc file, or a husky.config.js file
Or run ./node_modules/.bin/husky-upgrade for automatic update
```

* So run `$ ./node_modules/.bin/husky-upgrade`
  - And you will get this change added to your `package.json`

`package.json`

```
// MORE CODE

"husky": {
     "hooks": {
       "pre-commit": "lint-staged"
    }
  }
 }
```

* Now add and commit and you will get something like this in the terminal:

![husky and lint-staged working](https://i.imgur.com/S0HbOnu.png)

## Git and Github workflow
* For each page of notes you should follow this workflow
* Make sure you create a remote github repo
* Check if you connected it with `$ git remote -v`
  - Add with `$ git remote add origin GITHUB_REPO_URL_HERE`
* Are you forking a team project?
  - If so you'll also need to set up `upstream`

1. Check Status 

`$ git status`

(use my alias: `$ gs`)

2. Create a branch `$ git checkout -b branch-name`

If your code is working you can add to stage and commit it

3. Add to staging

`$ git add -A`

(use my alias: `$ ga -A`)

4. Commit with useful commit message

`$ git commit -m 'Useful message`

* Use my shortcut `$ gc -m 'Useful message`
* **note** Present tense message
* Combine add and commit for modifications only `$ gc -am 'Useful message`

4. Push Branch to Origin

`$ git push origin branch-name`

5. Create PR on Origin

Remotely: Do this on Github
* Code Review on Origin (Project Manager does this)
* Merge to master branch on Origin (or reject and don't merge - Project Manager does this)

Locally: On your computer

6. Check out of feature branch and into master branch

`$ git checkout master`

7. Fetch locally

`$ git fetch`

8. Git Diff to see changes

`$ git diff`

9. Pull Locally

`$ git pull origin master`

10. Delete local branch

`$ git branch -D branch-name`

Rinse and repeat. Early and Often.

## If you are working on your own project
* Here is a shorter way
* Create a new branch
* Add and commit
* Checkout to master
* Merge your branch yourself

`$ git merge branch-name`

* Remove the branch `$ git branch -D branch-name`
* Push to github

`$ git push origin master`
