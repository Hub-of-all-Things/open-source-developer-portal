---
layout: twoColumn
section: "Linked Data"
type: article
title:  "Save an example nudge"
description: "Save an example nudge"
---

## POST - Save an example nudge 
   
`https://testing.hubat.net/api/v2.6/data/rumpel/nudge`

An example of a nudge created separately, but to be attached to a note

### Headers

#### Content-Type
application/json
#### X-Auth-Token
{{accessToken}}

###Body 

```

{
    "type": "time",
    "nudge": "Share APIs with the world",
    "time": "2017-04-30T14:21:52+01:00"
}


```

```postman

"request": {
						"url": "https://{{hat}}/api/v2.6/data/rumpel/nudge",
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"value": "application/json",
								"description": ""
							},
							{
								"key": "X-Auth-Token",
								"value": "{{accessToken}}",
								"description": ""
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\n    \"type\": \"time\",\n    \"nudge\": \"Share APIs with the world\",\n    \"time\": \"2017-04-30T14:21:52+01:00\"\n}"
						},
						"description": "An example of a nudge created separately, but to be attached to a note"
					}

```
