# Tutorial for CI/CD using github actions (Wine quality prediction)
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

### 1. create yaml file in .github/workflows and commit (make a brench).

```
name: model-wine-quality
on: [push]
jobs:
  run:
    runs-on: [ubuntu-latest]
    container: docker://dvcorg/cml-py3:latest
    steps:
      - uses: actions/checkout@v2
      - name: cml_run
        env:
          repo_token: ${{ secrets.GITHUB_TOKEN }}
        run: |
          # Your ML workflow goes here
          pip install -r requirements.txt
          python train.py
          
          echo "## Model metrics" > report.md
          cat metrics.txt >> report.md
          
          echo "## Data viz" >> report.md
          cml-publish feature_importance.png --md >> report.md
          cml-publish residuals.png --md >> report.md
          
          cml-send-comment report.md
```

### 2. Check your commit in pull & requests

![image](https://user-images.githubusercontent.com/53808221/147482367-d74aed95-9a46-4138-bec7-c0d62be432cc.png)


### 3. Check your results in Actions.

![image](https://user-images.githubusercontent.com/53808221/147482485-c85f1dda-2fc2-471b-9c68-a491ddbc33e9.png)

### 4. Try to change your train code and re-commit.

ex) depth 2 -> 4 in train.py

![image](https://user-images.githubusercontent.com/53808221/147482399-69668c9c-3486-490d-bc5e-c7350bc401c2.png)

### 5. Merge

if you sure this code is better than previous code, merge this code in master branch.


- reference

https://www.youtube.com/watch?v=9BgIDqAzfuA&list=PL7WG7YrwYcnDBDuCkFbcyjnZQrdskFsBz&index=1

https://www.youtube.com/watch?v=9I8X-3HIErc&t=144s
