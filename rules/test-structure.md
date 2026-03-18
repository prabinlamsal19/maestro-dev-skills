---
name: test-structure
description: YAML test structure, appId, env variables, and flow definition
metadata:
  tags: yaml, structure, appId, env, flow
---

## Basic Structure

Every Maestro test is a YAML file with two sections separated by `---`:

```yaml
# Header section (metadata)
appId: com.example.myApp
env:
  USERNAME: test@example.com
  PASSWORD: secret123
---
# Flow section (test steps)
- launchApp
- tapOn: "Login"
- assertVisible: "Dashboard"
```

## Header Properties

| Property | Description           | Example               |
| -------- | --------------------- | --------------------- |
| `appId`  | Bundle ID (mobile)    | `com.example.myApp`   |
| `url`    | Web URL (desktop)     | `https://example.com` |
| `name`   | Flow name (optional)  | `"Login Flow"`        |
| `env`    | Environment variables | See below             |

## Environment Variables

Define constants in the header:

```yaml
appId: com.example.myApp
env:
  API_URL: https://api.example.com
  USERNAME: ${USERNAME || 'default@example.com'}
  PASSWORD: ${PASSWORD || 'password123'}
---
- inputText: ${USERNAME}
```

Variables support JavaScript expressions for defaults.

## External Parameters

Pass values from command line:

```bash
maestro test -e USERNAME=john@example.com -e PASSWORD=secret flow.yaml
```

Access with `${VARIABLE_NAME}` syntax in the flow.

## Multiple Flows in Directory

Run all flows in a directory:

```bash
maestro test e2e/
```

Maestro executes all `.yaml` files.

## Flow Naming Convention

```
{feature}_{action}.yaml

Examples:
- login_success.yaml
- login_invalid_credentials.yaml
- register_new_user.yaml
- dashboard_navigation.yaml
```

## Workspace Configuration (`config.yaml`)

A `config.yaml` file at the project root or `.maestro/` directory configures global settings:

```yaml
# config.yaml
flows:
  - 'subFolder/*'
  - 'anotherSubfolder/**'

includeTags:
  - smoke
excludeTags:
  - wip

executionOrder:
  continueOnFailure: false  # default is true
  flowsOrder:
    - flowA
    - flowB

testOutputDir: test_output_directory

# Platform configuration
platform:
  ios:
    snapshotKeyHonorModalViews: false
    disableAnimations: true          # Cloud only
  android:
    disableAnimations: true          # Cloud only

# Cloud-only config
baselineBranch: main
notifications:
  email:
    enabled: true
    recipients:
      - team@example.com
  slack:
    endpoint: https://hooks.slack.com/...
```

### Key Properties

| Property                         | Description                                         |
| -------------------------------- | --------------------------------------------------- |
| `flows`                          | Glob patterns for test discovery (default: `*`)     |
| `testOutputDir`                  | Custom output directory for screenshots/logs        |
| `includeTags` / `excludeTags`    | Filter flows by tags                                |
| `executionOrder.flowsOrder`      | Ordered list of flows to run sequentially           |
| `executionOrder.continueOnFailure` | Stop on failure if `false` (default: `true`)      |
| `platform.ios.disableAnimations` | Reduce Motion on iOS Simulator (Cloud only)         |
| `platform.ios.snapshotKeyHonorModalViews` | Include background hierarchy with modals  |
| `platform.android.disableAnimations` | Disable system animations (Cloud only)          |
