# publicthumbs-
.github/workflows/
name: Daily Thumbnail Generator

on:
  schedule:
    - cron: '0 8 * * *'  # daily at 08:00 UTC
  workflow_dispatch: # allows manual trigger

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: actions/setup-node@v3
      with:
        node-version: '18'
    - run: npm install
    - run: node index.js
    - run: |
        git config --local user.name "github-actions[bot]"
        git config --local user.email "github-actions[bot]@users.noreply.github.com"
        git add public/thumbs
        git commit -m "Daily thumbnail generated $(date +'%Y-%m-%d')" || echo "No changes to commit"
        git push
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
