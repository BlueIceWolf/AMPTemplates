# AMP Templates

Custom application templates for [CubeCoders AMP](https://cubecoders.com/AMP).

## GitHub Actions Runner

Run a self-hosted GitHub Actions runner on a Linux server and manage its process through AMP.

This template targets Linux x86-64 and AMP 2.8.0.4 or newer. It uses the official [GitHub Actions runner](https://github.com/actions/runner).

### Installation

1. In AMP, open **Configuration → Instance Deployment → Configuration Repositories**.
2. Add `BlueIceWolf/AMPTemplates:main` and select **Fetch Latest**.
3. Create an instance using the **GitHub Actions Runner** template.
4. Run **Update** in the instance to download the runner.
5. Register the runner with GitHub before starting it.

### GitHub registration

Open **Settings → Actions → Runners → New self-hosted runner** in the GitHub repository where you want to run jobs.

Run the registration command as the user that owns the AMP instance files, from the directory containing `config.sh`:

```bash
./config.sh --url https://github.com/OWNER/REPOSITORY --token YOUR_REGISTRATION_TOKEN --unattended --name amp-runner-01
```

Replace the URL and token with the values provided by GitHub. Do not run the configuration as root or commit tokens to this repository.

After registration, start the instance in AMP. The runner should appear under **Settings → Actions → Runners** in GitHub.

To target it from a workflow:

```yaml
jobs:
  build:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v4
```

### Current limitations

This is an initial template and has **not yet been validated on a live AMP instance**.

- Registration is manual; AMP does not configure GitHub access automatically.
- The download is pinned to runner v2.338.2.
- The runner is started through `bin/Runner.Listener` rather than the official `run.sh` wrapper, so its update/restart behavior may differ.
- Docker-based workflows require a separate, properly isolated build environment.

### Security

Runner jobs execute repository workflow code. Use an isolated instance with only the permissions it needs, especially for public repositories and pull requests from untrusted contributors. Do not expose the AMP host, game-server data, secrets, or the host Docker socket to untrusted jobs.

For the supported configuration format, see the [official AMPTemplates repository](https://github.com/CubeCoders/AMPTemplates).
