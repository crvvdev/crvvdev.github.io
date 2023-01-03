---
title: "[AntiCheat] XIGNCODE Reversing -- Usermode hooks"
last_modified_at: 2023-01-03T00:00:00-00:00
categories:
  - anticheat reversing
tags:
  - anticheat
  - reversing
  - hooks
---

I firstly wanna note that this was originally published by me on my old blog back in January 2020. Essentially the information provided here still the same.

-----

Hello, today i was improving my hack and i thought with myself, well it's been a long time since i last checked the hooks that this AC does. 

And here's the conclusion:

![img](/assets/images/post_xigncode_reversing/1.png)

Usual hooks, but now they hook:

`kernel32.dll!ReadProcessMemory` 

`kernel32.dll!WriteProcessMemory`

`user32.dll!MessageBoxA/W`

Analyzing the hooks:

![img](/assets/images/post_xigncode_reversing/2.png)

As you can see, there's a `_ReturnAddress` check going on, passing it as a parameter and calling it at `call 0D62244B`.

The function that checks it:

![img](/assets/images/post_xigncode_reversing/3.png)

It's a little bad too actually analizy just by using assembly code so i just dumped and decompiled it in IDA for better sake of viewing.

![img](/assets/images/post_xigncode_reversing/4.png)

I also took a bit deeper view at what they do with those pointers and i came to a conclusion that they store the Return Addresses in some kind of list.

Basically, you call it from a not signed memory region, they return a error to x3.xem (manually mapped copy runing into game process memory) and they analyze or just ban or either kick you from the game.