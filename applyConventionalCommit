
**Conventional Commits** provides an easy set of rules for creating an explicit commit history. This leads to more readable messages that are easy to follow when looking through the project history. [Read full of spec and samples of Conventional Commits.](https://conventionalcommits.org) 

#### Commit Message Format
```
<type>([optional scope]): <description>
<BLANK LINE>
[optional body]
<BLANK LINE>
[optional footer(s)]
```
#### **Type**
Must be one of the following:
- **build**: Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)
- **ci**: Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)
- **docs**: Documentation only changes
- **feat**: introduces a new feature to the codebase (this correlates with **MINOR** in Semantic Versioning)
- **fix**: A bug fix (this correlates with **PATCH** in Semantic Versioning)
- **perf**: A code change that improves performance
- **refactor**: A code change that neither fixes a bug nor adds a feature
- **style**: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- **test**: Adding missing tests or correcting existing tests

#### **Scope**
A scope MAY be provided after a type. A scope MUST consist of a noun describing a section of the codebase surrounded by parenthesis, e.g., fix(parser):

Scope enums should be defined base on specific project context.

#### **Description**
A succinct description of the change:

- use the imperative, present tense: "change" not "changed" nor "changes"
- don't capitalize the first letter
- no dot (.) at the end


#### **Samples** ([more real samples](https://github.com/angular/angular/commits/master))

Commit message with the description and breaking change footer
```
feat: allow provided config object to extend other configs

BREAKING CHANGE: `extends` key in the config file is now used for extending other config files
```
```
docs(changelog): update changelog to beta.5
```

```
fix(release): need to depend on latest rxjs and zone.js

The version in our package.json gets copied to the one we publish, and users need the latest of these.
```


## What is commitlint

**commitlint** checks if your commit messages meet the **[Conventional Commits](https://git3.nexdev.net/sc-packages/sc-game-base/-/wikis/Apply-Conventional-Commits#commit-message-format)**


## Install commitlint and husky


```sh
npm install --save-dev @commitlint/config-conventional @commitlint/cli
```

To lint commits before they are created you can use Husky's `commit-msg` hook:

```sh
# Install Husky v6
npm install husky --save-dev
# or
yarn add husky --dev

# Activate hooks
npx husky install
# or
yarn husky install

# Add hook
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
```

Add file **.commitlintrc.json** with below configurations

```sh
{
    "extends": [
        "@commitlint/config-conventional"
    ]
}
```

For projects which already have commitlint and husky configurations, developers should run this command to add husky hook for the first time.


```sh
# Install packages
npm install
# or
yarn install

# Activate hooks
npx husky install
# or
yarn husky install

```


