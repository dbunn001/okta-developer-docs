---
title: Okta Identity Engine Policy API
---

# Okta Identity Engine Policy API

<ApiLifecycle access="ea" />

Okta Identity Engine adds new Policy objects to the Okta `/policies` API, to control and configure the behavior of the steps of the Okta Identity Engine Pipeline.

During the Limited EA phase, Okta Identity Engine is enabled or disabled for an org as a whole. If Okta Identity Engine is enabled for an org, these new Policies control the pipeline that end users progress through when accessing OpenID Connect apps. You cannot mix old Policy and Rule objects with new Okta Identity Engine objects; old objects do not affect the new pipeline.

API endpoints for creating, getting, and updating Policy and Rule objects function the same way in Okta Identity Engine as they do in the existing Okta `/policies` API; only the objects used are different, with Okta Identity Engine introducing a set of new Policy and Rule objects. See the [Okta Identity Engine Policy Objects](#okta-identity-enging-policy-objects) section of this document for descriptions of the objects.

## Policies API Operations

The API endpoints support the following operations:

 * [Get Policy](#get-policy)
 * [List Policies](#list-policies)
 * [Get Rule](#get-rule)
 * [List Rules](#list-rules)
 * [Create Policy](#create-policy)
 * [Create Rule](#create-rule)
 * [Update Policy](#update-policy)
 * [Update Rule](#update-rule)
 * [Delete Policy](#delete-policy)
 * [Delete Rule](#delete-rule)
 
### Get Policy

<ApiOperation method="get" url="/policies/${PolicyID}" />

Given a `PolicyID` identifier, returns a [Policy Object](#okta-identity-engine-policy-objects).

#### Request Body

| Property   | Type   | Description                          |
|------------|--------|--------------------------------------|
| `PolicyID` | String | The unique identifier of the Policy. |

#### Request Query Parameters

None

#### Response Body

A [Policy Object](#okta-identity-engine-policy-objects).

#### Usage Example

##### Request

```bash
curl -v -X GET \
-H "Accept: application/json" \
-H "Content-Type: application/json" \
-H "Authorization: SSWS ${api_token}" \
"https://${yourOktaDomain}/api/v1/policies/${policyId}"
}'
```
##### Response

```json
{
    "id": "rs10xzYOyK9ux6v70g4",
    "name": "Default Policy",
    "type": "Okta:IdpRouting",
    "status": "ACTIVE",
    "default": true,
    "_links": {
        "self": {
            "href": "https://idx.okta1.com/api/v1/policies/rst10xzYOyK9ux6v70g4",
            "hints": {
                "allow": [
                    "GET",
                    "PUT"
                ]
            }
        },
        "rules": {
            "href": "https://idx.okta1.com/api/v1/policies/rst10xzYOyK9ux6v70g4/rules",
            "hints": {
                "allow": [
                    "GET"
                ]
            }
        }
    }
}
```


## Okta Identity Engine Policy Objects

This API uses the following objects:

* [IdP Routing Policy Object](#idp-routing-policy-object)
* [IdP Routing Rule Object](#idp-routing-rule-object)



### IdP Routing Policy Object

This object is meant to determine which IdP end users are routed to. You need to create one IdP Routing Policy object. Additional IdP Routing Policy objects, beyond the first, cannot be created. Currently, only the Okta Identity Provider can be used with the Okta Identity Engine Pipeline, so that, although this object needs to exist, it cannot change the behavior of the pipeline.

| Property | Type   | Description                                                                             |
|----------|--------|-----------------------------------------------------------------------------------------|
| id       | String | Unique identifier for this Policy (read-only)                                           |
| name     | String | Human-readable name for the Policy, configurable during creation or updating.           |
| type     | String | Type of the policy. For IdP Routing Policy objects, this needs to be `Okta:IdpRouting`. |
| status   | String |                                                                                         |

### IdP Routing Object Example

```json
{
    "id": "rs10xzYOyK9ux6v70g4",
    "name": "Default Policy",
    "type": "Okta:IdpRouting",
    "status": "ACTIVE",
    "default": true,
    "_links": {
        "self": {
            "href": "https://idx.okta1.com/api/v1/policies/rst10xzYOyK9ux6v70g4",
            "hints": {
                "allow": [
                    "GET",
                    "PUT"
                ]
            }
        },
        "rules": {
            "href": "https://idx.okta1.com/api/v1/policies/rst10xzYOyK9ux6v70g4/rules",
            "hints": {
                "allow": [
                    "GET"
                ]
            }
        }
    }
}
```

### IdP Routing Rule Object

You need to create at least one IdP Routing Rule Object. Currently, the Okta Identity Provider is the only supported IdP, and the only supported value for the `requirement` property of this rule is:

```json
{
    "idpId": "OKTA",
    "type": "okta_idp"
}
```


| Property | Type | Description |
|----------|------|-------------|
|          |      |             |
|          |      |             |
|          |      |             |
 
### IdP Routing Rule Object

```json
[
    {
        "name": "Catch-all Rule",
        "id": "rul10y0LatXn0HP2p0g4",
        "type": "Okta:IdpRouting",
        "priority": 2,
        "conditions": [],
        "action": "ALLOW",
        "requirement": {
            "idpId": "OKTA",
            "type": "okta_idp"
        },
        "status": "ACTIVE",
        "default": false,
        "_links": {
            "self": {
                "href": "https://idx.okta1.com/api/v1/policies/rst10xzYOyK9ux6v70g4/rules/rul10y0LatXn0HP2p0g4",
                "hints": {
                    "allow": [
                        "GET",
                        "PUT",
                        "DELETE"
                    ]
                }
            },
            "deactivate": {
                "href": "https://idx.okta1.com/api/v1/policies/rst10xzYOyK9ux6v70g4/rules/rul10y0LatXn0HP2p0g4/lifecycle/deactivate",
                "hints": {
                    "allow": [
                        "POST"
                    ]
                }
            },
            "policy": {
                "href": "https://idx.okta1.com/api/v1/policies/rst10xzYOyK9ux6v70g4",
                "hints": {
                    "allow": [
                        "GET",
                        "PUT"
                    ]
                }
            }
        }
    },
    {
        "name": "Default Rule",
        "id": "default-rule",
        "type": "Okta:IdpRouting",
        "priority": -1,
        "conditions": [],
        "action": "DENY",
        "requirement": {},
        "status": "ACTIVE",
        "default": true,
        "_links": {
            "self": {
                "href": "https://idx.okta1.com/api/v1/policies/rst10xzYOyK9ux6v70g4/rules/default-rule",
                "hints": {
                    "allow": [
                        "GET"
                    ]
                }
            },
            "policy": {
                "href": "https://idx.okta1.com/api/v1/policies/rst10xzYOyK9ux6v70g4",
                "hints": {
                    "allow": [
                        "GET",
                        "PUT"
                    ]
                }
            }
        }
    }
]
```