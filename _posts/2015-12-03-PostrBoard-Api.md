---
title:  "PostrBoard API and JSON setup"
date:   2015-12-03 9:00:00
description: PostrBoard API and JSON
---
<script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>

This is only to document the current PostrBoard API and JSON route setup.

#Event

* /api/events {get,post}
    - get users.requiresLogin, events.list
    - post users.requiresLogin, users.hasAdminAuthorization, upload.uploadSingle, events.create
* /api/createdEvents {get}
    - get users.requiresLogin, users.hasAdminAuthorization, events.listCreatedEvents
* /api/createdEvents/:organizationId {get}
    - get users.requiresLogin, users.hasAdminAuthorization, events.listCreatedEvents
* /api/events/:eventId/registered {get}
    - get users.requiresLogin, events.hasAuthorization, events.listRegistered
* /api/events/organization/:organizationId {get}
    - get events.listByOrganization
* /api/events/:eventId {get,put,delete}
    - get events.read
    - put users.requiresLogin, events.hasAuthorization, users.hasAdminAuthorization, upload.uploadSingle, events.update
    - delete users.requiresLogin, events.hasAuthorization, events.delete
* /api/eventRegister/:eventId {get,post,delete}
    - get users.requiresLogin, events.getStatus
    - post users.requiresLogin, events.register
    - delete users.requiresLogin, events.deregister
* /api/eventsRegister {get}
    - get users.requiresLogin, events.listRegisteredEvents
* /api/eventRegisterVolunteer/:eventId {get,post,delete}
    - get users.requiresLogin, events.volunteerStatus
    - post users.requiresLogin, events.registerVolunteer
    - delete users.requiresLogin, events.deregisterVolunteer

#Organization



#User

#Schedules

#
