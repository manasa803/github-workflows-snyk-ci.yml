# github-workflows-snyk-ci.yml

name: CI with Snyk Security Scan

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  snyk-security:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Install Snyk CLI
        run: npm install -g snyk

      - name: Authenticate Snyk
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
        run: snyk auth $SNYK_TOKEN

      - name: Run Snyk vulnerability scan
        run: snyk test

      - name: Monitor project in Snyk
        run: snyk monitor
