## 🧑‍💻 DevOps Interview Verdict
 * 🔥 Modern DevOps prefers `GitHub Actions`
 * 🌿 Legacy & enterprise still use `Jenkins`

### 🎯 Best Short Answer (Safe & Strong)
 * 🌿 Compared to Jenkins, GitHub Actions is the better choice when the code is already on GitHub because it removes infrastructure management, requires `almost zero setup`, and is easier to maintain.
 * 🎯 We don’t need to manage servers or plugins, and security is improved with features like `OIDC instead of long-lived secrets`.
 * 🎯 Jenkins is still powerful for very complex or legacy setups, but for `modern CI/CD`, GitHub Actions is usually `more efficient`.
 * 🌿 GitHub Actions is `more cost-effective` because it reduces infrastructure, maintenance, and operational costs.

### 🏁 One-Line Closing (Mic-Drop 🎤)
 * 🚨 If Jenkins is a powerful engine, GitHub Actions is a `built-in`, `modern`, `less maintenance`, `faster delivery`.
 * 🔥 In one of our project, CI costs were `high` due to `frequent pushes`. We optimized this by triggering workflows only on `pull requests`, cancelling older runs on new commits, and caching dependencies. As a result, we reduced GitHub Actions usage and improved pipeline speed.”

---

## ⚡ GitHub Actions — Rapid Fire Q&A
| 🔢 Q#   | ❓ Question                               | 💡 Answer                                                                                                                                                |
| ------- | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 🔹 Q1   | What is GitHub Actions?                  | 👉 Built-in CI/CD and automation platform in GitHub.                                                                                                               |
| 🔹 Q2   | Main purpose of GitHub Actions?          | 👉 Automate build, test, scan, and deployment workflows.                                                                                                           |
| 🔹 Q3   | Workflow files location?                 | 👉 `.github/workflows/`                                                                                                                                            |
| 🔹 Q4   | Workflow file format?                    | 👉 YAML                                                                                                                                                            |
| ⚙️ Q5   | Main sections of workflow?               | 👉 `name` <br> `on` <br> `jobs` <br> `steps`                                                                                                                       |
| ⚙️ Q6   | Basic workflow example?                  | 👉 `yaml\nname: CI\n\non:\n  push:\n    branches:\n      - main\n\njobs:\n  build:\n    runs-on: ubuntu-latest\n\n    steps:\n      - uses: actions/checkout@v4\n` |
| 🚀 Q7   | What does on: define?                    | 👉 Workflow trigger events.                                                                                                                                        |
| 🚀 Q8   | Common triggers?                         | 👉 `push` <br> `pull_request` <br> `workflow_dispatch` <br> `schedule`                                                                                             |
| 🚀 Q9   | Manual workflow trigger?                 | 👉 `workflow_dispatch`                                                                                                                                             |
| 🚀 Q10  | Scheduled workflow example?              | 👉 `yaml\non:\n  schedule:\n    - cron: "0 2 * * *"\n`                                                                                                             |
| 🛠️ Q11 | What is a job?                           | 👉 Collection of steps executed on runner.                                                                                                                         |
| 🛠️ Q12 | What is a step?                          | 👉 Individual command/action.                                                                                                                                      |
| 🛠️ Q13 | Can jobs run in parallel?                | 👉 ✅ Yes                                                                                                                                                           |
| 🛠️ Q14 | Make one job depend on another?          | 👉 `needs: build`                                                                                                                                                  |
| 🖥️ Q15 | What is a runner?                        | 👉 Machine executing workflows.                                                                                                                                    |
| 🖥️ Q16 | Types of runners?                        | 👉 GitHub-hosted <br> Self-hosted                                                                                                                                  |
| 🖥️ Q17 | Common GitHub-hosted runner OS?          | 👉 Ubuntu                                                                                                                                                          |
| 🖥️ Q18 | Why use self-hosted runners?             | 👉 Custom tooling/network/security.                                                                                                                                |
| 🔧 Q19  | What is an action?                       | 👉 Reusable automation component.                                                                                                                                  |
| 🔧 Q20  | Checkout repository action?              | 👉 `uses: actions/checkout@v4`                                                                                                                                     |
| 🔧 Q21  | Setup Java example?                      | 👉 `uses: actions/setup-java@v4`                                                                                                                                   |
| 🌍 Q22  | Define environment variable?             | 👉 `yaml\nenv:\n  ENV: prod\n`                                                                                                                                     |
| 🌍 Q23  | Access variable in workflow?             | 👉 `${{ env.ENV }}`                                                                                                                                                |
| 🔐 Q24  | Where store sensitive values?            | 👉 GitHub Secrets                                                                                                                                                  |
| 🔐 Q25  | Access secret?                           | 👉 `${{ secrets.AWS_SECRET }}`                                                                                                                                     |
| 🔐 Q26  | Why avoid hardcoding secrets?            | 👉 Security risk.                                                                                                                                                  |
| 🔄 Q27  | What is Continuous Integration (CI)?     | 👉 Automatically building/testing code changes.                                                                                                                    |
| 🔄 Q28  | What is Continuous Deployment (CD)?      | 👉 Automated deployment process.                                                                                                                                   |
| 🐳 Q29  | Can GitHub Actions build Docker images?  | 👉 ✅ Yes                                                                                                                                                          |
| ☸️ Q30  | Can it deploy to Kubernetes?             | 👉 ✅ Yes                                                                                                                                                          |
| ☸️ Q31  | Common Kubernetes deployment tools used? | 👉 `kubectl`, `Helm`                                                                                                                                               |
| ☁️ Q32  | How connect GitHub Actions to AWS?       | 👉 IAM credentials or OIDC.                                                                                                                                        |
| ☁️ Q33  | Why OIDC preferred?                      | 👉 No long-term AWS secrets required.                                                                                                                              |
| 🔢 Q34  | What is matrix build?                    | 👉 Run workflow across multiple versions/platforms.                                                                                                                |
| 🔢 Q35  | Example matrix use case?                 | 👉 Test app on Node.js 18 & 20.                                                                                                                                    |
| ⚡ Q36   | Why use caching?                         | 👉 Faster workflows.                                                                                                                                               |
| ⚡ Q37   | Common cache targets?                    | 👉 npm, Maven, pip dependencies.                                                                                                                                   |
| 📦 Q38  | What are artifacts?                      | 👉 Files stored after workflow execution.                                                                                                                          |
| 📦 Q39  | Example artifacts?                       | 👉 Build binaries, reports, logs.                                                                                                                                  |
| 🛡️ Q40 | How secure workflows?                    | 👉 Use secrets <br> Restrict permissions <br> Protect branches                                                                                                     |
| 🛠️ Q41 | Workflow not starting?                   | 👉 Check: <br> Trigger event <br> YAML syntax <br> Branch filters                                                                                                  |
| 🛠️ Q42 | Workflow failing?                        | 👉 Review logs in Actions tab.                                                                                                                                     |
| 🚨 Q43  | Deployment failed after merge — action?  | 👉 Rollback + analyze logs.                                                                                                                                        |
| 🚨 Q44  | CI pipeline too slow — optimization?     | 👉 Caching + parallel jobs.                                                                                                                                        |
| 🚨 Q45  | Why separate CI and CD pipelines?        | 👉 Better control and approvals.                                                                                                                                   |
| 🌿 Q46  | GitHub Actions role in GitOps?           | 👉 Update manifests/images in Git repository.                                                                                                                      |
| 🌿 Q47  | Which tool syncs Git changes to cluster? | 👉 Argo CD                                                                                                                                                         |
| 🧩 Q48  | Reusable workflows?                      | 👉 Shared workflows across repos.                                                                                                                                  |
| 🧩 Q49  | Composite actions?                       | 👉 Bundle multiple steps into reusable action.                                                                                                                     |
| 🏆 Q50  | Why GitHub Actions popular in DevOps?    | 👉 Native GitHub integration + easy CI/CD automation.                                                                                                              |

---

## ⚡ Scenario-Based GitHub Actions — Rapid Fire Q&A
| 🔢 Q#   | ❓ Question                                                             | 💡 Answer                                                                                |
| ------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| 🔹 Q1   | Developer pushed code but workflow didn’t trigger — what do you check? | 👉 Check: <br> Workflow file path <br> Trigger (`on:`) <br> Branch name <br> YAML syntax |
| 🔹 Q2   | Workflow YAML exists but Actions tab shows nothing — reason?           | 👉 Workflow file may not be in: <br><br> `.github/workflows/`                            |
| 🔹 Q3   | CI pipeline failing after merge — first step?                          | 👉 Check workflow logs and failed step.                                                  |
| 🛠️ Q4  | Application builds locally but fails in GitHub Actions — why?          | 👉 Environment mismatch/dependency issues.                                               |
| 🛠️ Q5  | Pipeline suddenly fails without code change — possible reasons?        | 👉 Dependency updates <br> Runner changes <br> Expired secrets                           |
| 🛠️ Q6  | Node.js version mismatch in CI — fix?                                  | 👉 Use: <br><br> `actions/setup-node`                                                    |
| 🔐 Q7   | AWS credentials exposed in workflow logs — action?                     | 👉 Rotate credentials immediately.                                                       |
| 🔐 Q8   | Best way to access AWS securely from GitHub Actions?                   | 👉 OIDC federation.                                                                      |
| 🔐 Q9   | Why avoid storing secrets in YAML?                                     | 👉 Security risk.                                                                        |
| 🚀 Q10  | Production deployment triggered accidentally — prevention?             | 👉 Use: <br> Protected branches <br> Environment approvals                               |
| 🚀 Q11  | Deployment failed midway — what next?                                  | 👉 Rollback deployment + analyze logs.                                                   |
| ☸️ Q12  | Kubernetes deployment not updating after workflow success — why?       | 👉 Check: <br> Image tag <br> kubeconfig <br> Deployment rollout status                  |
| 🐳 Q13  | Docker image build failing in Actions — checks?                        | 👉 Dockerfile <br> Disk space <br> Registry authentication                               |
| 🐳 Q14  | Docker push denied — why?                                              | 👉 Invalid registry credentials.                                                         |
| 🌿 Q15  | GitHub Actions + Argo CD deployment flow?                              | 👉 Actions updates manifests → Argo CD syncs cluster.                                    |
| 🌿 Q16  | Argo CD shows OutOfSync after workflow — reason?                       | 👉 Git repo and cluster state differ.                                                    |
| 🖥️ Q17 | Self-hosted runner offline — impact?                                   | 👉 Jobs remain queued/fail.                                                              |
| 🖥️ Q18 | Why use self-hosted runners?                                           | 👉 Private network/custom software access.                                               |
| 🖥️ Q19 | GitHub-hosted runner limitations?                                      | 👉 Limited customization and network access.                                             |
| ⚡ Q20   | Pipeline taking too long — optimization?                               | 👉 Caching <br> Parallel jobs <br> Smaller builds                                        |
| ⚡ Q21   | Why use dependency caching?                                            | 👉 Avoid repeated downloads.                                                             |
| 🔄 Q22  | One job depends on another — how define?                               | 👉 `needs: build`                                                                        |
| 🔄 Q23  | Can jobs run simultaneously?                                           | 👉 ✅ Yes                                                                                 |
| 🛠️ Q24 | Workflow stuck in queued state — why?                                  | 👉 No available runners.                                                                 |
| 🛠️ Q25 | YAML syntax error troubleshooting?                                     | 👉 Validate indentation carefully.                                                       |
| 🛠️ Q26 | Workflow passes but app broken in production — why?                    | 👉 Missing integration/e2e tests.                                                        |
| 🔀 Q27  | PR merged without CI checks — prevention?                              | 👉 Branch protection rules.                                                              |
| 🔀 Q28  | Why enforce mandatory CI checks?                                       | 👉 Prevent broken code merges.                                                           |
| 🛡️ Q29 | Malicious PR trying to expose secrets — protection?                    | 👉 Restrict secrets for forked PRs.                                                      |
| 🛡️ Q30 | Why least-privilege permissions important?                             | 👉 Reduce blast radius.                                                                  |
| 🚨 Q31  | GitHub Actions deployment failed due to expired token — fix?           | 👉 Rotate/update secrets.                                                                |
| ☁️ Q32  | EKS deployment failing from Actions — what check first?                | 👉 IAM permissions + kubeconfig.                                                         |
| 🏗️ Q33 | Terraform apply failed in Actions — why?                               | 👉 State lock, credentials, syntax errors.                                               |
| 📢 Q34  | How notify team on workflow failure?                                   | 👉 Slack/email/webhook integrations.                                                     |
| 📢 Q35  | Why monitor CI/CD pipelines?                                           | 👉 Faster incident response.                                                             |
| 🔄 Q36  | Bad deployment pushed automatically — response?                        | 👉 Rollback immediately.                                                                 |
| 🔄 Q37  | Best rollback strategy?                                                | 👉 Previous stable image/version.                                                        |
| 🌍 Q38  | Separate workflows for dev/stage/prod — why?                           | 👉 Better isolation and approvals.                                                       |
| 🌍 Q39  | How avoid deploying dev code to prod?                                  | 👉 Environment protection rules.                                                         |
| 🧩 Q40  | Workflow reusable across repos — solution?                             | 👉 Reusable workflows.                                                                   |
| 🧩 Q41  | Why use composite actions?                                             | 👉 Reusability and cleaner workflows.                                                    |
| 🏆 Q42  | Why GitHub Actions popular in DevOps?                                  | 👉 Native GitHub integration + simple automation.                                        |
| 🏆 Q43  | Jenkins vs GitHub Actions in real projects?                            | 👉 Jenkins = flexible/customizable <br> 👉 GitHub Actions = simpler GitHub-native CI/CD  |

## 🚀 Quick Interview Summary
| 📌 Scenario              | 💡 Best Practice                                   |
| ------------------------ | -------------------------------------------------- |
| 🔐 Secrets Management    | Use GitHub Secrets + OIDC                          |
| 🚀 Deployment Safety     | Protected branches + approvals                     |
| ⚡ Pipeline Optimization  | Cache dependencies + parallel jobs                 |
| ☸️ Kubernetes Deployment | Verify image tag + rollout status                  |
| 🌿 GitOps                | GitHub Actions updates Git → Argo CD syncs cluster |
| 🛡️ Security             | Least privilege + restricted secrets               |
| 🔄 Rollback              | Revert to previous stable version/image            |
