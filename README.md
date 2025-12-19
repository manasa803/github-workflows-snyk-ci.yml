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
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install || true

      - name: Install Snyk
        run: npm install -g snyk

      - name: Authenticate Snyk
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
        run: snyk auth $SNYK_TOKEN

      - name: Run Snyk scan
        run: snyk test || true

      - name: Upload results to Snyk
        run: snyk monitor || true

        
        
