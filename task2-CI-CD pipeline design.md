name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout code
      - name: Checkout repository
        uses: actions/checkout@v3

      # Step 2: Configure AWS credentials
      - name: Configure AWS
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      # Step 3: Login to ECR
      - name: Login to Amazon ECR
        run: |
          aws ecr get-login-password --region us-east-1 \
          | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

      # Step 4: Build Docker image
      - name: Build Docker image
        run: |
          docker build -t my-app:${{ github.sha }} .

      # Step 5: Tag and push image to ECR
      - name: Push to ECR
        run: |
          docker tag my-app:${{ github.sha }} <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/my-app:${{ github.sha }}
          docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/my-app:${{ github.sha }}

      # Step 6: Update ECS service
      - name: Deploy to ECS
        run: |
          aws ecs update-service \
            --cluster my-ecs-cluster \
            --service my-ecs-service \
            --force-new-deployment



## GitHub actions Pipeline Flow

- Push code to the main branch.
- GitHub Actions starts the workflow.
- Build the Docker image.
- Tag the image with the Git SHA.
- Push the image to Amazon ECR.
- Trigger ECS service deployment.
- ECS pulls the latest image.
- New tasks become healthy and serve traffic.
