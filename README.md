# Push Then PR to Other Repo

If you need to centralize something to one repository, you can use this action. 

## Implement

* Generate new [token](https://github.com/settings/tokens/new). Select `repo` scope.
* In your_source_repo/settings/secret , add `API_TOKEN_GITHUB` with your generated token.
* In source repo, put `push_and_pr.yml` in `.github/workflow` with this code:

```yaml
# This CI will run after publish a new release

name: Push to Other Repo and Make a PR

on:
  release:
    types: [published]
  
jobs:
  processing_docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Push then create PR
        uses: sepulsa/push_then_pr@master
        env:
            API_TOKEN_GITHUB: ${{ secrets.API_TOKEN_GITHUB }}

            DEST_GITHUB_USERNAME: 'sepulsa'
            DEST_REPO_NAME: 'knowledgebase'
            USER_EMAIL: 'test@example.com'
            PUSH_TO_BRANCH: 'docs'
            PR_TO_BRANCH: 'master'
            # You can add multiple source file or folder separate by space
            SRC_DIR: 'docs readme.adoc' 
            # You can create nested folder
            DEST_DIR: 'initiative/system_name'
            # You can rename with format source(comma)target
            # Multiple line if you want to move multiple file / folder
            RENAME: >-
                    readme.adoc,docs.adoc
                    docs,docs
            # Add your custom Pull Request message
            PR_MESSAGE: 'New release from system_name'
```

* Create a release and see your action!

## Example

* Source repo: https://github.com/tegarimansyah/Jupyter
* Destination repo: https://github.com/tegarimansyah/destination_repo
