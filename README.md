# Test Setup

- Install [kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
- Create a cluster `kind create cluster`.
- Clone [agent repository](https://github.bhs-world.com/iCorrOperationsSupport/icorr-edge-deployment-agent) with branch "fix-deployment-engine-interaction" `git clone -b fix-deployment-engine-interaction git@github.bhs-world.com:iCorrOperationsSupport/icorr-edge-deployment-agent.git`
- Build docker image `docker build -t container.artifact.bhs-world.com/edge-artifacts/icorr-edge-deployment-agent:0.1.2 .`
- Load image into kind cluster `kind load docker-image container.artifact.bhs-world.com/edge-artifacts/icorr-edge-deployment-agent:0.1.2`
- Deploy sealed secrets and edge-agent `kubectl apply -k tests/integration_blackbox/kubernetes_deployments/`
- Wait for the agent to be ready `kubectl get pods -n edge-agent`
- Copy the edge-agent pod name `kubectl get pods -n edge-agent | grep edge | awk '{print $1}'`
- Run the registration command `kubectl exec -it <edge-agent-pod-name> -n edge-agent -- curl -s -o /dev/null --unix-socket /tmp/edge-agent.sock -X POST "http://localhost/init?token=<registration-token>"`
