---
description: Manage AWS resources and SSH access to EC2 instances
---

# Appendix. Quick Wrap-up

## Update Public DNS After DCV Instance Restart

To reduce costs, restarting a DCV EC2 instance (Stop → Start) changes its **Public DNS** if no Elastic IP or EBS volume is reserved. CloudFront distribution's origin still points to the old DNS, exposing users to 502 Bad Gateway errors.

CDK automatically tags all resources with `UserId=<userId>`, so a small script matches EC2 and CloudFront by tag and replaces only the origin DomainName.

### Workflow

1. Query EC2's current `PublicDnsName` using condition `tag:UserId=<userId>` + `state:running`
2. Find CloudFront distribution with `tag:UserId=<userId>`
3. Call `get-distribution-config` → replace origin `DomainName` with new DNS → call `update-distribution` (ETag matching required)
4. No-op if already matching

### Usage Example

```bash
# Immediately after DCV instance stop → start
cd ~/aws-physical-ai-recipes/e2e-workshop/infra/isaaclab
./scripts/fix-cloudfront-origin.sh <userId>
```

```bash
# If you deployed both Stable and Latest stacks with same userId (use discriminator)
./scripts/fix-cloudfront-origin.sh <userId> stable
./scripts/fix-cloudfront-origin.sh <userId> latest
```

### Results

After running this script, you'll see output like:

```bash
=== Results ===
Instance ID:      i-0ecd614dfd1f00b67
Public IP:        54.91.222.154
Public DNS:       ec2-54-91-222-154.compute-1.amazonaws.com
Distribution ID:  E1VCKFYZE31CU6
DCV URL:          https://54.91.222.154:8443
code-server URL:  https://d35g3id1b3geim.cloudfront.net
```

Save this output for reference.

### Prerequisites

- CDK has already applied `UserId` tags to all resources automatically (`isaac-lab-stack.ts` contains `cdk.Tags.of(this).add('UserId', userId)`) — the same tag propagates to CloudFront
- Target EC2 instance must be in `running` state for `PublicDnsName` to exist

{% hint style="info" %}
CloudFront updates usually complete within 2–5 minutes for edge location propagation. You may see 502 briefly during this time. Wait a moment before clearing your cache.
{% endhint %}

---

## Resolve `npm install` "no space left on device" in CloudShell

CloudShell provides only about **1GB** of permanent storage in the home directory. Repeated CDK deployments or switching between workshop modules accumulates `node_modules`, causing this error on subsequent `npm install`:

```
npm ERR! code ENOSPC
npm ERR! syscall write
npm ERR! errno -28
npm ERR! nospc ENOSPC: no space left on device
```

First, check where space is being used:

```bash
df -h ~                                                     # Home usage/limit
du -sh ~/.npm ~/.cache ~/.cdk ~/aws-physical-ai-recipes 2>/dev/null
du -ah ~ 2>/dev/null | sort -rh | head -20                  # Top 20 files/folders by size
```

### Solution 1: Clear npm cache and unnecessary node_modules

```bash
# Clear npm cache
npm cache clean --force
rm -rf ~/.npm

# Review node_modules in other modules (exclude current working directory)
find ~/aws-physical-ai-recipes -type d -name node_modules -prune -print
# Selectively delete unused paths from the output above
rm -rf <path-from-above>
```

### Solution 2: Clean CDK artifacts and other caches

CDK creates hundreds of MB in `cdk.out` on each synthesis (zip files, Docker context). pip, yarn, pnpm, and HuggingFace caches also consume significant space.

```bash
# CDK artifacts (regenerated on next cdk deploy/synth)
find ~/aws-physical-ai-recipes -type d -name cdk.out -prune -print
rm -rf <path-from-above>
rm -rf ~/.cdk                                # CDK CLI cache

# Other package manager caches
rm -rf ~/.cache/pip ~/.cache/yarn ~/.cache/pnpm
rm -rf ~/.cache/huggingface ~/.cache/torch    # Model/data caches (if present)
```

### Solution 3: Move working directory to `/tmp`

Move the entire CDK project to `/tmp` so `node_modules` and `cdk.out` use temporary storage instead of home quota.

```bash
cp -r ~/aws-physical-ai-recipes/e2e-workshop/infra/groot /tmp/
cd /tmp/groot
npm install
npx cdk deploy ...
```

> `/tmp` is wiped on session end, so back up cache files like `cdk.context.json` separately, or copy them home after deployment.

### Solution 4: Reset CloudShell home directory (last resort)

When none of the above work or mysterious residual files remain, reinitialize CloudShell.

Go to CloudShell top-right **Actions → Delete AWS CloudShell home directory**. This deletes **all** home directory files, so migrate important data to S3 or local storage first.

---
