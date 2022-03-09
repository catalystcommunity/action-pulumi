<!-- start title -->

# GitHub Action:Hello World

<!-- end title -->
<!-- start description -->

Greet someone

<!-- end description -->
<!-- start contents -->
<!-- end contents -->
<!-- start usage -->

```yaml
- uses: catalystsquad/action-composite-action-template@undefined
  with:
    # Who to greet
    # Default: World
    who-to-greet: ""
```

<!-- end usage -->
<!-- start inputs -->

| **Input**          | **Description** | **Default** | **Required** |
| :----------------- | :-------------- | :---------: | :----------: |
| **`who-to-greet`** | Who to greet    |   `World`   |   **true**   |

<!-- end inputs -->
<!-- start outputs -->

| **Output**      | **Description** | **Default** | **Required** |
| :-------------- | :-------------- | ----------- | ------------ |
| `random-number` | Random number   |             |              |

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
        stack-name: [ common, nonprod, prod ]
    name: Deploy ${{ matrix.stack-name }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        uses: catalystsquad/action-pulumi@v1
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
