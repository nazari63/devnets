## Description

The manifest file is used as a high level description of devnet with specific settings for both L1 and L2 layers. It defines:

1. Basic network information (name and type)
2. L1 configuration specifying the base network
3. L2 configuration including:
   - Deployment parameters
   - Default versions for various OP Stack components
   - Multiple L2 network configurations with unique names and chain IDs. Defaults can be overridden per network here.

## L2 Deployment Configuration

This section configures parameters used during the deployment of the L2 network:

- `op-deployer`: Version of the deployment tooling to use (tag or git sha)
- `l1-contracts`: Locator for the L1 contract implementations to deploy
- `l2-contracts`: Locator for the L2 contract implementations to deploy
- `overrides`: Optional configuration overrides for deployment parameters, for example:
  - `seconds_per_slot`: Block time in seconds
  - `fjord_time_offset`: Timestamp offset for the Fjord hardfork activation
  - `granite_time_offset`: Timestamp offset for the Granite hardfork activation
  - `holocene_time_offset`: Timestamp offset for the Holocene hardfork activation

## L2 Component Configurations

This section specifies versions and configuration for each L2 component:

- `version`: Version tag or git sha.
- `env`: Environment variables to configure feature flags and other runtime settings.

## L2 Chains

This section defines multiple L2 chains that are deployed using the same base configuration:

- `name`: Unique identifier for the L2 network instance
- `chain_id`: Unique chain ID for the L2 network
- Additional fields can be specified to override default configurations for this specific network

Each network entry inherits the default component versions and configurations defined above, but can override any setting as needed.

## Example Usage

```yaml
name: devnet
type: alphanet
scope: <scoping-issue-url>
l1:
  name: sepolia
  chain_id: 11155111
l2:
  deployment:
    op-deployer:
      version: v0.0.0
    l1-contracts:
      locator: <artifact-url>
      version: optimism-sha
    l2-contracts:
      locator: <artifact-url>
      version: optimism-sha
    overrides:
      seconds_per_slot: 2
      fjord_time_offset: 0
      granite_time_offset: 0
      holocene_time_offset: 0
  components:
    op-node:
      version: v0.0.0
    op-geth:
      version: v0.0.0
    op-reth:
      version: v0.0.0
    op-proposer:
      version: v0.0.0
    op-batcher:
      version: v0.0.0
      env:
        OP_BATCHER_MAX_CHANNEL_DURATION: "1h"
    op-challenger:
      version: v0.0.0
  bootnodes:
    - <enode uri>
  chains:
  - name: devnet-0
    chain_id: 901
    components:
      op-geth:
        env:
          GETH_VERBOSITY: "4"
  - name: devnet-1
    chain_id: 902
    deployment:
      overrides:
        seconds_per_slot: 4
```
