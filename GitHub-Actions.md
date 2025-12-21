### GitHub Actions
When developers push code or open a pull request, the pipeline runs automatically.
Developers like it because the pipeline is written in simple YAML and is easy to understand and debug.
Overall, GitHub Actions reduces maintenance work, improves security, and helps teams deliver faster with less effort.
Developers like it because they don’t need to learn a separate CI tool. YAML workflows are simple, reusable actions are available in the marketplace, and troubleshooting is easier
### Just create a file → GitHub executes it.
```
.github/workflows/main.yml
```
### Simple rule: 
   Workflow has Jobs →→ Jobs have Steps →→ Steps run on Runners This is the heart of GitHub Actions.
### Why teams like it

Zero setup (already in GitHub)

Huge marketplace of reusable actions

Great for DevOps / Platform / SRE workflows

Works nicely with Docker, Kubernetes, Terraform
