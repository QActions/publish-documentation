# Publish service documentation

Used to publish service documentation to swagger.extrahorizon.com.

- Uploads the contents of the `./documentation/developers/` folder to the swagger.extrahorizon.com S3 bucket
  - As a subfolder based on the project name and project version
  - For example: `swagger.extrahorizon.com/users-service/1.0.8/`
  - Adds a `-dev` suffix for all builds except for builds on the `master` branch
  - For example: `swagger.extrahorizon.com/users-service/1.0.8-dev/`
- Tells CloudFront to refresh its cache for swagger.extrahorizon.com

Expected environment variables:
```sh
# Project info, set by QActions/expose-project-info
PROJECT_VERSION=1.0.0
PROJECT_NAME=my-service

# AWS credentials
AWS_ACCESS_KEY_ID=......
AWS_SECRET_ACCESS_KEY=......
AWS_REGION=eu-central-1
```

Example usage:
```yml
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      # Exposes PROJECT_VERSION and PROJECT_NAME
      - name: Expose project info
        uses: QActions/expose-project-info@1.0.0

      - name: Build and publish the documentation
        uses: QActions/publish-service-documentation@2.0.0
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_REGION: ${{ secrets.AWS_REGION }}
```