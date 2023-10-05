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

### Prerequisites 

Make sure your website is deployed as CodeAuditor only scans what the HTML browsers see not the code developers write

# CodeAuditor Feedback Loop Workflow

This workflow action runs CodeAuditor scan on your website and creates new GitHub issue if it sees an increase in the number of broken links or code warnings/errors found from CodeAuditor scan

## Inputs

| name         | required | type  | description |
| ------------ | ---      | ------ | ----------- |
| GitHub_Token        | yes      | string | Your GitHub personal access token used to fetch data. Pass a secret by for instance using `${{ secrets.GH_TOKEN }}`. [Go here](https://github.com/settings/tokens/new?scopes=read:user) to generate one
| token     | yes      | string | Your personal CodeAuditor token that can be found on CodeAuditor's How It Works page
| url       | yes      | string | The url used on your CodeAuditor scan

## Job Summaries

Upon completion of the workflow, user will be able to view a the scan summary as part of the workflow output.

<img width="434" alt="image" src="https://github.com/tombui99/codeauditor-github-workflow/assets/67776356/bbf76296-7b0e-4c78-90f5-3947d8ee8994">

**Figure: CodeAuditor Workflow Job Summaries**

## Example usage

Check out [`test.yml`](./.github/workflows/test.yml) for an example workflow.

Workflow:

```yml
name: Test CodeAuditor Workflow

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: CodeAuditor Feedback Loop Workflow
        uses: tombui99/codeauditor-github-workflow@v1.0.0
        with:
          # Your CodeAuditor token
          token: ${{ vars.CODEAUDITORTOKEN }}
          # Your Scan URL
          url: ${{ vars.SCANURL }}
          # Your GitHub Token
          GitHub_Token: ${{secrets.GH_TOKEN}}
```
