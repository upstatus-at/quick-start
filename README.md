# Upstatus - Kubernetes Monitoring Solution

This repository contains a Helm chart for deploying Upstatus, a Kubernetes monitoring and error analysis solution. Upstatus consists of two main components:

- **Manager**: Central component that processes logs and provides the monitoring dashboard
- **Agent**: DaemonSet that runs on each node to collect logs and metrics

## Prerequisites

- Kubernetes 1.16+
- Helm 3.0+
- An API key for one of the supported LLM providers (OpenAI, Claude, or Llama)

## Installation

### Quick Start

To install the Upstatus chart with default configuration:

```bash
# Create the upstatus-system namespace first
kubectl create namespace upstatus-system

# Install Upstatus in the upstatus-system namespace
helm install upstatus ./charts/upstatus --namespace upstatus-system
```

Note: By default, Upstatus will be configured to watch resources in the `upstatus-system` namespace.

### Installing with an LLM API Key

You can provide your LLM API key during installation using the `--set` flag. Upstatus supports three LLM providers: OpenAI, Claude, and Llama.

#### Using OpenAI

```bash
# Create the upstatus-system namespace first
kubectl create namespace upstatus-system

# Install with OpenAI as the provider
helm install upstatus ./charts/upstatus \
  --namespace upstatus-system \
  --set manager.llm.provider=openai \
  --set manager.llm.apiKey=your-openai-api-key
```

#### Using Claude

```bash
# Create the upstatus-system namespace first
kubectl create namespace upstatus-system

# Install with Claude as the provider
helm install upstatus ./charts/upstatus \
  --namespace upstatus-system \
  --set manager.llm.provider=claude \
  --set manager.llm.apiKey=your-claude-api-key
```

#### Using Llama

```bash
# Create the upstatus-system namespace first
kubectl create namespace upstatus-system

# Install with Llama as the provider
helm install upstatus ./charts/upstatus \
  --namespace upstatus-system \
  --set manager.llm.provider=llama \
  --set manager.llm.apiKey=your-llama-api-key
```

### Using an Existing Secret for the LLM API Key

For better security, you can create a Kubernetes secret containing your LLM API key and reference it in the Helm installation:

1. Create a secret with your LLM API key:

```bash
# Create the upstatus-system namespace first
kubectl create namespace upstatus-system

# Create the secret in the same namespace
kubectl create secret generic llm-api-key \
  --namespace upstatus-system \
  --from-literal=api-key=your-llm-api-key
```

2. Install the chart referencing the existing secret:

```bash
helm install upstatus ./charts/upstatus \
  --namespace upstatus-system \
  --set manager.llm.provider=openai \
  --set manager.llm.existingSecret=llm-api-key \
  --set manager.llm.existingSecretKey=api-key
```

Replace `openai` with `claude` or `llama` depending on which provider you want to use.

## Uninstalling the Chart

To uninstall/delete the `upstatus` deployment:

```bash
helm uninstall upstatus -n upstatus-system
```

## Troubleshooting

### Common Issues

1. **OpenAI API Key Issues**: If you encounter errors related to the OpenAI API, verify your API key is correctly set and has sufficient quota.

2. **Agent Not Collecting Logs**: Check that the agent has the necessary permissions to access logs on the node. You may need to adjust the security context.

3. **Manager Not Starting**: Ensure the manager has the correct RBAC permissions to watch the specified namespace.

### Viewing Logs

To view logs from the manager:

```bash
kubectl logs -l app.kubernetes.io/name=upstatus-manager -n upstatus-system
```

To view logs from an agent:

```bash
kubectl logs -l app.kubernetes.io/name=upstatus-agent -n upstatus-system
```