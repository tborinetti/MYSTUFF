---
tags:
  - Programming
---

Will likely need to be running on 4 different threads:
- One for Receiving 
- One for sending
- One for updating UI 
- One for server connection

Not sure if send/recv can be done on the same socket 
Also not sure if UpdatingUI/server connection can be done by just pinging the server every 5 seconds or so. 
