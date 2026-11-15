# AMP Templates

Application templates for [CubeCoders AMP](https://cubecoders.com/AMP).

## GitHub Actions Runner

Manage a self-hosted GitHub Actions runner from AMP on Linux x86-64.

### Add the template

1. Open **Configuration → Instance Deployment → Configuration Repositories** in AMP.
2. Add `BlueIceWolf/AMPTemplates:main` and select **Fetch Latest**.
3. Create a new instance using **GitHub Actions Runner**.

### Configure the runner

Open the instance's **Configuration** page and fill in:

| Setting | Value |
| --- | --- |
| **GitHub repository or organization URL** | `https://github.com/OWNER/REPOSITORY` or `https://github.com/ORGANIZATION` |
| **Registration token** | Temporary token from GitHub |
| **Runner name** | Unique name for this instance |
| **Additional labels** | Optional comma-separated labels |

Get the registration token from **GitHub → Settings → Actions → Runners → New self-hosted runner**. For organization-wide runners, use the organization's Actions runner settings instead.

Save the settings and run **Update** on the AMP instance. AMP downloads the official runner and registers it using the values from the interface. Then click **Start**.

No SSH commands are needed for GitHub registration. Registration is skipped on subsequent updates if the runner is already configured. To move an existing runner to another repository, remove its old GitHub registration first; editing the URL does not migrate an existing registration.

A workflow can target the runner with:

```yaml
jobs:
  build:
    runs-on: [self-hosted, amp]
    steps:
      - uses: actions/checkout@v4
```

### Notes

- Linux x86-64, AMP 2.8.0.4 or newer.
- The runner package is pinned to `v2.338.2`.
- No inbound ports are required.
- The template has **not yet been tested on a live AMP 2.8 instance**.
- The runner uses `bin/Runner.Listener` directly; automatic restart behavior may differ from GitHub's `run.sh` script.
- Registration tokens expire. Generate a new token if registration fails. The AMP password field is used for entry, but the token may still be exposed to local process inspection or AMP update logs while registration runs. Clear it from AMP after registration.
- GitHub workflows execute code. Use an isolated runner with minimal host permissions and do not expose your game-server files or the host Docker socket.

Template structure follows [CubeCoders/AMPTemplates](https://github.com/CubeCoders/AMPTemplates).
