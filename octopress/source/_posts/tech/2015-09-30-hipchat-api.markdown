---
layout: post_tech
title: "Hipchat API"
date: 2015-09-30 15:09:22 +0800
comments: true
categories: [tech]
tags: [hipchat, api] 
---

## 1. Hipchat API

```
ROOM_NOTIFICATION_URL = "https://%(server)s/v2/room/%(room_id)s/notification?auth_token=%(token)s    "
ROOM_TOPIC_URL = "https://%(server)s/v2/room/%(room_id)s/topic?auth_token=%(token)s"
PRIVATE_MESSAGE_URL = "https://%(server)s/v2/user/%(user_id)s/message?auth_token=%(token)s"
SET_TOPIC_URL = "https://%(server)s/v2/room/%(room_id)s/topic?auth_token=%(token)s"
USER_DETAILS_URL = "https://%(server)s/v2/user/%(user_id)s?auth_token=%(token)s"
ALL_USERS_URL = "https://%(server)s/v2/user?auth_token=%(token)s&start-index=%(start_index)s"
```

## 2. Command Line

```bash
$ curl -d '{"color": "green", "message": "message", "notify": false, "messae_format": "text"}' -H 'Content-Type: application/json' https://api.hipchat.com/v2/room/room id/scope_name(message/notification)?auth_token=specific_token
```

## Reference

- [Hipchat API Docs](https://www.hipchat.com/docs/apiv2)
