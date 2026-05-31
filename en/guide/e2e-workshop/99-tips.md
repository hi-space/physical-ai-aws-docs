---
description: Manage AWS resources and SSH access to EC2 instances
---

# Appendix. Practical Tips and Reference

## 1. Amazon S3 Storage Management

[Amazon S3 (Simple Storage Service)](https://docs.aws.amazon.com/s3/latest/userguide/Welcome.html) is object storage for learning data, model checkpoints, and results. Store unlimited data without capacity limits and access it from anywhere, making it ideal for training pipeline input/output storage.

{% hint style="info" %}
AWS CLI must be installed to use S3 commands. See [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).
{% endhint %}

### 1.1 Create an S3 Bucket

```bash
# Create bucket (bucket name must be globally unique)
aws s3 mb s3://<BucketName> --region <Region>

# Example
aws s3 mb s3://isaaclab-workshop-user01 --region us-east-1
```

{% hint style="warning" %}
S3 bucket names must be **globally unique**. Only lowercase letters, numbers, and hyphens (-) are allowed, and names must be 3–63 characters. Include team or user names to ensure uniqueness.
{% endhint %}

### 1.2 Recommended Folder Structure (Example)

For robot reinforcement learning projects, use this folder structure. Append folder paths to `s3://<BucketName>/` when specifying S3 paths in CLI commands (e.g., `s3://isaaclab-workshop-user01/checkpoints/pretrained/agent_72000.pt`).

```
s3://<BucketName>/
├── datasets/                    # Training datasets
│   ├── demonstrations/          # VLA training data
│   └── episodes/                # Episode recording data
├── assets/                      # IsaacSim assets
│   ├── robots/                  # Robot URDF/USD files
│   └── environments/            # Environment USD scenes, objects, etc.
├── checkpoints/                 # Model checkpoints
│   ├── pretrained/              # Pre-trained models (agent_72000.pt, etc.)
│   └── experiments/             # Experiment results
│       ├── h1-rough-v1/         # Organized by experiment name
│       │   ├── best_agent.pt
│       │   └── config.yaml
│       └── h1-rough-v2/
├── logs/                        # Training logs
│   ├── tensorboard/             # TensorBoard event files
│   └── cloudwatch/              # Exported CloudWatch logs
└── results/                     # Final outputs
    ├── videos/                  # Simulation recordings
    └── reports/                 # Evaluation reports
```

### 1.3 Key CLI Commands

Use these commands to upload locally-trained models or download cloud checkpoints and assets.

**List buckets**

```bash
aws s3 ls
```

**List files in bucket**

View files and folders under a specific path. End the path with `/` to view inside a folder.

```bash
aws s3 ls s3://<BucketName>/checkpoints/
```

**Upload files (local → S3)**

Specify `<source> <target>` after `cp`. Use `--recursive` to upload entire folders.

```bash
# Single file
aws s3 cp ./best_agent.pt s3://<BucketName>/checkpoints/best_agent.pt

# Entire folder
aws s3 cp ./results/ s3://<BucketName>/results/ \
  --recursive
```

**Download files (S3 → local)**

Reverse the source and target direction to download from S3 to local. Use this to retrieve cloud-stored checkpoints.

```bash
# Single file
aws s3 cp s3://<BucketName>/checkpoints/agent_72000.pt ./agent_72000.pt

# Entire folder
aws s3 cp s3://<BucketName>/checkpoints/ ./checkpoints/ \
  --recursive
```

**Sync directories (transfer only changed files)**

`sync` compares source and target, transferring only modified files. Useful for periodically backing up logs and other growing data.

```bash
aws s3 sync ./logs/ s3://<BucketName>/logs/
```

**Delete files (clean up failed experiments)**

Use `rm` to delete S3 objects. Add `--recursive` to delete entire paths.

```bash
aws s3 rm s3://<BucketName>/checkpoints/failed-run/ \
  --recursive
```

**Generate temporary share URLs (1-hour validity)**

`presign` creates temporary unauthenticated access URLs. Use `--expires-in` to specify validity duration in seconds. Useful for quickly sharing files with team members.

```bash
aws s3 presign s3://<BucketName>/checkpoints/best_agent.pt \
  --expires-in 3600
```

{% hint style="info" %}
Using `aws s3 sync` for large checkpoint transfers saves time and cost by uploading only changed files.
{% endhint %}

<details>
<summary><strong>AccessDenied error when accessing S3</strong></summary>

CDK-deployed workshop infrastructure includes S3 permissions. For other environments, if `AccessDenied` errors occur, run:

```bash
# Check current instance's IAM Role
aws sts get-caller-identity

# Add S3 Full Access permission
ROLE_NAME=$(aws sts get-caller-identity --query Arn --output text | grep -oP 'assumed-role/\K[^/]+')
aws iam attach-role-policy \
  --role-name "$ROLE_NAME" \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

</details>

---

## 2. Elastic File System (EFS) Usage

[Amazon EFS](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html) is a shared filesystem accessible simultaneously from multiple instances and containers. EC2 instances and AWS Batch containers mount the same EFS, enabling immediate sharing of training results without separate file copying.

EFS capacity is unlimited and automatically scales with usage. No manual capacity provisioning is needed.

**Why we use EFS in this workshop:**
* AWS Batch jobs are ephemeral. After completion, container termination deletes local storage data.
* Saving checkpoints to EFS preserves training results after batch job termination.
* Mount the same EFS on the DCV EC2 instance to use training results immediately for inference without separate file copying.
* During multi-node training, when the main node writes checkpoints to EFS, other nodes access the same files.

### 2.1 Mount EFS

Check the file system ID in the EFS console, then mount.

```bash
# Mount EFS (adjust region to your deployment: us-east-1 or us-west-2)
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport \
  <FileSystemID>.efs.<Region>.amazonaws.com:/ /home/ubuntu/environment/efs
```

```bash
# Verify mount
df -h | grep efs
```

{% hint style="warning" %}
Run the mount command on the **host (EC2 instance)**, not in Docker.
{% endhint %}

### 2.2 Use EFS in Docker Containers

Use the `-v` option to mount the host's EFS directory inside the container.

```bash
docker run --gpus all -it \
    -v /home/ubuntu/environment/efs:/workspace/IsaacLab/TrainedModel \
    isaaclab-batch:latest
```

| Host Path | Container Path | Purpose |
|-----------|----------------|---------|
| `/home/ubuntu/environment/efs` | `/workspace/IsaacLab/TrainedModel` | Share trained models |
| `/home/ubuntu/environment/efs/models` | `/efs/models` (Batch) | Store batch training results |

---

## 3. Elastic Container Registry (ECR) Management

[Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html) is a central registry for Docker container images. When you push Isaac Sim + Isaac Lab images to ECR, multiple AWS Batch nodes pull the same image for consistent training environments.

### 3.1 ECR Login

```bash
aws ecr get-login-password --region <Region> | docker login --username AWS --password-stdin <AccountID>.dkr.ecr.<Region>.amazonaws.com
```

### 3.2 Build and Push Image

```bash
# Build Docker image
docker build -t isaaclab-batch .

# Tag for ECR repository
docker tag isaaclab-batch:latest <AccountID>.dkr.ecr.<Region>.amazonaws.com/isaaclab-batch:latest

# Push to ECR
docker push <AccountID>.dkr.ecr.<Region>.amazonaws.com/isaaclab-batch:latest
```

### 3.3 Pull Image

```bash
docker pull <AccountID>.dkr.ecr.<Region>.amazonaws.com/isaaclab-batch:latest
```

### 3.4 Image Management Tips

* **Tagging Strategy**: Use version tags instead of `latest` (e.g., `isaaclab-batch:v1.0`, `isaaclab-batch:experiment-001`)
* **Clean Unused Images**: Set Lifecycle Policy in ECR console to auto-delete old images
* **Scan for Vulnerabilities**: Use ECR's image scanning to detect security issues

---

## 4. SSH Access Guide for IsaacLab EC2 Instance

DCV's browser terminal has high input latency and inconvenient copy/paste. Direct SSH access provides terminal speed matching local machines. VS Code Remote-SSH extension enables file exploration, editing, debugging, and all IDE features as if working locally.

### 4.1 Automated Setup

Use this script for one-command SSH setup. Replace `<PUBLIC_IP>` with your EC2 instance IP.

{% tabs %}
{% tab title="Mac / Linux" %}
```bash
# [Local]
curl -fsSL https://raw.githubusercontent.com/hi-space/aws-physical-ai-recipes/main/tools/01-setup-ssh-client.sh -o setup-ssh.sh
bash setup-ssh.sh <PUBLIC_IP>
```
{% endtab %}

{% tab title="Windows (PowerShell)" %}
```powershell
# [Local] Run PowerShell as Administrator
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/hi-space/aws-physical-ai-recipes/main/tools/01-setup-ssh-client.ps1" -OutFile "setup-ssh.ps1"
.\setup-ssh.ps1 <PUBLIC_IP>
```
{% endtab %}
{% endtabs %}

Follow script prompts to register your public key on the EC2 instance for immediate access.

### 4.2 Manual Setup (Step-by-Step)

Use this guide for manual setup.

#### 4.2.1 Generate SSH Key (if none exists)

```bash
# [Local]
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N ""
```

#### 4.2.2 Configure SSH Config

Add the following to your local PC's `~/.ssh/config`. Replace `<PUBLIC_IP>` with your IP.

```
# [Local] Add to ~/.ssh/config
Host isaaclab
    HostName <PUBLIC_IP>
    User ubuntu
    IdentityFile ~/.ssh/id_ed25519
```

#### 4.2.3 Initial Connection (Register Public Key)

**Step 1.** Check your local public key content.

```bash
# [Local]
cat ~/.ssh/id_ed25519.pub
```

**Step 2.** Connect via EC2 Instance Connect from AWS Console, then register the public key.

1. AWS Console > EC2 > Select instance > **Connect** > **EC2 Instance Connect** tab > **Connect**
2. Browser terminal opens. Run:

```bash
# [EC2 instance] — run in browser terminal
echo "<PUBLIC_KEY_content>" >> /home/ubuntu/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys
```

#### 4.2.4 Subsequent Connections

After public key registration, connect directly:

```bash
# [Local]
ssh isaaclab
```

After SSH connection, perform all tasks via terminal instead of DCV.

### 4.3 Connect via VS Code Remote-SSH

Use VS Code's Remote-SSH extension to edit files on the remote EC2 instance as if local.

#### Install Extension

1. Open VS Code **Extensions** panel (or `Ctrl+Shift+X`)
2. Search `Remote - SSH` → Install extension by **Microsoft**

#### Connect

1. `Ctrl+Shift+P` (Mac: `Cmd+Shift+P`) → Select **Remote-SSH: Connect to Host...**
2. Select `isaaclab` from the list (the Host name from your SSH config in 4.2)
3. New VS Code window opens, connected to the remote instance

#### Open Remote Folder

After connecting, go to **File > Open Folder...** and select your working directory.

```
/home/ubuntu/workspace/IsaacLab
```

Now use all VS Code features—file explorer, editing, terminal, debugging—identically to your local environment.
