name: Open Pull Request

on: 
  branch:
    
  
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
  
    - run: git config --global user.email "$(git --no-pager log --format=format:'%ae' -n 1)"
    - run: git config --global user.name "$(git --no-pager log --format=format:'%an' -n 1)"    
    - run: git fetch origin
    - run: git checkout -b workflow-branch
    - run: git status
    - run: git branch
    - run: date > test-reports/workflow-run-date.txt
    - run: git add test-reports/workflow-run-date.txt
    - run: git commit -m "Run Date Added"
#   - run: git pull origin workflow-branch
    - run: git push "https://${{github.actor}}:${{secrets.ghtoken}}@github.com/${{github.repository}}" workflow-branch
#   - run: git request-pull v4.0 "https://${{github.actor}}:${{secrets.ghtoken}}@github.com/${{github.repository}}" workflow-branch

    - uses: mattdavis0351/actions/open-pull-request@master
      with:
        headBranch: workflow-branch
        baseBranch: master
        apiToken: ${{ secrets.ghtoken }}
        pullTitle: "I really need to merge these changes"
        pullBody: "I will show up in the body of the pull request"
