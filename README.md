# `@nicoloboschi/cleanup-astradb-env` Github action

This action deletes [DataStax AstraDB](https://www.datastax.com/products/datastax-astra) unused databases.
To create a database, use the `@nicoloboschi/setup-astradb` [action](https://github.com/nicoloboschi/setup-astradb).

## Action Inputs

| Input name        | Description                                                                               	                     | Required 	 | Default Value |
|-------------------|-----------------------------------------------------------------------------------------------------------------|------------|---------------|
| token             | Astra DB application token. It needs to have enough permissions to create/delete databases in the organization. | true       |               |
| name              | Name of the database to create.                                                                                 | true       |               |
| threshold-seconds | Threshold in seconds to delete databases based on their last usage time.                                        | true       |               |
| wait              | Whether to wait for the database to be deleted or not.                                                          | true       |               |
| env               | Astra DB environment. (ENV, PROD).                                                                              | false      | PROD          |

## Example

```yml
name: Clean Astra env

on:
  workflow_dispatch: {}
  schedule:
    - cron: "*/5 * * * *"

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
      - name: Clean AstraDB databases older than 1 day
        uses: nicoloboschi/cleanup-astradb-env@v1
        with:
          token: ${{ secrets.ASTRA_DB_TOKEN }}
```