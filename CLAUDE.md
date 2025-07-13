# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains a collection of GitHub Actions for managing Autonomi network testnets. Each action is a composite action that wraps the `testnet-deploy` command-line tool to perform various network operations.

## Architecture

The repository is organized as a collection of independent GitHub Actions, each in its own directory:

- **Network Lifecycle Actions**: `launch-network`, `bootstrap-network`, `destroy-network`
- **Node Management**: `start-nodes`, `stop-nodes`, `upgrade-network`, `reset-to-n-nodes`
- **Client Operations**: `client-deploy`, `start-uploaders`, `stop-uploaders`, `start-downloaders`, `stop-downloaders`
- **Infrastructure**: `provision`, `infra-plan`, `kill-droplets`, `configure-swapfile`
- **Monitoring & Metrics**: `start-telegraf`, `stop-telegraf`, `telegraf-upgrade-*-config`, `network-status`
- **Financial Operations**: `deposit-funds`, `drain-funds`, `start-faucet`, `stop-faucet`
- **Utilities**: `fetch-logs`, `cleanup-logs`, `print-inventory`, `update-peer`

## Core Concepts

### testnet-deploy Tool
All actions are wrappers around the `testnet-deploy` CLI tool, which is expected to be available in the `sn-testnet-deploy` directory. Actions construct command-line arguments dynamically based on provided inputs.

### Environment Types
Actions support three environment types that determine VM sizes and counts:
- `development`: Small-scale testing
- `staging`: Medium-scale testing  
- `production`: Full-scale deployment

### Network Architecture
Networks support multiple node types:
- **Genesis nodes**: Initial bootstrap nodes
- **Generic nodes**: Standard network nodes
- **Peer cache nodes**: Specialized caching nodes
- **Private nodes**: Full-cone and symmetric NAT configurations

### Cloud Providers
Actions support AWS and Digital Ocean as cloud providers, with region-specific deployments.

## Common Input Patterns

### Required Inputs
- `network-name`: Unique identifier for the testnet
- `environment-type`: Scale of deployment (development/staging/production)
- `provider`: Cloud provider (aws/digital-ocean)

### Version Control
Many actions accept version inputs for different components:
- `antnode-version`: Version of the Autonomi node binary
- `antctl-version`: Version of the control tool
- `ant-version`: Version of the client binary

### Custom Builds
Actions support building from custom branches:
- `autonomi-branch`: Build from specific branch
- `autonomi-repo-owner`: Build from forked repository
- `antnode-features`: Comma-separated Rust features

### VM Configuration
Actions allow fine-tuning of infrastructure:
- `*-vm-count`: Number of VMs for each node type
- `*-vm-size`: Size specification for VMs
- `*-volume-size`: Storage size in GB

## Development Workflow

Since this repository contains only GitHub Actions with no build scripts or tests, development involves:

1. **Editing Actions**: Modify `action.yml` files in individual directories
2. **Input Validation**: Ensure required inputs are properly defined
3. **Command Construction**: Verify bash command assembly logic
4. **Environment Variable Mapping**: Check input-to-environment variable mappings

## Testing

Actions are tested through actual GitHub workflow executions. There are no local test scripts or build processes in this repository.

## Key Files to Understand

- `launch-network/action.yml`: The primary network deployment action with comprehensive input options
- `bootstrap-network/action.yml`: Network bootstrapping from existing networks
- `upgrade-network/action.yml`: Rolling upgrade procedures for live networks
- `provision/action.yml`: Infrastructure provisioning for existing environments