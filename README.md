## Important Notice

This public repository is read-only and no longer maintained.

![](https://img.shields.io/badge/STATUS-NOT%20CURRENTLY%20MAINTAINED-red.svg?longCache=true&style=flat)

---

# SAP Customer Data Platform Connectors Hub

[![REUSE status](https://api.reuse.software/badge/github.com/SAP/customer-data-platform-connectors)](https://api.reuse.software/info/github.com/SAP/customer-data-platform-connectors)

## Description
We're using the connectors to connect the CDP to the external world. Either receiving or pushing external data into the CDP, or sending various types of data from CDP to another platform. Here we want to share our knowledge on how to do that or maybe get the external point of view on how to improve that or add the missing feature

## Specification
Our connector definition is based on OpenAPI specification. You can use swagger tools for implementing the specification

## Download and Installation
None

## Usage

- **Terms**
	
	- **Event** - pushing data to CDP
		- Webhook	
		- Schedual event
	-  **Action** - Sending data from CDP
		- Journey (real time)
		- Audience (batch - offline)
	- **Mapping** - relations between request or response schema to CDP entities (Profile, Action etc...)



- **Action Example**
	In the root element, add predefineAction element to define all the actions
		

	```
		"preDefinedActions":[
		      {
		         "resourcePath":"/import/post",
		         "name":"Create and update subscribers in batch",
				 "configValues":{
					"subscribe": true
				 }
		      },
			  {
		         "resourcePath":"/unsubscribe/post",
		         "name":"Update unsubscribes",
				 "configValues":{
					"subscribe": false
				 }
		      }
		   ]
	```

	Then refer to the action definition when you describe the action
	```
		"/import":{
            "post":{
               "tags":[
                  "Actions"
               ],
               "summary":"Import subscribers",
               "description":"Import subscribers",
               "operationId":"ImportSubscribers",
               "x-cdp-operation-settings":{
                   "rateLimitConfig": {
                        "type": "RPS",
                        "rateLimitParameters": {
                        "ratePerSecond": 100,
                        "errorCodes": [ 606, 607 ],
                        "delayAfterRateLimitInSecond": 300
                    }
                    }
               },
               "requestBody":{
                  "required":true,
                  "content":{
                     "application/json":{
                        "schema":{
						   "$ref":"#/components/schemas/ImportSubscriberRequest"	                           
                        }
                     }
                  }
               },
               "responses":{
                  "200":{
                     "description":"Result status",
                     "content":{
                        "application/json":{
                           "schema":{
                              "$ref":"#/components/schemas/ImportSubscribersResponse"
                           }
                        }
                     }
                  }
               }
            }
         }
	``` 
- **Events Example**
	- **Webhook**
   
		In the root element, add preDefinedEventListeners element to define all the webhooks
		```
		 "preDefinedEventListeners": [
			 {
			  "webhookPath": "intercomEvent/post",
			  "recordsLocator": "$",
			  "name": "Get contacts and leads in real-time"
			}
		  ]
		```
		Then refer to this definition when you describe the webhook
		```
		"webhooks": {
				"intercomEvent": {
			  "post": {
				"summary": "",
				"responses": {
				  "200": {
					"description": "Return a 200 status to indicate that the webhook was received successfully"
				  }
				},
				"requestBody": {
				  "content": {
					"application/json": {
					  "schema": {
						"type": "object",
						"properties": {
						  "data": {
							"type": "object",
							"properties": {
							  "item": {
								"type": "object",
								"properties": {
								  "email": {
									"type": "string"
								  },
								  "name": {
									"type": "string"
								  },
								  "id": {
									"type": "string"
								  },
								  "phone": {
									"type": "string"
								  },
								  "created_at": {
									"type": "string",
									"format": "date-time"
								  }
								}
							  }
							}
						  },
						  "topic": {
							"type": "string"
						  },
						  "id": {
							"type": "string"
						  },
						  "created_at": {
							"type": "integer"
						  }
						}
					  }
					}
				  }
				}
			  }
			}
		  }
		```
	- **Event Example**

		In the root element, add preDefinedEvents element to define all the webhooks
		```
		"preDefinedEvents":[
			{
		   "resourcePath":"/subscribers/get",
		   "name":"Get subscribers"
			}
		]
		```
		And  then refer to this definition when you describe the event
		```
		"/subscribers": {
		  "get": {
			"tags": [
			  "Events"
			],
			"summary": "Load of subscribers",
			"description": "Load of subscribers",
			"operationId": "LoadSubscribers",
			"responses": {
			  "200": {
				"description": "Successful operation",
				"content": {
				  "application/json": {
					"schema": {
					  "type": "object",
					  "x-cdp-schema-settings": {
						"recordsLocatorPath": "Results"
					  },
					  "properties": {
						"Results": {
						  "type": "array",
						  "items": {
							"$ref": "#/components/schemas/Subscriber"
						  }
						}
					  }
					}
				  }
				}
			  }
			}
		  }
		}
		```

## Limitations
None

## Known Issues
None

## How to obtain support
Via SAP standard support.
https://developers.gigya.com/display/GD/Opening+A+Support+Incident

## Contributing
Via pull request to this repository.

## To-Do (upcoming changes)
None

## Licensing
Please see our [LICENSE](https://github.com/SAP/customer-data-platform-connectors/blob/main/LICENSES/Apache-2.0.txt) for copyright and license information. Detailed information including third-party components and their licensing/copyright information is available [via the REUSE tool](https://api.reuse.software/info/github.com/SAP/customer-data-platform-connectors)
