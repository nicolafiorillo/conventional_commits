# Conventional Commits

This is the (opinionated) format of conventional commit message specification I tend to follow.
It adheres to the (Conventional commits)[https://www.conventionalcommits.org] specifications.

## Goals

* allow generating CHANGELOG.md by script
* allow ignoring commits by git (not important commits like formatting)
* provide better information when browsing the history

## Structure

Data in [] are optional.

```
<type>[scope][!]: <subject (imperative, present tense, lowercase, no dot at end)>

[ Body section.

  Motivation for the change and contrast with previous behaviour.

  Can span multiple lines.
]

[BREAKING CHANGE: <description, multiline>]

[<footer(s)>]

[
  - Additional links and meta-information
]
```

## Types

```
type = fix¦feat  (custom types are allowed)
```

* `!`: draw attention to breaking change
* `fix`: patches a bug in your codebase (this correlates with PATCH in Semantic Versioning)
* `feat`: introduces a new feature to the codebase (this correlates with MINOR in Semantic Versioning)

`BREAKING CHANGE`: correlating with MAJOR in Semantic Versioning; can be part of commits of any type

### Other useful (custom) types

* `revert`: commit reverts previous commit
* `build`: changes affect the build system or external dependencies (example scopes: npm)
* `ci`: changes to CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs)
* `docs`: changes to documentation only
* `feat`: new feature
* `fix`: bug fix
* `perf`: changes that improves performance
* `refactor`: changes that neither fixes a bug nor adds a feature
* `style`: changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
* `test`: adding missing tests or correcting existing tests
* `chore`: all other changes

## Scope

The scope should be the name of the business package/macro-functionality affected (as perceived by the person reading the changelog generated from commit messages).
Someone use it in the form of `$a_scope`.

You can use `*` or `other` if there isn't a more fitting scope.

## Subject

* use the imperative, present tense (eg. "change" not "changed" nor "changes")
* don't capitalize the first letter
* no dot (.) at the end

## Body

Use the imperative, present tense (see Subject).

The body should include the motivation for the change and contrast this with previous behavior.

## Footer

Can be wrote similar to git trailer format `<key>: <description>` (eg. `Issue: JIRA-1234`) or simple text (eg. `Closes JIRA-1234`, `Fixes JIRA-1234`).

## Examples

```
feat: allow provided config object to extend other configs

BREAKING CHANGE: `extends` key in config file is now used for extending other config files
```

```
chore!: drop support for Node 6

BREAKING CHANGE: use JavaScript features not available in Node 6.
```

```
docs: correct spelling of CHANGELOG
```

```
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are
obsolete now.

Reviewed-by: Z
Refs: #123
```

```
feat: new best feature

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Refs: JIRA-12345
```

```
fix($compile): couple of unit tests for IE9

Older IEs serialize html uppercased, but IE9 does not...
Would be better to expect case insensitive, unfortunately jasmine does
not allow to user regexps for throw expectations.

Closes #392
```

```
docs(guide): updated fixed docs from Google Docs

Couple of typos fixed:
- indentation
- batchLogbatchLog -> batchLog
- start periodic checking
- missing brace
```

```
fix!: a fix

BREAKING CHANGE: isolate scope bindings definition has changed and
    the inject option for the directive controller injection was removed.
    
    To migrate the code follow the example below:
    
    Before:
    
    scope: {
      myAttr: 'attribute',
      myBind: 'bind',
      myExpression: 'expression',
      myEval: 'evaluate',
      myAccessor: 'accessor'
    }
    
    After:
    
    scope: {
      myAttr: '@',
      myBind: '@',
      myExpression: '&',
      // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
      myAccessor: '=' // in directive's template change myAccessor() to myAccessor
    }
    
    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

## Generating `CHANGELOG.md`
Commands to generate a skeleton of changelog file.

List of all subjects (first lines in commit message) since last release:
```
git log <last tag> HEAD --pretty=format:%s
```

New features in this release
```
git log <last release> HEAD --grep feature
```

Recognizing unimportant commits:
```
git bisect skip $(git rev-list --grep irrelevant <good place> HEAD)
```

