# Docker Push

## Usage

```yaml
name: Docker Push

on: push

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v1
        with:
          fetch-depth: 1

      - name: Build & Push to Docker Hub
        uses: opspresso/action-docker@master
        env:
          USERNAME: ${{ secrets.DOCKER_USERNAME }}
          PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
          # IMAGE_NAME: "user_id/image_name"
          TAG_NAME: "v0.0.1"
          LATEST: "true"

      - name: Build & Push to GitHub Package
        uses: opspresso/action-docker@master
        env:
          USERNAME: ${{ secrets.DOCKER_USERNAME }}
          PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
          REGISRTY: "docker.pkg.github.com"
          # IMAGE_NAME: "user_id/image_name"
          TAG_NAME: "v0.0.1"
          LATEST: "true"

      - name: Build & Push to AWS ECR
        uses: opspresso/action-docker@master
        with:
          args: --ecr
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_REGION: "us-east-1"
          # IMAGE_NAME: "user_id/image_name"
          TAG_NAME: "v0.0.1"
          LATEST: "true"
```

## env for Docker Hub

Name | Description | Default | Required
---- | ----------- | ------- | --------
USERNAME | Your Docker Hub Username. | ${GITHUB_ACTOR} | No
PASSWORD | Your Docker Hub Password. | | **Yes**
REGISTRY | Your Docker Registry Uri. | | No
IMAGE_NAME | Your Docker Image name. | ${GITHUB_REPOSITORY} | No
TAG_NAME | Your Docker Tag name. | $(cat ./target/TAG_NAME) | No
LATEST | Use latest tag name. | false | No

## env for AWS ECR

Name | Description | Default | Required
---- | ----------- | ------- | --------
AWS_ACCESS_KEY_ID | Your AWS Access Key. | | **Yes**
AWS_SECRET_ACCESS_KEY | Your AWS Secret Access Key. | | **Yes**
AWS_REGION | Your AWS Region. | us-east-1 | No
AWS_ACCOUNT_ID | Your AWS Account ID. | $(aws sts get-caller-identity) | No
IMAGE_NAME | Your Docker Image name. | ${GITHUB_REPOSITORY} | No
IMAGE_URI | Your Docker Image uri. | ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${IMAGE_NAME} | No
TAG_NAME | Your Docker Tag name. | $(cat ./target/TAG_NAME) | No
IMAGE_TAG_MUTABILITY | The tag mutability setting for the repository. | MUTABLE | No
LATEST | Use latest tag name. | false | No
