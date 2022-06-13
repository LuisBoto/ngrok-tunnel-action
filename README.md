# Ngrok tunnel Github Action

This action is based on the code found on [this repository](https://github.com/apogiatzis/ngrok-tunneling-action).

This is a Github Action that can be used in your Github Workflow to tunnel incoming/outgoing TCP or HTTP traffic in your workflow environment, achieving temporary deployment for testing or other purposes.

## How to use

This action accepts the following parameters:

| Name| Description | Required  | Default |
| ------------- |-------------|-----|-----|
| timeout | After this timeout the deployment will automatically shutdown the tunelling and therefore stop the action. (max is 6 hours) | No | 1h |
| port | The port in localhost to forward traffic from/to  | Yes | - |
| ngrok_authtoken | Your ngrok authtoken| Yes | - |
| tunnel_type | Whether the tunnel type will be TCP or HTTP | Yes | tcp |
| save_url_to_filename | If provided, save the deployed tunnel's URL to a file with the provided name, otherwise just print URL on console | No | - |


Here is an example of using this action:

```yaml
name: CI
on: push

jobs:

  deploy:
    name: Docker container with Ngrok tunnel
    runs-on: ubuntu-latest
    needs: cancel

    steps:
    - name: Checkout
      uses: actions/checkout@v2
    
    - name: Run container
      run: docker-compose up -d 
    
    - uses: luisboto/ngrok-tunnel-action@<VERSION>
      with:
        timeout: 1h
        port: 8080
        ngrok_authtoken: ${{ secrets.NGROK_AUTHTOKEN }}
        tunnel_type: http
        save_url_to_filename: tunnelURL.md
```
