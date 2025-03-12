# What is CodeAuditor?

CodeAuditor is a tool that automatically scans your website and its code to check
- Find broken Links - Links to pages which do not work
- Check HTML formatting - May cause pages to be incorrectly shown to the user
- Lighthouse scan - Audits for performance, accessibility, SEO, and more
- Artillery load test - See how website behaves when lot of users access it simultaneously

### How does it work?

CodeAuditor runs scans and checks for issues on your website, and can then generate a report which can be viewed online.

CodeAuditor is simple to use and can be either be run manually, or embedded directly into your build pipeline where it can be configured to automatically fail a build based on a number of broken links, SEO issues or other rules failures to ensure quality.

Signing up for free and logging in to CodeAuditor will allow you to view and track your website's changes and improvements over time.

### What are the benefits?

CodeAuditor will automatically pick up and report issues which may exist in your website during the build process which enables you to catch any issues and fix them before they are published and cause bigger problems.

### What are the limitations?

CodeAuditor looks at the HTML browsers see, not the code developers write
Only works on static sites, not Angular SPA, React SPA or Blazor
To make it clear, this is not completely fixable, but we could improve it to make it less painful.

### Managing errors

Managing errors becomes simple with Code Auditor. Check out this Rule on how to manage [errors with Code Auditor](https://www.ssw.com.au/rules/managing-errors-with-code-auditor/).

### Prerequisites 

Make sure your website is deployed as CodeAuditor only scans what the HTML browsers see not the code developers write

# CodeAuditor Feedback Loop Workflow

This workflow action runs CodeAuditor scan on your website and creates new GitHub issue if it sees an increase in the number of broken links or code warnings/errors found from CodeAuditor scan

## Inputs

| name         | required | type  | description |
| ------------ | ---      | ------ | ----------- |
| GitHub_Token        | yes      | string | Your repo default GitHub token i.e. using `${{ github.token }}` 
| | | | Make sure you grant the [token permission](https://docs.github.com/en/actions/using-jobs/assigning-permissions-to-jobs) to create issue
| token     | yes      | string | Your personal CodeAuditor token that can be found on CodeAuditor's How It Works page
| url       | yes      | string | The url used on your CodeAuditor scan
| AlertIssue       | no      | boolean | Set to "true" if you want to switch on issue alert feature
| GoMaxthread       | no      | number | Set the maximum number of threads for Golang web scraping (Default is 100)

## Job Summaries

Upon completion of the workflow, user will be able to view a the scan summary as part of the workflow output.

<img width="434" alt="image" src="https://github.com/tombui99/codeauditor-github-workflow/assets/67776356/bbf76296-7b0e-4c78-90f5-3947d8ee8994">

**Figure: CodeAuditor Workflow Job Summaries**

## Creating Task Items

Upon completion of the workflow, if CodeAuditor finds new broken links or code warnings/errors, it will create a new issue with a report and a task item to fix.

<img width="1297" alt="image" src="https://github.com/SSWConsulting/codeauditor-scan-site/assets/67776356/ee3b96ad-f4fd-4bc7-bcfe-af6b865e2c35">

**Figure: Sample generated issues**

## Example usage

Check out [`test.yml`](./.github/workflows/test.yml) for an example workflow.

Workflow:

```yml
name: Test CodeAuditor Workflow

jobs:
  build:
    runs-on: ubuntu-latest
    permissions: 
      issues: write
    steps:
      - uses: actions/checkout@v3
      - name: CodeAuditor Feedback Loop Workflow
        uses: tombui99/codeauditor-github-workflow@v1.0.0
        with:
          # Your CodeAuditor token
          token: ${{ secrets.CODEAUDITORTOKEN }}
          # Your Scan URL
          url: ${{ vars.SCANURL }}
          # Your GitHub Token
          GitHub_Token: ${{ github.token }}
```
