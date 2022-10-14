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
          regex: '^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)\/AB\#[0-9]{5}\-[a-zA-Z0-9-/]+$' # https://learn.microsoft.com/en-us/azure/devops/user-guide/service-limits?view=azure-devops#work-items
          allowed_prefixes: 'feat,fix,docs,style,refactor,perf,test,build,ci,chore,revert'
          ignore: master,staging,develop
          min_length: 5
          max_length: 20
```

```
pattern = '^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)\/AB\#[0-9]{5}\-[a-zA-Z0-9-/]+$'



Valid strings:

feat/AB-bug-title#12512-branch-name-validation
feat/AB-asd#12322-validation/something
fix/AB-ahjbsdhkfj-#12313-jasknd
fix/AB-ahjbs--dhkfj#12313-jasknd-asd--asd-a-s-sd



Invalid strings:

chore/AB-bug/title#123-branch_name_validation
update/AB-#12322-validation/something
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
          configuration-path: .github/pr-labeler.yml
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### __`.github/pr-labeler.yml`__

```yml
feat: feat/*
fix: fix/*
docs: docs/*
style: style/*
refactor: refactor/*
perf: perf/*
test: test/*
build: build/*
ci: ci/*
chore: chore/*
revert: revert/*
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

