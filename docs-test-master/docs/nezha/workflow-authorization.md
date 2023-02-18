---
id: workflow-authorization
title: Authorization workflow
sidebar_label: Authorization workflow
---
import Mermaid from '@theme/Mermaid'

## Overview

### Handle `auth` action

This diagram represents how native side handles `auth` action.

<Mermaid chart={`
  graph TD
    start[/Start/]
    receive_auth[Receive \`auth\` action]
    is_system_perm{System permission?}
    is_granted{Already granted?}
    request_system_perm[Request system permission]
    is_system_granted{Granted?}
    check_custom_perm_status[Check custom permission status]
    is_first_time_request{First time?}
    custom_perm_granted{Already granted?}
    request_custom_perm[Request custom permission]
    is_user_accepted{User accepted?}
    send_callback_success[Send callback with success]
    send_callback_error[Send callback with error]
    finish_permission_denied[/Finish - permission denied/]
    finish_permission_granted[/Finish - permission granted/]
    start --> receive_auth
    receive_auth --> is_system_perm
    is_system_perm --> |Yes| is_granted
    is_system_perm --> |No| check_custom_perm_status
    is_granted --> |No| request_system_perm
    is_granted --> |Yes| check_custom_perm_status
    request_system_perm --> is_system_granted
    is_system_granted --> |No| send_callback_error
    is_system_granted --> |Yes| check_custom_perm_status
    check_custom_perm_status --> is_first_time_request
    is_first_time_request --> |Yes| request_custom_perm
    is_first_time_request --> |No| custom_perm_granted
    custom_perm_granted --> |No| send_callback_error
    custom_perm_granted --> |Yes| send_callback_success
    request_custom_perm --> is_user_accepted
    is_user_accepted --> |No| send_callback_error
    is_user_accepted --> |Yes| send_callback_success
    send_callback_success --> finish_permission_granted
    send_callback_error --> finish_permission_denied
`}></Mermaid>



### Check permission during action call

This diagram represents how native side checks permission for the any action received by native side.

<Mermaid chart={`
graph TD
  start[/Start/]
  receive_action[Recive any action]
  is_perm_required{Permission required?}
  is_perm_granted{Permission granted?}
  is_system_perm{System permission?}
  is_system_perm_granted{Permission granted?}
  do_action[Do action]
  is_success{Success?}
  send_callback_result[Send callback with result]
  send_callback_error[Send callback with error]
  send_callback_no_permission[Send callback with error - no permission]
  finish_success[/Finish success/]
  finish_failure[/Finish failure/]
  start --> receive_action
  receive_action --> is_perm_required
  is_perm_required --> |Yes| is_perm_granted
  is_perm_granted --> |No| send_callback_no_permission
  is_perm_granted --> |Yes| is_system_perm
  is_system_perm_granted --> |No| send_callback_no_permission
  is_system_perm_granted --> |Yes| do_action
  is_system_perm --> |No| do_action
  is_system_perm --> |Yes| is_system_perm_granted
  is_perm_required --> |No| do_action
  do_action --> is_success
  is_success --> |Yes| send_callback_result
  is_success --> |No| send_callback_error
  send_callback_no_permission --> finish_failure
  send_callback_error --> finish_failure
  send_callback_result --> finish_success
`}></Mermaid>


Notes:
- All permissions should be handled by native side.
- Permissions are saved locally on native side in encrypted storage.
- Each mini program has own permission storage.
- When mini program is uninstalled permission storage for the app should be cleared.
- Mini program should provide meaningful description for authorization request (`authorize` action, `description` field in payload).

## List of permissions

### System permissions

- `camera` - device camera, required for [Camera native component](./native-component-camera).
- `userLocation` - user location data, this permission is required for all [location actions](./rpc-action#location) and all location events.

### App permissions (non-system permissions)


## Permissions authorization actions

- [`authorize`](./rpc-action#authorize) - request user authorization for permission
- [`getGetting`](./rpc-action#get-setting) - show all settings, including auth settings
- [`openSetting`](./rpc-action#open-setting) - open Setting panel
