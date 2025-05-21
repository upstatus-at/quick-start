# Upstatus Helm Repository

This branch hosts the Helm repository for Upstatus, a Kubernetes monitoring and error analysis solution. The repository is served via GitHub Pages.

## Using the Helm Repository

### Add the Repository

To add this Helm repository to your Helm client:

```bash
helm repo add upstatus https://upstatus-at.github.io/quick-start/repo
helm repo update
```

### Available Charts

The following Helm charts are available in this repository:

- **upstatus**: A Kubernetes monitoring and error analysis solution

### Installing Charts

To install the Upstatus chart:

```bash
# Create the upstatus-system namespace first
kubectl create namespace upstatus-system

# Install Upstatus in the upstatus-system namespace
helm install upstatus upstatus/upstatus --namespace upstatus-system
```

For detailed configuration options and usage instructions, please refer to the main branch of this repository.

## Repository Structure

This branch is structured as follows:

- `/repo/`: Contains the packaged Helm charts and the repository index
  - `index.yaml`: The Helm repository index file
  - `upstatus-x.y.z.tgz`: Packaged Helm chart(s)
- `index.html`: Landing page for the GitHub Pages site

## Updating the Repository

To update this Helm repository with a new chart version:

1. Switch to the main branch
2. Update your chart and increment the version in `Chart.yaml`
3. Package the updated chart: `helm package charts/upstatus -d /tmp`
4. Switch to the gh-pages branch
5. Move the packaged chart to the repo directory: `mv /tmp/upstatus-x.y.z.tgz repo/`
6. Update the repository index: `helm repo index repo --url https://upstatus-at.github.io/quick-start/repo`
7. Commit and push the changes to the gh-pages branch

## Documentation

For more detailed information about the Upstatus Helm chart, including configuration options and usage examples, please refer to the main branch of this repository.

