---
layout: post
category : Asar
tags : [Asar, C++]
comments: true
---
{% include JB/setup %}

[Asar](http://smwc.me/s/14560) is a SNES Assembler made by Alcaro.

This assembler has issue about memory leak.

So, I made some test codes for Asar v1.50.

However, it failed even basic tests.

For example:

```c++
// Check asar_patch function.
// The path of a file that doesn't exist in FP is set.
TEST(if_patch, input_missing)
{
	CHECK_FALSE(asar_patch(FP, data, dsize, &len));
}
```

This abnormally ends with a Segmentation fault.

I uploaded the codes I wrote to [here](https://github.com/boldowa/Asar)

It's difficult to create tests because this program isn't object oriented.

If I have time I would like to review the program design.

