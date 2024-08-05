<!-- start title -->

# GitHub Action:Run Pulumi Command

<!-- end title -->
<!-- start description -->

Runs pulumi command on specified stack. Currently supports pulumi in golang and AWS, may support other languages and providers in the future. If the runtime is go, then go modules are cached.

<!-- end description -->
<!-- start contents -->
<!-- end contents -->
<!-- start usage -->

```yaml
- uses: catalystcommunity/action-pulumi@undefined
  with:
    # Pulumi runtime
    # Default: go
    runtime: ""

    # Go version to use, this input is only used if the runtime is set to `go`
    # Default: ~1.17
    go-version: ""

    # Cloud provider to get credentials for
    # Default: aws
    provider: ""

    # AWS region
    # Default: us-west-2
    aws-region: ""

    # AWS access key id to use
    # Default:
    aws-access-key-id: ""

    # AWS secret access key
    # Default:
    aws-secret-access-key: ""

    # Pulumi command to run, one of `up`, `refresh`, `destroy`, `preview`
    command: ""

    # Which stack you want to apply to, eg. prod
    stack-name: ""

    # Location of your Pulumi files. Defaults to ./
    # Default: ./
    work-dir: ""

    # If true, a comment will be created with results
    # Default: true
    comment-on-pr: ""

    # Github Token
    # Default: ${{ github.token }}
    github-token: ""

    # A cloud URL to log in to
    # Default:
    cloud-url: ""

    # The type of the provider that should be used to encrypt and decrypt secrets.
    # Possible choices: default, passphrase, awskms, azurekeyvault, gcpkms, hashivault
    # Default:
    secrets-provider: ""

    # Allow P resource operations to run in parallel at once (1 for no parallelism).
    # Defaults to unbounded.
    # Default: 2147483647
    parallel: ""

    # Optional message to associate with the update operation
    # Default:
    message: ""

    # Return an error if any changes occur during this update
    expect-no-changes: ""

    # Display operation as a rich diff showing the overall change
    # Default: true
    diff: ""

    # Specify resources to replace. Multiple resources can be specified one per line
    replace: ""

    # Specify a single resource URN to update. Other resources will not be updated.
    # Multiple resources can be specified one per line
    target: ""

    # Allows updating of dependent targets discovered but not specified in target.
    # Default: false
    target-dependents: ""

    # Perform a stack refresh as part of the operation
    # Default: true
    refresh: ""

    # Create the stack if it currently does not exist
    # Default: false
    upsert: ""

    # Edit previous PR comment instead of posting new one
    # Default: true
    edit-pr-comment: ""
```

<!-- end usage -->
<!-- start inputs -->

| **Input**                   | **Description**                                                                                                                                               |      **Default**      | **Required** |
| :-------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------ | :-------------------: | :----------: |
| **`runtime`**               | Pulumi runtime                                                                                                                                                |         `go`          |  **false**   |
| **`go-version`**            | Go version to use, this input is only used if the runtime is set to `go`                                                                                      |        `~1.17`        |  **false**   |
| **`provider`**              | Cloud provider to get credentials for                                                                                                                         |         `aws`         |  **false**   |
| **`aws-region`**            | AWS region                                                                                                                                                    |      `us-west-2`      |  **false**   |
| **`aws-access-key-id`**     | AWS access key id to use                                                                                                                                      |                       |  **false**   |
| **`aws-secret-access-key`** | AWS secret access key                                                                                                                                         |                       |  **false**   |
| **`command`**               | Pulumi command to run, one of `up`, `refresh`, `destroy`, `preview`                                                                                           |                       |   **true**   |
| **`stack-name`**            | Which stack you want to apply to, eg. prod                                                                                                                    |                       |   **true**   |
| **`work-dir`**              | Location of your Pulumi files. Defaults to ./                                                                                                                 |         `./`          |  **false**   |
| **`comment-on-pr`**         | If true, a comment will be created with results                                                                                                               |        `true`         |  **false**   |
| **`github-token`**          | Github Token                                                                                                                                                  | `${{ github.token }}` |  **false**   |
| **`cloud-url`**             | A cloud URL to log in to                                                                                                                                      |                       |  **false**   |
| **`secrets-provider`**      | The type of the provider that should be used to encrypt and decrypt secrets. Possible choices: default, passphrase, awskms, azurekeyvault, gcpkms, hashivault |                       |  **false**   |
| **`parallel`**              | Allow P resource operations to run in parallel at once (1 for no parallelism). Defaults to unbounded.                                                         |     `2147483647`      |  **false**   |
| **`message`**               | Optional message to associate with the update operation                                                                                                       |                       |  **false**   |
| **`expect-no-changes`**     | Return an error if any changes occur during this update                                                                                                       |                       |  **false**   |
| **`diff`**                  | Display operation as a rich diff showing the overall change                                                                                                   |        `true`         |  **false**   |
| **`replace`**               | Specify resources to replace. Multiple resources can be specified one per line                                                                                |                       |  **false**   |
| **`target`**                | Specify a single resource URN to update. Other resources will not be updated. Multiple resources can be specified one per line                                |                       |  **false**   |
| **`target-dependents`**     | Allows updating of dependent targets discovered but not specified in target.                                                                                  |        `false`        |  **false**   |
| **`refresh`**               | Perform a stack refresh as part of the operation                                                                                                              |        `true`         |  **false**   |
| **`upsert`**                | Create the stack if it currently does not exist                                                                                                               |        `false`        |  **false**   |
| **`edit-pr-comment`**       | Edit previous PR comment instead of posting new one                                                                                                           |        `true`         |  **false**   |

<!-- end inputs -->
<!-- start outputs -->

| **Output**       | **Description**          | **Default** | **Required** |
| :--------------- | :----------------------- | ----------- | ------------ |
| `command-output` | Output of pulumi command |             |              |

<!-- end outputs -->
<!-- start examples -->

### Example usage

```yaml
name: Deploy via Pulumi
on: # only trigger on prs to main closed
  pull_request:
    types:
      - closed
    branches:
      - main
jobs:
  deploy:
    if: github.event.pull_request.merged == true # only deploy on merged PRs
    strategy:
      matrix:
        stack-name: [common, nonprod, prod]
    name: Deploy ${{ matrix.stack-name }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        uses: catalystcommunity/action-pulumi@v1
        with:
          command: up
          stack-name: ${{ matrix.stack-name }}
          aws-access-key-id: ${{ secrets.PULUMI_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.PULUMI_AWS_SECRET_ACCESS_KEY }}
          cloud-url: ${{ secrets.PULUMI_BACKEND_URL_PLATFORM }}
```

<!-- end examples -->
<!-- start [.github/ghdocs/examples/] -->
<!-- end [.github/ghdocs/examples/] -->
