# Wait for Nexploit Scan to Find Something

This action waits until Nexploit finds an issue, or until timeout.

## Inputs

### `api_token`

**Required** Api Token. You can generate it in *Organization* section

### `scan`

Scan ID to wait.

### `wait_for`

Wait for first issue: *any*, *medium*, *high*

Example:

```yml
wait_for: any
```

### `timeout`

Time in seconds for the action to wait for issues

## Outputs

### `url`

Url of the resulting scan

## Example usage

```yml
start_and_wait_scan:
  runs-on: ubuntu-latest
  name: A job to run a Nexploit scan
  steps:
  - name: Start Nexploit Scan 🏁
    id: start
    uses: NeuraLegion/run-scan@v1
    with:
      api_token: ${{ secrets.NEXPLOIT_TOKEN }}
      name: GitHub scan ${{ github.sha }}
      discovery_types: |
        [ "crawler", "archive" ]
      crawler_urls: |
        [ "https://juice-shop.herokuapp.com" ]
      file_id: LiYknMYSdbSZbqgMaC9Sj
      hosts_filter: |
        [ ]
      wait_for: on_high
  - name: Get the output scan url
    run: echo "The scan was started on ${{ steps.start.outputs.url }}"
  - name: Wait for any issues ⏳
    id: wait
    uses: NeuraLegion/wait-for@v1
    with:
      api_token: ${{ secrets.NEXPLOIT_TOKEN }}
      scan: ${{ steps.start.outputs.id }}
      wait_for: any
      timeout: 55
```
