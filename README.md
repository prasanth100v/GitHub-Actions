# GitHub Actions vocabulary table 👇
| **Term**                 | **Meaning (Simple Words)**             | **Example / Notes**                  |
| ------------------------ | -------------------------------------- | ------------------------------------ |
| **GitHub Actions**       | CI/CD tool built into GitHub           | Automate build, test, deploy         |
| **Workflow**             | A complete automation process          | Defined in `.github/workflows/*.yml` |
| **Workflow File**        | YAML file that defines automation      | `ci.yml`, `deploy.yml`               |
| **Event**                | What triggers a workflow               | `push`, `pull_request`, `schedule`   |
| **Job**                  | A set of steps that run together       | Runs on one runner                   |
| **Step**                 | Individual task inside a job           | `run`, `uses`                        |
| **Action**               | Reusable automation logic              | `actions/checkout`                   |
| **Runner**               | Machine that executes jobs             | GitHub-hosted or self-hosted         |
| **GitHub-hosted Runner** | Runner provided by GitHub              | `ubuntu-latest`                      |
| **Self-hosted Runner**   | Your own machine as runner             | EC2, VM, on-prem                     |
| **YAML**                 | Configuration file format              | Used for workflows                   |
| **uses**                 | Calls an existing action               | `uses: actions/checkout@v4`          |
| **run**                  | Runs shell commands                    | `run: npm install`                   |
| **on**                   | Defines workflow trigger               | `on: push`                           |
| **jobs**                 | Section defining jobs                  | `jobs: build:`                       |
| **steps**                | Section defining steps                 | `steps:`                             |
| **name**                 | Human-readable label                   | Workflow or job name                 |
| **env**                  | Environment variables                  | `env: NODE_ENV=prod`                 |
| **secrets**              | Encrypted sensitive values             | `secrets.AWS_KEY`                    |
| **variables (vars)**     | Non-secret reusable values             | `vars.ENV_NAME`                      |
| **Matrix**               | Run job multiple times with variations | Multiple OS / versions               |
| **strategy.matrix**      | Matrix configuration                   | Node 16, 18, 20                      |
| **needs**                | Job dependency                         | Run job after another                |
| **if**                   | Conditional execution                  | Run only on `main` branch            |
| **timeout-minutes**      | Job timeout                            | Prevent infinite runs                |
| **permissions**          | GitHub token access control            | Least privilege                      |
| **checkout**             | Fetch repo code                        | `actions/checkout`                   |
| **cache**                | Store reusable data                    | Faster builds                        |
| **artifacts**            | Files saved after workflow             | Logs, binaries                       |
| **upload-artifact**      | Save files                             | Share between jobs                   |
| **download-artifact**    | Retrieve saved files                   | Next job use                         |
| **status check**         | Result shown in PR                     | Pass / Fail                          |
| **workflow_dispatch**    | Manual trigger                         | Click “Run workflow”                 |
| **schedule**             | Time-based trigger                     | Cron jobs                            |
| **cron**                 | Schedule format                        | `"0 2 * * *"`                        |
| **context**              | Metadata available in workflow         | `github`, `env`, `steps`             |
| **github context**       | Repo & event info                      | `github.ref`                         |
| **steps context**        | Step outputs                           | `steps.build.outputs`                |
| **outputs**              | Data passed between jobs               | Job → Job                            |
| **concurrency**          | Prevent parallel runs                  | One deploy at a time                 |
| **OIDC**                 | Secure auth without secrets            | AWS auth best practice               |
| **Reusable Workflow**    | Workflow called by another             | DRY principle                        |
| **Composite Action**     | Multiple steps as one action           | Custom action                        |
| **Marketplace**          | Public actions repository              | github.com/marketplace               |

