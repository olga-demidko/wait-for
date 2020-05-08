# Nexploit Scan Runner

This action runs a new scan in Nexploit, or reruns an existing one.

## Inputs

### `api_token`

**Required** Api Token. You can generate it in *Organization* section

### `scan`

Scan ID to restart.

## Example usage

Start a new scan with parameters

```yml
steps:
    - name: Start Nexploit Scan
      id: start
      uses: NeuraLegion/run-scan@v0.2
      with:
        api_token: ${{ secrets.NEXPLOIT_TOKEN }}
        name: GitHub scan ${{ github.sha }}
        discovery_types: |
          [ "crawler", "archive" ]
        crawler_urls: |
          [ "http://vulnerable-bank.com" ]
        file_id: LiYknMYSdbSZbqgMaC9Sj
        hosts_filter: |
          [ ]
        wait_for: on_any
    - name: Get the output scan url
      run: echo "The scan was started on ${{ steps.start.outputs.url }}"
```

Restart an existing scan

```yml
steps:
    - name: Start Nexploit Scan
      id: start
      uses: NeuraLegion/run-scan@v0.2
      with:
        api_token: ${{ secrets.NEXPLOIT_TOKEN }}
        name: GitHub scan ${{ github.sha }}
        restart_scan: ai3LG8DmVn9Rn1YeqCNRGQ
        wait_for: on_any
    - name: Get the output scan url
      run: echo "The scan was started on ${{ steps.start.outputs.url }}"
```
