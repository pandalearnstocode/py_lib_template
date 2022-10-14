
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
          subjectPattern: ^AB\#[0-9]{5}.*$
          subjectPatternError: |
            The subject "{subject}" found in the pull request title "{title}"
            didn't match the configured pattern. Please ensure that the subject
            doesn't start with an uppercase character.
          ignoreLabels: |
            bot
            ignore-semantic-pull-request
          headerPattern: '^(\w*): (.*)$'
          headerPatternCorrespondence: type, subject
          wip: true
          validateSingleCommit: true
          validateSingleCommitMatchesPrTitle: true
      - name: Python setup
        run: echo "Python setup"
      - name: Install Poetry and system dependencies
        run: echo "Install Poetry and system dependencies"
      - name: Install Package dependencies
        run: echo "Install Package dependencies"
      - name: Setup cache
        run: echo "Setup cache"
      - name: Formatting
        run: echo "Formatting"
      - name: Static typing
        run: echo "Static typing"
      - name: Linting
        run: echo "Linting"
      - name: safety, bandit
        run: echo "safety, bandit"
      - name: gitleaks, checkmarx
        run: echo "gitleaks, checkmarx"
      - name: CodeQL, OSSR
        run: echo "CodeQL, OSSR"
      - name: Synk (we have to check how to setup automated scanning)
        run: echo "Synk (we have to check how to setup automated scanning)"
      - name: Running pytest
        run: echo "Running pytest"
      - name: Generate test coverage
        run: echo "Generate test coverage"
      - name: Perform Sonar Cloud scanning
        run: echo "Perform Sonar Cloud scanning"
      - name: Upload all artifacts as github artifacts
        run: echo "Upload all artifacts as github artifacts"
      - name: Perform CML with sample dvc  (optional)
        run: echo "Perform CML with sample dvc  (optional)"
      - name: Integrate model KPI goodness
        run: echo "Integrate model KPI goodness"
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