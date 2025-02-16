# Terraform google buckets module with semantic versioning

This repository contains reusable Terraform modules designed for managing bucket resources on GCP. This project shows how you can configure semantic versioning for such module.

## Step-by-setup semantic versioning on a repo:

1. Create `.github/workflows/pipeline.yml` file
   ```
   name: Release
   on:
     push:
       branches:
         - main
         - feat/add_tf_module_with_semvar

   permissions:
     contents: read # for checkout

   jobs:
     release:
       name: Release
       runs-on: ubuntu-latest
       permissions:
         contents: write # to be able to publish a GitHub release
         issues: write # to be able to comment on released issues
         pull-requests: write # to be able to comment on released pull requests
         id-token: write # to enable use of OIDC for npm provenance
       steps:
         - name: Checkout
           uses: actions/checkout@v4
           with:
             fetch-depth: 0
             persist-credentials: false
         - name: Semantic Release
           id: semantic-release
           uses: cycjimmy/semantic-release-action@v4
           env:
             GITHUB_TOKEN: ${{ secrets.GH_TOKEN }}
   ```
2. Create `.releaserc` file at root location
    ```
      {
          "plugins": [
              "@semantic-release/commit-analyzer",
              "@semantic-release/release-notes-generator",
              [
                  "@semantic-release/changelog",
                  {
                      "changelogFile": "CHANGELOG.md"
                  }
              ],
              [
                  "@semantic-release/git",
                  {
                      "assets": [
                          "CHANGELOG.md"
                      ]
                  }
              ],
              "@semantic-release/github"

          ]
      }
    ```
3. You will need a [Github Personal Access Token](https://github.com/settings/personal-access-tokens/new) and store it as environment variable via `Settings > CI/CD > Actions variables > Secret`

4. Make and pull request and merge to Main or Master branch this will automatically publish a semantic release version tag for the commit.


## References

1. [Semantic Versioning official documentation](https://semantic-release.gitbook.io/semantic-release)
2. [Semantic versioning on Github](https://github.com/marketplace/actions/action-for-semantic-release)
