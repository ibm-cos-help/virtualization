---
copyright:
  years: 2014, 2018
lastupdated: "2018-01-22"
---
{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Giving containers access to the private network through PIM

Giving containers access to your private network is easy to do, but you need to understand the implications in providing access to your internal infrastructure to your customers. This can lead to security issues.

1. Go to the container that you want to grant private network access.
2. Click the **Network** tab and click **Configure**.
3. You can now setup the Bridged Network Section:
  * **Bridged Network**: Select the check box.
  * **Connect to**: Is not required to provide access to the {{site.data.keyword.cloud}} private network.
  * **Get IP Address by DHCP**: Clear this check box.
  * **IP Address / Subnet Mask**: Choose an INTERNAL IP address from your pool of private addresses.
  * Click **Submit**
  
You now need to refresh the page from the link on the upper right and you are done.
