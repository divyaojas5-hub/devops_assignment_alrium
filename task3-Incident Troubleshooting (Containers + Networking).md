Step‑by‑Step Runbook
1.Check ALB Target Health
AWS Console → EC2 → Target Groups → Health checks.
using CLI:
    aws elbv2 describe-target-health --target-group-arn <TG_ARN>

2.Verify Security Groups & Networking
  Ensure ECS tasks allow inbound traffic from ALB SG.
   using CLI:   aws ec2 describe-security-groups --group-ids <SG_ID>


3.Check ECS Task Definitions & Health Checks
  Confirm container health check command matches app readiness.
    using CLI:    aws ecs describe-task-definition --task-definition my-task

4.Review ALB Listener Rules
  Ensure correct path/host routing rules.
     using CLI: aws elbv2 describe-listeners --load-balancer-arn <ALB_ARN>


Plausible Root Causes

Container health check misconfigured
Confirm by checking ECS task health status.
would Fix BY: Adjust health check path/timeout.

Networking/security group mismatch
ALB cannot reach ECS tasks.
would Fix BY: Update SG rules to allow traffic.

App not ready on startup (cold start issue)
Tasks marked RUNNING but app not serving yet.
would Fix BY: Increase container health check grace period.




ROLLBACK PLAN

By using ECS console or CLI to redeploy previous task definition revision:

aws ecs update-service \
  --cluster my-ecs-cluster \
  --service my-ecs-service \
  --task-definition my-task:REVISION \
  --force-new-deployment
