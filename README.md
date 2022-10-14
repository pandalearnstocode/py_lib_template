```bash
git init
git add README.md
git commit -m "first commit"
git branch -M init
git remote add origin https://github.com/pandalearnstocode/py_lib_template.git
git push -u origin init
```

## __Branch name check__

```yml
name: 'Assert Branch Naming Convention'
on: pull_request

jobs:
  branch-naming-rules:
    runs-on: ubuntu-latest
    steps:
      - uses: deepakputhraya/action-branch-name@master
        with:
          regex: '([a-z])+\/([a-z])+' # This we need to check how to add pattern for `feat/AB#125-branch-name-validation`,`fix/AB#125-branch-name-validation`
          allowed_prefixes: 'feat,fix,docs,style,refactor,perf,test,build,ci,chore,revert'
          ignore: master,staging,develop
          min_length: 5
          max_length: 20
```


## __PR labeler__

### __`.github/workflows/pr-labeler.yml`__

```yml
name: PR Labeler
on:
  pull_request:
    types: [opened]

jobs:
  pr-labeler:
    runs-on: ubuntu-latest
    steps:
      - uses: TimonVS/pr-labeler-action@v3
        with:
          configuration-path: .github/pr-labeler.yml # optional, .github/pr-labeler.yml is the default value
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### __`.github/pr-labeler.yml`__

```yml
feature: ['feature/*', 'feat/*']
fix: fix/*
chore: chore/*
```


## __PR validation__

```yaml
name: "Lint PR"
on:
  pull_request_target:
    types:
      - opened
      - edited
      - synchronize
jobs:
  main:
    name: Validate PR title
    runs-on: ubuntu-latest
    steps:
      - uses: amannn/action-semantic-pull-request@v5
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          types: |
            fix
            feat
          scopes: |
            core
            ui
          requireScope: true
          disallowScopes: |
            release
          subjectPattern: ^(?![A-Z]).+$
          subjectPatternError: |
            The subject "{subject}" found in the pull request title "{title}"
            didn't match the configured pattern. Please ensure that the subject
            doesn't start with an uppercase character.
          githubBaseUrl: https://github.myorg.com/api/v3
          ignoreLabels: |
            bot
            ignore-semantic-pull-request
          headerPattern: '^(\w*)(?:\(([\w$.\-*/ ]*)\))?: (.*)$'
          headerPatternCorrespondence: type, scope, subject
          wip: true
          validateSingleCommit: true
          validateSingleCommitMatchesPrTitle: true
```


## __Draft release (optional)__

https://github.com/release-drafter/release-drafter

