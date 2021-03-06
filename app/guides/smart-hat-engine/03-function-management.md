---
layout: twoColumn
section: guides
type: Smart HAT Engine
guide: 
    name: smart-hat-engine
    step: 03-function-management
title: Function management
description: How function management is controlled using HAT APIs
product: she
weight: 1
---

All SHE function management is performed through the endpoints to list, setup and disable the apps as well as in certain cases - get the application token for the frontend to use in authenticating with a remote service.


### Listing functions

Applications are listed at `/api/v2.6/she/function` - returns the full list of available functions

This method is the only one needed to call to get a comprehensive list of functions along with their status on the HAT (available, enabled, execution time).

An individual function information is accessible at `/api/v2.6/she/function/:function-id` but this shouldn’t be needed in most cases. It will have exactly the same information and format as a single item in the list returned by `/api/v2.6/she/function`.

### Setting up

Function is set up by calling GET `/api/v2.6/she/function/:function-id/enable`

The steps of setting up a function with a HAT happen transparently after calling the `enable` endpoint.

Similarly a function gets disabled by calling `/api/v2.6/she/function/:function-id/disable`. This takes care of recording the fact on the HAT, disabling any HAT access and suspending future function invocations.

### Executing

Most functions are expected to be executed automatically by the HAT, however it is still possible (with "owner" HAT permissions) to execute a function manually by calling GET `/api/v2.6/she/function/:function-id/trigger`

### Function availability

It is important to note that only SHE functions that have been registered with a HAT cluster will be available to use.

It is achieved in the HAT's configuration (`application.conf`) and therefore new functions currently require the HAT to be redeployed with them included in the configuraiton:

```
she {
  functions = [
    {
      id = "data-feed-counter"
      version = "1.0.0"
      baseUrl = "https://ociflwukh1.execute-api.eu-west-1.amazonaws.com/dev"
      namespace = "she"
      endpoint = "insights/activity-records"
    }
    {
      id = "sentiment-tracker"
      version = "1.0.0"
      baseUrl = "https://ociflwukh1.execute-api.eu-west-1.amazonaws.com/dev"
      namespace = "she"
      endpoint = "insights/emotions"
    }
  ]
}
```

The format of the configuration should be self-explanatory: the configuration provides a list of functions, each identified by an ID, the version to be used, `baseUrl` as the address of the API gateway, and finally - `namespace` and `endpoint` it is allowed to create the data in. The rest is done automatically by the HAT, including loading the full configuration, issuing the calls, saving the data, etc.


<nav class="pager-nav">
	<a href="02-function-testing.html">Testing your function</a>
	<a href="https://documenter.getpostman.com/view/110376/RWTiveMe">API Reference</a>
</nav>
