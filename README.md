# platform-pipeline-templates

GitHub Actions reusable workflows for the local Kind lab:

- `.github/workflows/platform-default.yml`: validate, npm ci/test, npm audit,
  Docker build/publish, then the reusable GitOps workflow for TI.
- `.github/workflows/platform-gitops.yml`: verify the published image, update
  one environment YAML with PyYAML, validate Helm, commit/push deployment_files,
  and wait for Argo CD to reconcile the exact commit and finish the rollout.
  HML requires the tag from healthy TI; PROD requires the tag from healthy HML.
  Promotions never build or publish images.

Call workflows using a pinned commit SHA. Inputs: `image_name` and
`application_prefix`; GitOps additionally requires `environment` and `image_tag`.
The consumer grants `contents: write` to the reusable workflow. It uses the
automatic GITHUB_TOKEN; no PAT or SSH private key is stored in the workflow.

Runner prerequisites: Linux x64, labels `self-hosted,linux,x64,platform-lab`,
Docker socket access, Git, curl, Python 3 with PyYAML, Helm, kubectl and the
`kind-platform-lab` kubeconfig. Node 22 is provided by setup-node.
Registry URLs are `localhost:5000` for publishing and `platform-registry:5000`
for Kubernetes. SHA tags already present are reused instead of overwritten.

Only trusted main-branch push/workflow_dispatch events can use this local runner.
There is no pull_request or pull_request_target trigger. GitOps branch pushes
do not trigger CI. GitOps writes are serialized across CI and promotion jobs.
A concurrent external Git push fails safely instead of overwriting remote work.

Existing GitLab templates remain as corporate reference material and are not
the execution path of this lab.
