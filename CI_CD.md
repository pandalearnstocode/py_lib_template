```yml
name: 'Assert Branch Naming Convention'
on: 
  pull_request:
      types:
        - opened
        - edited
        - synchronize
jobs:
  branch-naming-rules:
    runs-on: ubuntu-latest
    steps:
      - uses: deepakputhraya/action-branch-name@master
        with:
          regex: '^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)\/AB\#[0-9]{5}\-[a-zA-Z0-9-/]+$'
          allowed_prefixes: 'feat,fix,docs,style,refactor,perf,test,build,ci,chore,revert'
          ignore: master,staging,develop
          min_length: 5
          max_length: 50
      - uses: TimonVS/pr-labeler-action@v3
        with:
          configuration-path: .github/pr-labeler.yml
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      - uses: amannn/action-semantic-pull-request@v5
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          types: |
            feat
            fix
            docs
            style
            refactor
            perf
            test
            build
            ci
            chore
            revert
          requireScope: false
          subjectPattern: ^AB\#[0-9]{5}\-[a-zA-Z0-9-/]+$
          subjectPatternError: |
            The subject "{subject}" found in the pull request title "{title}"
            didn't match the configured pattern. Please ensure that the subject
            doesn't start with an uppercase character.
          githubBaseUrl: https://github.myorg.com/api/v3
          ignoreLabels: |
            bot
            ignore-semantic-pull-request
          headerPattern: '^(\w*): (.*)$'
          headerPatternCorrespondence: type, subject
          wip: true
          validateSingleCommit: true
          validateSingleCommitMatchesPrTitle: true
```

__Examples:__

* branch name: `feat/AB#-12345-develop-poisson-model`
* pr label: `feat`
* pr title: `feat(ui): AB#-12345 develop poisson model`
* commit message: `feat(ui): AB#-12345 develop poisson model`
`type`: `feat`, `scope`: `ui`, `subject`: `AB#-12345 develop poisson model`

feat: AB#-12345 develop poisson model #22
fix: AB#-12346 develop poisson model #21



* feat(ui): AB#-12345 develop poisson model
* feat(ci): AB#-12345 develop poisson model
* feat(cd): AB#-12345 develop poisson model


* fix(ui): AB#-12345 develop poisson model
* fix(ci): AB#-12345 develop poisson model
* fix(cd): AB#-12345 develop poisson model