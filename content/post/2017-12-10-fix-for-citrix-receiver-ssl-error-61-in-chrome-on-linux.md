---
author: Jesse Morgan
categories:
- Uncategorized
date: "2017-12-10T12:27:35Z"
guid: http://morgajel.net/?p=1758
id: 1758
title: Fix for Citrix Receiver SSL Error 61 in Chrome on Linux
url: /2017/12/10/1758
---

Found this [**here**](https://geekpete.com/blog/ssl-error-61-using-citrix-ica-client-on-linux/), which fortunately fixed my issue with 3 lines:

```
sudo mv /opt/Citrix/ICAClient/keystore/cacerts /opt/Citrix/ICAClient/keystore/cacerts_old
sudo cp /opt/Citrix/ICAClient/keystore/cacerts_old/* /usr/share/ca-certificates/mozilla/
sudo ln -s /usr/share/ca-certificates/mozilla /opt/Citrix/ICAClient/keystore/cacerts
```