# `@nicoloboschi/cleanup-astradb-env` Github action

This action deletes [DataStax AstraDB](https://www.datastax.com/products/datastax-astra) databases that have not been used for some time.

Related actions:

- `@nicoloboschi/setup-astradb` [action](https://github.com/nicoloboschi/setup-astradb): Creates a AstraDB database.
- `@nicoloboschi/cleanup-astradb` [action](https://github.com/nicoloboschi/cleanup-astradb): Deletes a specific AstraDB
  database.

## Action Inputs

| Input name        | Description                                                                               	                     | Required 	 | Default Value |
|-------------------|-----------------------------------------------------------------------------------------------------------------|------------|---------------|
| token             | Astra DB application token. It needs to have enough permissions to create/delete databases in the organization. | true       |               |
| threshold-seconds | Threshold in seconds to delete databases based on their last usage time.                                        | false      | 3600 (1 hour) |
| wait              | Whether to wait for the database to be deleted or not.                                                          | false      | true          |
| env               | Astra DB environment. (ENV, PROD).                                                                              | false      | PROD          |

## Example

```yml
name: Clean Astra env

on:
  workflow_dispatch: { }
  schedule:
    - cron: "*/5 * * * *"

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
      - name: Clean AstraDB databases that have not been used in the last hour
        uses: nicoloboschi/cleanup-astradb-env@v1
        with:
          token: ${{ secrets.ASTRA_DB_TOKEN }}
```