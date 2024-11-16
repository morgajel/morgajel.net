---
author: Jesse Morgan
categories:
- Uncategorized
date: "2020-03-07T16:33:18Z"
guid: https://morgajel.net/?p=1815
id: 1815
title: DD Blocksize
url: /2020/03/07/1815
---

I’ve been working with Linux for 20 years, and dd has always been that dangerous tool that makes me nervous to use. While trying to burn a series of SD cards through a USB adapter, I decided to performance test various Block Size (BS) settings, and figured I’d share the results.

The following transfer rates were the result of copying the RancherOS from my local SSD to a microcenter XC 64GB SD card using a USB adapter.

<figure class="wp-block-table is-style-stripes">| Block Size | Image Size | Image Name | Transfer Rate |
|---|---|---|---|
| 2.0K | 2.1G | build/rancheros-raspberry-pi64.img | 11.1 MB/s |
| 4.0K | 2.1G | build/rancheros-raspberry-pi64.img | 11.4 MB/s |
| 8.0K | 2.1G | build/rancheros-raspberry-pi64.img | 11.5 MB/s |
| 16K | 2.1G | build/rancheros-raspberry-pi64.img | 11.5 MB/s |
| 32K | 2.1G | build/rancheros-raspberry-pi64.img | 10.3 MB/s |
| 64K | 2.1G | build/rancheros-raspberry-pi64.img | 11.2 MB/s |
| 128K | 2.1G | build/rancheros-raspberry-pi64.img | 10.5 MB/s |
| 256K | 2.1G | build/rancheros-raspberry-pi64.img | 10.6 MB/s |
| 512K | 2.1G | build/rancheros-raspberry-pi64.img | 12.0 MB/s |
| 1.0M | 2.1G | build/rancheros-raspberry-pi64.img | 9.8 MB/s |
| 2.0M | 2.1G | build/rancheros-raspberry-pi64.img | 11.5 MB/s |
| 4.0M | 2.1G | build/rancheros-raspberry-pi64.img | 11.1 MB/s |
| 8.0M | 2.1G | build/rancheros-raspberry-pi64.img | 11.0 MB/s |
| 16M | 2.1G | build/rancheros-raspberry-pi64.img | 11.0 MB/s |
| 32M | 2.1G | build/rancheros-raspberry-pi64.img | 11.0 MB/s |
| 64M | 2.1G | build/rancheros-raspberry-pi64.img | 11.0 MB/s |

</figure>As you can see, at that size with this configuration, it really doens’t matter much.