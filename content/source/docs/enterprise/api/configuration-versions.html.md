---
layout: enterprise2
page_title: "Configuration Versions - API Docs - Terraform Enterprise"
sidebar_current: "docs-enterprise2-api-configuration-versions"
---

# Configuration Versions API

-> **Note**: These API endpoints are in beta and are subject to change.

A configuration version (`configuration-version`) is a resource used to reference the uploaded configuration files. It is associated with the run to use the uploaded configuration files for performing the plan and apply.

## Create a Configuration Version

`POST /workspaces/:workspace_id/configuration-versions`

| Parameter       | Description                                                             |
| --------------- | ----------------------------------------------------------------------- |
| `:workspace_id` | **Required.** The workspace ID to create the new configuration version. |

### Sample Payload

```json
{
  "data": {
    "type": "configuration-versions"
  }
}
```

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $ATLAS_TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request POST \
  --data @payload.json \
  https://app.terraform.io/api/v2/workspaces/ws-2Qhk7LHgbMrm3grF/configuration-versions
```

### Sample Response

```json
{
  "data": {
    "id": "cv-ntv3HbhJqvFzamy7",
    "type": "configuration-versions",
    "attributes": {
      "upload-url":
        "https://ARCHIVIST/v1/object/4c44d964-eba7-4dd5-ad29-1ece7b99e8da"
    }
  }
}
```

## Upload Configuration Files

-> **Note**: Uploading a configuration file automatically creates a run and associates it with this configuration-version. Therefore it is unnecessary to [create a run on the workspace](./run.html#create-a-run) if a new file is uploaded.

`POST https://ARCHIVIST/v1/object/4c44d964-eba7-4dd5-ad29-1ece7b99e8da`

**The URL is provided in the `upload-url` attribute in the `configuration-versions` resource.**

### Request Body

This POST endpoint requires the following properties as a request payload.

Properties without a default value are required.

| Key path | Type | Default | Description                                                                                                                                                                                                         |
| -------- | ---- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `data`   | file |         | **Required.** A local .tar.gz file containing the folder of the terraform configuration files. This file can be created by running `tar -zcvf ./config.tar.gz .` from within the terraform configuration directory. |

### Sample Request

```shell
curl \
  --request PUT \
  -F 'data=@config.tar.gz' \
  https://ARCHIVIST/v1/object/4c44d964-eba7-4dd5-ad29-1ece7b99e8da
```

## List Configuration Versions

`GET /workspaces/:workspace_id/configuration-versions`

| Parameter       | Description                                                             |
| --------------- | ----------------------------------------------------------------------- |
| `:workspace_id` | **Required.** The workspace ID to create the new configuration version. |

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $ATLAS_TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  https://app.terraform.io/api/v2/workspaces/ws-2Qhk7LHgbMrm3grF/configuration-versions
```

### Sample Response

```json
{
  "data": [
    {
      "id": "cv-q883NfwaUrLntKrZ",
      "type": "configuration-versions",
      "attributes": {
        "error": null,
        "source": "github",
        "status": "pending",
        "status-timestamps": {},
        "upload-url": "https://ARCHIVIST/v1/object/91f9aea4-749f-4415-aa54-f76bad6250dd"
      },
      "relationships": {
        "ingress-attributes": {
          "data": { "id": "ia-XnKwoPUb1zruaoGX", "type": "ingress-attributes" },
          "links": {
            "related":
              "/api/v2/configuration-versions/cv-q883NfwaUrLntKrZ/ingress-attributes"
          }
        }
      },
      "links": { "self": "/api/v2/configuration-versions/cv-q883NfwaUrLntKrZ" }
    }
  ]
}
```

## Get a Configuration Version

`GET /configuration-versions/:id`

| Parameter | Description                                        |
| --------- | -------------------------------------------------- |
| `:id`     | **Required.** The ID of the configuration version. |

### Sample Request

```shell
curl \
  --header "Authorization: Bearer $ATLAS_TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  https://app.terraform.io/api/v2/configuration-versions/:id
```

### Sample Response

```json
{
  "data": {
    "id": "cv-q883NfwaUrLntKrZ",
    "type": "configuration-versions",
    "attributes": {
      "error": null,
      "source": "github",
      "status": "pending",
      "status-timestamps": {},
      "upload-url": "http://ARCHIVIST/v1/object/5338a80d-51b2-4122-adad-5b63553f68b1"
    },
    "relationships": {
      "ingress-attributes": {
        "data": { "id": "ia-XnKwoPUb1zruaoGX", "type": "ingress-attributes" },
        "links": {
          "related":
            "/api/v2/configuration-versions/cv-q883NfwaUrLntKrZ/ingress-attributes"
        }
      }
    },
    "links": { "self": "/api/v2/configuration-versions/cv-q883NfwaUrLntKrZ" }
  }
}
```
