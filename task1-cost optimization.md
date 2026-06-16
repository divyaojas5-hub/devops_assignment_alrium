#!/bin/bash


RDS_INSTANCE_ID="my-rds-instance"   --Replace with your RDS instance identifier
AWS_REGION="us-east-1"              --Replace with your region

ACTION=$1

if [ "$ACTION" == "start" ]; then
    echo "Starting RDS instance: $RDS_INSTANCE_ID"
    aws rds start-db-instance \
        --db-instance-identifier $RDS_INSTANCE_ID \
        --region $AWS_REGION
elif [ "$ACTION" == "stop" ]; then
    echo "Stopping RDS instance: $RDS_INSTANCE_ID"
    aws rds stop-db-instance \
        --db-instance-identifier $RDS_INSTANCE_ID \
        --region $AWS_REGION
else
    echo "Usage: $0 {start|stop}"
    exit 1
fi

Eventbridge schedule:

Stop every weekday at 8 PM

cron(0 20 ? * MON-FRI *)


Weekend Stop

cron(0 0 ? * SAT *)



additional cost optimisation steps across ECS, ALB, and data transfer


ECS Fargate
Use spot tasks for non‑critical workloads.
Right‑size CPU/memory allocations.
Apply scheduled scaling to reduce tasks during off‑hours.

RDS
Considering Aurora Serverless v2 for auto‑scaling.
Enable storage auto‑scaling.
Use smaller instance class for staging.

ALB
consolidate multiple ALBs into one.
Tune idle timeout to reduce unnecessary connections.
Use path‑based routing to optimize traffic.

Data Transfer
Placing ECS tasks and RDS in the same AZ to avoid cross‑AZ charges.
Use VPC endpoints for S3/ECR traffic.
Enabling compression and caching with CloudFront.
