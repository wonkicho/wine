# Tutorial for CI/CD using github actions

# Wine quality prediction
Modelling a Kaggle dataset of [red wine properties and quality ratings](https://www.kaggle.com/uciml/red-wine-quality-cortez-et-al-2009). 



## Cml yaml template
```
name: your-workflow-name
on: [push]
jobs:
  run:
    runs-on: ubuntu-latest
    # optionally use a convenient Ubuntu LTS + DVC + CML image
    # container: docker://ghcr.io/iterative/cml:0-dvc2-base1
    steps:
      - uses: actions/checkout@v2
      # may need to setup NodeJS & Python3 on e.g. self-hosted
      # - uses: actions/setup-node@v2
      #   with:
      #     node-version: '16'
      # - uses: actions/setup-python@v2
      #   with:
      #     python-version: '3.x'
      - uses: iterative/setup-cml@v1
      - name: Train model
        run: |
          # Your ML workflow goes here
          pip install -r requirements.txt
          python train.py
      - name: Write CML report
        env:
          REPO_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          # Post reports as comments in GitHub PRs
          cat results.txt >> report.md
          cml-send-comment report.md
```

## Process

### 1. create yaml file



- reference

https://www.youtube.com/watch?v=9BgIDqAzfuA&list=PL7WG7YrwYcnDBDuCkFbcyjnZQrdskFsBz&index=1

https://www.youtube.com/watch?v=9I8X-3HIErc&t=144s
