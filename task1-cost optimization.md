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
