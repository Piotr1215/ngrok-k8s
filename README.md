# Expose local Kubernetes NodePort service on internet using ngrok

1. Set alias for `kubectl` to easier run commands:

    ``alias k=kubectl``

2. Create nginx pod in your local cluster

    `k run nginx --image nginx --restart Never`

3. Expose nginx pod via a NodePort service

    `k expose pod nginx --port 80 --target-port 80 --type NodePort --name nginx-service`

4. Create variable with the node port of the service

    `NODE_PORT=$(k get svc nginx-service -o=jsonpath="{$.spec.ports[0].nodePort}{'\n'}")`

5. Use curl to check if nginx is available on the port of the host

- If you are running kubernetes in **minikube**:

  `curl http://{minikube ip}:$NODE_PORT`

  Hint: you can check minikube ip by running `mikikube ip`

- If you are running local kubernetes installation that supports localhost, just type

`curl http://localhost:$NODE_PORT`

6. Expose your service on the internet using ngrok

- If you are running kubernetes in **minikube**:

  ``ngrok http http://{minikube ip}:$NODE_PORT``

- If you are running local kubernetes installation that supports localhost, just type

`ngrok http http://localhost:$NODE_PORT`