# ZKsync Server Action (L1 + L2)

Start a local **L1 (Anvil)** and **L2 (zksync_os_bin)** stack directly from official [`matter-labs/zksync-os-server`](https://github.com/matter-labs/zksync-os-server) release assets.

Perfect for **SDK integration** or **E2E testing** of deposits, withdrawals, and cross-chain flows.

## Features

✅ Automatically resolves `latest` release tag via GitHub API
✅ Supports `include_prerelease: true` for pre-release testing
✅ Downloads official binaries + state (`genesis.json`, `zkos-l1-state.json`)
✅ Boots both **L1 (Anvil)** and **L2 (zksync_os_bin)**
✅ Waits until both RPC endpoints respond
✅ Exports `ETH_RPC` and `ZKSYNC_RPC` for your subsequent test steps

## Quick Start

### **Minimal usage (latest stable)**

```yaml
- name: Run ZKsync OS
  uses: dutterbutter/zksync-server-action@v0.1.0
  with:
    version: latest
````

### **Pinned version**

```yaml
- name: Run ZKsync OS 
  uses: dutterbutter/zksync-server-action@v0.1.0
  with:
    version: v0.8.2
    l1_port: 8545
    l2_port: 3050
```

### **Include pre-releases**

```yaml
- name: Run ZKsync OS
  uses: dutterbutter/zksync-server-action@v0.1.0
  with:
    version: latest
    include_prerelease: true
```

## Inputs

| Name                 | Default                        | Description                                         |
| -------------------- | ------------------------------ | --------------------------------------------------- |
| `version`            | `latest`                       | Release tag (e.g. `v0.8.2`) or `latest`             |
| `include_prerelease` | `false`                        | If `true` and `version=latest`, allows pre-releases |
| `l1_port`            | `8545`                         | L1 RPC port (Anvil)                                 |
| `l2_port`            | `3050`                         | L2 RPC port (zksync_os_bin)                         |
| `linux_arch`         | `x86_64`                       | Architecture for binary (`x86_64` or `aarch64`)     |
| `wait_seconds`       | `300`                          | Max seconds to wait for L2 to be ready              |
| `set_env`            | `true`                         | Export `ETH_RPC` and `ZKSYNC_RPC` to `GITHUB_ENV`   |

## Outputs

| Name               | Description                         |
| ------------------ | ----------------------------------- |
| `l1_rpc_url`       | Local L1 RPC URL                    |
| `l2_rpc_url`       | Local L2 RPC URL                    |
| `resolved_version` | Actual tag resolved (e.g. `v0.8.2`) |

## Requirements

* **Runner:** `ubuntu-latest` (Anvil requires Linux)
* **Foundry:** must be available (to provide `anvil` binary)

Example setup:

```yaml
- name: Setup Foundry
  uses: foundry-rs/foundry-toolchain@v1
  with:
    version: v1.3.4
```

## Environment Variables

If `set_env` is true (default), these are automatically exported:

```bash
ETH_RPC=http://127.0.0.1:8545
ZKSYNC_RPC=http://127.0.0.1:3050
```

## Troubleshooting

* **Ports busy:** Adjust `l1_port` / `l2_port` if the runner already uses 8545 or 3050.
* **Timeouts:** Increase `wait_seconds` if CI hardware is slow.
* **Logs:**

  * `.zks/anvil.log` — Anvil output
  * `.zks/l2.log` — zksync_os_bin output
    Upload them on failure for debugging:

  ```yaml
  - name: Upload logs
    if: always()
    uses: actions/upload-artifact@v4
    with:
      name: zks-logs
      path: .zks/*.log
  ```

### Support

If you encounter issues not covered in the troubleshooting section, feel free to [open an issue](https://github.com/dutterbutter/anvil-zksync-action/issues) in the repository.

## Contributing 🤝

Feel free to open issues or PRs if you find any problems or have suggestions for improvements. Your contributions are more than welcome!

## License 📄

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**Happy Testing! 🚀**