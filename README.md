# gcp-bigquery-apigee
Access BigQuery APIs directly from Apigee using Auth extension for tokens. Apigee BQ extension only allows access to the tabledata API and this is limiting. Direct usage of BQ APIs is useful when there is a need to access APIs like jobs etc., The Apigee auth extension can help in getting the necessary tokens.

## Instructions

### GCP Project
1. Create GCP Project
2. Create BQ dataset
3. Create BQ table and import data
4. Create GCP service account and provide permissions (instructions below)
5. Download service account JSON file and setup Apigee auth extension
6. Import API proxy in this repo
7. Setup API product and create app
8. Use curl below to invoke

```
    APIGEE_ORGANIZATION=<my-apigee-org>
    GCP_PROJECT=<my-project>
    SERVICE_ACCOUNT_NAME=(APIGEE_ORGANIZATION + "@" + GCP_PROJECT + ".iam.gserviceaccount.com")

    echo "$APIGEE_ORGANIZATION"
    echo "$SERVICE_ACCOUNT_NAME"
    echo "$GCP_PROJECT"

    gcloud iam service-accounts create $APIGEE_ORGANIZATION \
        --description="Service account for BQTest $APIGEE_ORGANIZATION Apigee org" \
        --display-name="$APIGEE_ORGANIZATION"

    gcloud projects add-iam-policy-binding $GCP_PROJECT \
        --member serviceAccount:$SERVICE_ACCOUNT_NAME \
        --role roles/bigquery.user

    gcloud projects add-iam-policy-binding $GCP_PROJECT \
        --member serviceAccount:$SERVICE_ACCOUNT_NAME \
        --role roles/bigquery.dataViewer

    gcloud projects get-iam-policy $GCP_PROJECT \
    --flatten="bindings[].members" \
    --format='table(bindings.role)' \
    --filter="bindings.members:$SERVICE_ACCOUNT_NAME"

    gcloud iam service-accounts list
```

### Usage
```
    curl https://org-env.apigee.net/bqtest?maxResults=2&apikey=<you-key>
```


