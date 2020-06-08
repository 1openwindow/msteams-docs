---
title: Testing activity feed notifications in Teams
description: JSON  test file activity feed notifications in Teams using Postman
localization_priority:  Normal
author: laujan
ms.author: lajanuar
ms.topic: How-to
keywords: teams activity feed notifications Postman Graph json
---

# Test activity feed notifications Postman collection JSON

>[!NOTE]
> Depending on the user you want to send notification **from** and the **target user/recipient** you may have to update those parameter in the payload.

```json
{
	"info": {
		"_postman_id": "30f23435-b1b0-4a8a-a121-95b21eb8ad76",
		"name": "Activity Feed",
		"schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
	},
	"item": [
		{
			"name": "Authentication",
			"item": [
				{
					"name": "Authenticate in User Context",
					"event": [
						{
							"listen": "test",
							"script": {
								"id": "109e4090-9583-4758-89dd-d6f4110cc515",
								"exec": [
									"pm.test(\"Status Code 200\", () => {",
									"    // Need to validate the request succeeded. ",
									"    pm.response.to.have.status(200);",
									"});",
									"",
									"// Set the user_context access token",
									"const { access_token } = pm.response.json();",
									"pm.environment.set(\"graph_access_token\", access_token);"
								],
								"type": "text/javascript"
							}
						},
						{
							"listen": "prerequest",
							"script": {
								"id": "07539d78-265a-4315-9b7a-6265ba2f5cd5",
								"exec": [
									""
								],
								"type": "text/javascript"
							}
						}
					],
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"name": "Content-Type",
								"type": "text",
								"value": "application/x-www-form-urlencoded"
							}
						],
						"body": {
							"mode": "urlencoded",
							"urlencoded": [
								{
									"key": "grant_type",
									"value": "password",
									"type": "text"
								},
								{
									"key": "username",
									"value": "PattiF@M365x347208.OnMicrosoft.com",
									"type": "text"
								},
								{
									"key": "password",
									"value": "07Apples!",
									"type": "text"
								},
								{
									"key": "client_id",
									"value": "73ee2834-38aa-4077-b4c9-e8e1c36f7e40",
									"type": "text"
								},
								{
									"key": "client_secret",
									"value": "]0ES5E7Sf]J7:fPg[xVr3oKoGjft]D5-",
									"type": "text"
								},
								{
									"key": "scope",
									"value": "https://graph.microsoft.com/.default",
									"type": "text"
								}
							]
						},
						"url": {
							"raw": "https://login.microsoftonline.com/60bcdb18-3e8b-4c8d-8e03-c9bd0a442378/oauth2/v2.0/token",
							"protocol": "https",
							"host": [
								"login",
								"microsoftonline",
								"com"
							],
							"path": [
								"60bcdb18-3e8b-4c8d-8e03-c9bd0a442378",
								"oauth2",
								"v2.0",
								"token"
							]
						},
						"description": "Authenticate with AAD"
					},
					"response": []
				},
				{
					"name": "Authenticate in App Context",
					"event": [
						{
							"listen": "test",
							"script": {
								"id": "3edbf764-64d2-40b2-acc4-dd23c7667642",
								"exec": [
									"pm.test(\"Status Code 200\", () => {",
									"    // Need to validate the request succeeded. ",
									"    pm.response.to.have.status(200);",
									"});",
									"",
									"// Set the user_context access token",
									"const { access_token } = pm.response.json();",
									"pm.environment.set(\"graph_access_token\", access_token);"
								],
								"type": "text/javascript"
							}
						},
						{
							"listen": "prerequest",
							"script": {
								"id": "0176efaf-4dc6-4f93-b6b1-468c571998f8",
								"exec": [
									""
								],
								"type": "text/javascript"
							}
						}
					],
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"name": "Content-Type",
								"type": "text",
								"value": "application/x-www-form-urlencoded"
							}
						],
						"body": {
							"mode": "urlencoded",
							"urlencoded": [
								{
									"key": "grant_type",
									"value": "client_credentials",
									"type": "text"
								},
								{
									"key": "client_id",
									"value": "73ee2834-38aa-4077-b4c9-e8e1c36f7e40",
									"type": "text"
								},
								{
									"key": "client_secret",
									"value": "]0ES5E7Sf]J7:fPg[xVr3oKoGjft]D5-",
									"type": "text"
								},
								{
									"key": "scope",
									"value": "https://graph.microsoft.com/.default",
									"type": "text"
								}
							]
						},
						"url": {
							"raw": "https://login.microsoftonline.com/60bcdb18-3e8b-4c8d-8e03-c9bd0a442378/oauth2/v2.0/token",
							"protocol": "https",
							"host": [
								"login",
								"microsoftonline",
								"com"
							],
							"path": [
								"60bcdb18-3e8b-4c8d-8e03-c9bd0a442378",
								"oauth2",
								"v2.0",
								"token"
							]
						},
						"description": "Authenticate with AAD"
					},
					"response": []
				}
			],
			"protocolProfileBehavior": {}
		},
		{
			"name": "Requests",
			"item": [
				{
					"name": "Send Notification about a Team to a User",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"name": "Content-Type",
								"value": "application/json",
								"type": "text"
							},
							{
								"key": "Authorization",
								"value": "Bearer {{graph_access_token}}",
								"type": "text"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"entityUrl\",\r\n        \"value\": \"https://graph.microsoft.com/teams/416d30d8-484e-4951-9775-f92ca643c91d\"\r\n    },\r\n    \"activityType\": \"taskCreated\",\r\n    \"previewText\": {\r\n    \t\"content\": \"Task Assigned\"\r\n    },\r\n    \"recipient\": {\r\n        \"@odata.type\": \"microsoft.graph.aadUserNotificationRecipient\",\r\n        \"userId\": \"4e485ac3-9c7f-48fa-8711-50b4a47b5722\"\r\n    },\r\n    \"templateParameters\": [\r\n    \t{\r\n\t\t\t\"name\": \"taskId\",\r\n\t\t\t\"value\": \"Task 12322\"\r\n\t\t}\r\n    ]\r\n}"
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/teams/416d30d8-484e-4951-9775-f92ca643c91d/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"teams",
								"416d30d8-484e-4951-9775-f92ca643c91d",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				},
				{
					"name": "Send Notification about a Channel to a User",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"value": "application/json",
								"type": "text"
							},
							{
								"key": "Authorization",
								"value": "Bearer {{graph_access_token}}",
								"type": "text"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"entityUrl\",\r\n        \"value\": \"https://graph.microsoft.com/teams/416d30d8-484e-4951-9775-f92ca643c91d/channels/19:5afb25f3d5054f53814733ef39d951f3@thread.tacv2\"\r\n    },\r\n    \"activityType\": \"taskCreated\",\r\n    \"previewText\": {\r\n    \t\"content\": \"Task Assigned\"\r\n    },\r\n    \"recipient\": {\r\n        \"@odata.type\": \"microsoft.graph.aadUserNotificationRecipient\",\r\n        \"userId\": \"4e485ac3-9c7f-48fa-8711-50b4a47b5722\"\r\n    },\r\n    \"templateParameters\": [\r\n    \t{\r\n\t\t\t\"name\": \"taskId\",\r\n\t\t\t\"value\": \"Task 12322\"\r\n\t\t}\r\n    ]\r\n}"
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/teams/416d30d8-484e-4951-9775-f92ca643c91d/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"teams",
								"416d30d8-484e-4951-9775-f92ca643c91d",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				},
				{
					"name": "Send Notification about a Channel Tab to a User",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"value": "application/json",
								"type": "text"
							},
							{
								"key": "Authorization",
								"value": "Bearer {{graph_access_token}}",
								"type": "text"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"entityUrl\",\r\n        \"value\": \"https://graph.microsoft.com/teams/416d30d8-484e-4951-9775-f92ca643c91d/channels/19:5afb25f3d5054f53814733ef39d951f3@thread.tacv2/tabs/f835f30b-1fcb-4c37-a353-ac7855d3936c\"\r\n    },\r\n    \"activityType\": \"taskCreated\",\r\n    \"previewText\": {\r\n    \t\"content\": \"Task Assigned\"\r\n    },\r\n    \"recipient\": {\r\n        \"@odata.type\": \"microsoft.graph.aadUserNotificationRecipient\",\r\n        \"userId\": \"4e485ac3-9c7f-48fa-8711-50b4a47b5722\"\r\n    },\r\n    \"templateParameters\": [\r\n    \t{\r\n\t\t\t\"name\": \"taskId\",\r\n\t\t\t\"value\": \"Task 12322\"\r\n\t\t}\r\n    ]\r\n}"
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/teams/416d30d8-484e-4951-9775-f92ca643c91d/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"teams",
								"416d30d8-484e-4951-9775-f92ca643c91d",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				},
				{
					"name": "Send Notification about a Channel Message to a User",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"value": "application/json",
								"type": "text"
							},
							{
								"key": "Authorization",
								"value": "Bearer {{graph_access_token}}",
								"type": "text"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"entityUrl\",\r\n        \"value\": \"https://graph.microsoft.com/teams/416d30d8-484e-4951-9775-f92ca643c91d/channels/19:5afb25f3d5054f53814733ef39d951f3@thread.tacv2/messages/1584741801947\"\r\n    },\r\n    \"activityType\": \"taskCreated\",\r\n    \"previewText\": {\r\n    \t\"content\": \"Task Assigned\"\r\n    },\r\n    \"recipient\": {\r\n        \"@odata.type\": \"microsoft.graph.aadUserNotificationRecipient\",\r\n        \"userId\": \"4e485ac3-9c7f-48fa-8711-50b4a47b5722\"\r\n    },\r\n    \"templateParameters\": [\r\n    \t{\r\n\t\t\t\"name\": \"taskId\",\r\n\t\t\t\"value\": \"Task 12322\"\r\n\t\t}\r\n    ]\r\n}"
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/teams/416d30d8-484e-4951-9775-f92ca643c91d/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"teams",
								"416d30d8-484e-4951-9775-f92ca643c91d",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				},
				{
					"name": "Send Notification about a Chat to a User",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"value": "application/json",
								"type": "text"
							},
							{
								"key": "Authorization",
								"value": "Bearer {{graph_access_token}}",
								"type": "text"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"entityUrl\",\r\n        \"value\": \"https://graph.microsoft.com/chats/19:4e485ac3-9c7f-48fa-8711-50b4a47b5722_8e4d61a1-60d5-4f11-8887-c0b09b5e0f7e@unq.gbl.spaces\"\r\n    },\r\n    \"activityType\": \"taskCreated\",\r\n    \"previewText\": {\r\n    \t\"content\": \"Task Assigned\"\r\n    },\r\n    \"recipient\": {\r\n        \"@odata.type\": \"microsoft.graph.aadUserNotificationRecipient\",\r\n        \"userId\": \"4e485ac3-9c7f-48fa-8711-50b4a47b5722\"\r\n    },\r\n    \"templateParameters\": [\r\n    \t{\r\n\t\t\t\"name\": \"taskId\",\r\n\t\t\t\"value\": \"Task 12322\"\r\n\t\t}\r\n    ]\r\n}"
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/chats/19:4e485ac3-9c7f-48fa-8711-50b4a47b5722_8e4d61a1-60d5-4f11-8887-c0b09b5e0f7e@unq.gbl.spaces/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"chats",
								"19:4e485ac3-9c7f-48fa-8711-50b4a47b5722_8e4d61a1-60d5-4f11-8887-c0b09b5e0f7e@unq.gbl.spaces",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				},
				{
					"name": "Send Notification about a Chat Message to a User",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"value": "application/json",
								"type": "text"
							},
							{
								"key": "Authorization",
								"value": "Bearer {{graph_access_token}}",
								"type": "text"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"entityUrl\",\r\n        \"value\": \"https://graph.microsoft.com/chats/19:4e485ac3-9c7f-48fa-8711-50b4a47b5722_8e4d61a1-60d5-4f11-8887-c0b09b5e0f7e@unq.gbl.spaces/messages/1584662866715\"\r\n    },\r\n    \"activityType\": \"taskCreated\",\r\n    \"previewText\": {\r\n    \t\"content\": \"Task Assigned\"\r\n    },\r\n    \"recipient\": {\r\n        \"@odata.type\": \"microsoft.graph.aadUserNotificationRecipient\",\r\n        \"userId\": \"4e485ac3-9c7f-48fa-8711-50b4a47b5722\"\r\n    },\r\n    \"templateParameters\": [\r\n    \t{\r\n\t\t\t\"name\": \"taskId\",\r\n\t\t\t\"value\": \"Task 12322\"\r\n\t\t}\r\n    ]\r\n}"
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/chats/19:4e485ac3-9c7f-48fa-8711-50b4a47b5722_8e4d61a1-60d5-4f11-8887-c0b09b5e0f7e@unq.gbl.spaces/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"chats",
								"19:4e485ac3-9c7f-48fa-8711-50b4a47b5722_8e4d61a1-60d5-4f11-8887-c0b09b5e0f7e@unq.gbl.spaces",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				},
				{
					"name": "Send a Custom Notification to a User",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"value": "application/json",
								"type": "text"
							},
							{
								"key": "Authorization",
								"value": "Bearer {{graph_access_token}}",
								"type": "text"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"text\",\r\n        \"value\": \"Just some custom text.\",\r\n        \"webUrl\": \"https://teams.microsoft.com/l/team/19%3A2fb10ab94fab4b048063e3e3951787a2%40thread.tacv2/conversations?groupId=416d30d8-484e-4951-9775-f92ca643c91d&tenantId=60bcdb18-3e8b-4c8d-8e03-c9bd0a442378\"\r\n    },\r\n    \"activityType\": \"taskCreated\",\r\n    \"previewText\": {\r\n    \t\"content\": \"Task Assigned\"\r\n    },\r\n    \"recipient\": {\r\n        \"@odata.type\": \"microsoft.graph.aadUserNotificationRecipient\",\r\n        \"userId\": \"4e485ac3-9c7f-48fa-8711-50b4a47b5722\"\r\n    },\r\n    \"templateParameters\": [\r\n    \t{\r\n\t\t\t\"name\": \"taskId\",\r\n\t\t\t\"value\": \"Task 12322\"\r\n\t\t}\r\n    ]\r\n}"
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/teams/416d30d8-484e-4951-9775-f92ca643c91d/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"teams",
								"416d30d8-484e-4951-9775-f92ca643c91d",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				},
				{
					"name": "Send Notification about a Personal App to a User",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"value": "application/json",
								"type": "text"
							},
							{
								"key": "Authorization",
								"value": "Bearer {{graph_access_token}}",
								"type": "text"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"entityUrl\",\r\n        \"value\": \"https://graph.microsoft.com/beta/users/9aa87a7a-a2f4-4803-92e5-08c887b7d740/teamwork/installedApps/2bc14f92-d8ec-4843-84e2-d95e6f33c1c9\"\r\n    },\r\n    \"activityType\": \"taskCreated\",\r\n    \"previewText\": {\r\n    \t\"content\": \"Task Assigned\"\r\n    },\r\n    \"templateParameters\": [\r\n    \t{\r\n\t\t\t\"name\": \"taskId\",\r\n\t\t\t\"value\": \"Task 12322\"\r\n\t\t}\r\n    ]\r\n}",
							"options": {
								"raw": {
									"language": "json"
								}
							}
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/users/9aa87a7a-a2f4-4803-92e5-08c887b7d740/teamwork/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"users",
								"9aa87a7a-a2f4-4803-92e5-08c887b7d740",
								"teamwork",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				},
				{
					"name": "Send Mention to User",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"name": "Content-Type",
								"type": "text",
								"value": "application/json"
							},
							{
								"key": "Authorization",
								"type": "text",
								"value": "Bearer {{graph_access_token}}"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"entityUrl\",\r\n        \"value\": \"https://graph.microsoft.com/teams/416d30d8-484e-4951-9775-f92ca643c91d\"\r\n    },\r\n    \"activityType\": \"userMention\",\r\n    \"previewText\": {\r\n    \t\"content\": \"User Mention\"\r\n    },\r\n    \"recipient\": {\r\n        \"@odata.type\": \"microsoft.graph.aadUserNotificationRecipient\",\r\n        \"userId\": \"4e485ac3-9c7f-48fa-8711-50b4a47b5722\"\r\n    },\r\n    \"templateParameters\": []\r\n}"
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/teams/416d30d8-484e-4951-9775-f92ca643c91d/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"teams",
								"416d30d8-484e-4951-9775-f92ca643c91d",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				},
				{
					"name": "Send Mention to Team",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"name": "Content-Type",
								"type": "text",
								"value": "application/json"
							},
							{
								"key": "Authorization",
								"type": "text",
								"value": "Bearer {{graph_access_token}}"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"entityUrl\",\r\n        \"value\": \"https://graph.microsoft.com/teams/416d30d8-484e-4951-9775-f92ca643c91d\"\r\n    },\r\n    \"activityType\": \"teamMention\",\r\n    \"previewText\": {\r\n    \t\"content\": \"Team Mention\"\r\n    },\r\n    \"recipient\": {\r\n        \"@odata.type\": \"microsoft.graph.aadUserNotificationRecipient\",\r\n        \"userId\": \"4e485ac3-9c7f-48fa-8711-50b4a47b5722\"\r\n    },\r\n    \"templateParameters\": []\r\n}"
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/teams/416d30d8-484e-4951-9775-f92ca643c91d/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"teams",
								"416d30d8-484e-4951-9775-f92ca643c91d",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				},
				{
					"name": "Send Mention to Channel",
					"request": {
						"method": "POST",
						"header": [
							{
								"key": "Content-Type",
								"name": "Content-Type",
								"type": "text",
								"value": "application/json"
							},
							{
								"key": "Authorization",
								"type": "text",
								"value": "Bearer {{graph_access_token}}"
							}
						],
						"body": {
							"mode": "raw",
							"raw": "{\r\n    \"topic\": {\r\n        \"source\": \"entityUrl\",\r\n        \"value\": \"https://graph.microsoft.com/teams/416d30d8-484e-4951-9775-f92ca643c91d/channels/19:5afb25f3d5054f53814733ef39d951f3@thread.tacv2\"\r\n    },\r\n    \"activityType\": \"channelMention\",\r\n    \"previewText\": {\r\n    \t\"content\": \"Channel Mention\"\r\n    },\r\n    \"recipient\": {\r\n        \"@odata.type\": \"microsoft.graph.aadUserNotificationRecipient\",\r\n        \"userId\": \"4e485ac3-9c7f-48fa-8711-50b4a47b5722\"\r\n    },\r\n    \"templateParameters\": []\r\n}"
						},
						"url": {
							"raw": "https://graph.microsoft.com/beta/teams/416d30d8-484e-4951-9775-f92ca643c91d/sendActivityNotification",
							"protocol": "https",
							"host": [
								"graph",
								"microsoft",
								"com"
							],
							"path": [
								"beta",
								"teams",
								"416d30d8-484e-4951-9775-f92ca643c91d",
								"sendActivityNotification"
							]
						}
					},
					"response": []
				}
			],
			"description": "Use the User Context or App Context Authentication calls as appropriate.",
			"event": [
				{
					"listen": "prerequest",
					"script": {
						"id": "2d98339b-8f99-4dbf-9b4d-2091b9097ddd",
						"type": "text/javascript",
						"exec": [
							""
						]
					}
				},
				{
					"listen": "test",
					"script": {
						"id": "ed1eb74e-39ea-46d9-8148-a71acb8ded04",
						"type": "text/javascript",
						"exec": [
							""
						]
					}
				}
			],
			"protocolProfileBehavior": {}
		}
	],
	"protocolProfileBehavior": {}
}
```
