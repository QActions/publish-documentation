# Publish service documentation

Used to publish service documentation to developers.extrahorizon.io.

- Converts the markdown files in `./documentation/developers/` folder with vuepress to static html files
- Moves the `openapi.yaml` in the result folder
- Uploads the result folder to the developers.extrahorizon.io S3 bucket
- Tells CloudFront to refresh its cache for developers.extrahorizon.io

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
        uses: QActions/publish-service-documentation@1.0.0
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_REGION: ${{ secrets.AWS_REGION }}
```