---
title: "[Programming] Detecting Manually Mapped (Unsigned) modules"
last_modified_at: 2023-01-03T00:00:00-00:00
categories:
  - programming
tags:
  - anticheat
  - manual map
  - programming
---

I firstly wanna note that this was originally published by me on my old blog back in January 2020. Essentially the information provided here still the same.

-----

Hello, i wanna show the reader a easy way to detect modules that where not loaded by the windows native loader.

The API we gonna use is `VirtualQuery`.

![img](/assets/images/post_detect_mapped_images/1.png)

We will initiate by utilizing this API and querying all the memory until it fails.

![img](/assets/images/post_detect_mapped_images/2.png)

We also check if the memory is either `EXECUTE_READ` or `EXECUTE_READWRITE`.

![img](/assets/images/post_detect_mapped_images/3.png)

 Here's a small check to skip some Windows Native DLLs, which is a common case on WoW64.

One thing to notice here, you may have problems with Antivirus mapped .dlls, sometimes you gonna just find a unknown module and either can or cannot be a legit module (manual analyze that i guess?).

![img](/assets/images/post_detect_mapped_images/4.png)

 That is simple, a simplier version of `GetModuleHandleEx`, if no module is found then the function fails.

And there you go, the final thing:

![img](/assets/images/post_detect_mapped_images/5.png)

With the above snippet you're able to detect unsigned memory on the current scanning process context, be aware that it might raise false positives.