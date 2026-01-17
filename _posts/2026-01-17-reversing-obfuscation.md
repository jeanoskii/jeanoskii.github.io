---
layout: post
title: Reversing an Attempt at Obfuscation
date: 2026-01-17 00:00:00
description: In other words, reverse engineering
tags: obfuscation java static-analysis
categories: ethical-hacking reverse-engineering retrospective
code_diff: true
---

In 2015, during a random work day, I was talking with a good friend of mine about programming. We were figuring out which one of us is better, by exchanging challenges back and forth about software engineering. I defeated all his challenges, but he then brought out his hardest one yet. It wasn't about engineering anymore, but the opposite. He sent me a Java file and told me to describe to him its logic. Here's the code that he sent me and I'll let you guess what is happening:

```
package com.cerberus.core;

import java.math.BigInteger;
import android.support.v4.view.MotionEventCompat;

public class CerberusUtil {
    public static final String APP = rev3753("/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:1/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:0/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:0");
    public static final String DEV = rev3753("/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:9/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:0/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:8/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:0/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:1/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:0/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:8/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:9/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:1");
    public static final String PACKAGE = rev3753("/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-/-/-/-/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5");

    public static String bytesToHex(byte[] bArray) {
        byte[] bArray1 = bArray;
        char[] chArray = new char[(bArray1.length * 2)];
        for (int i = 0; i < bArray1.length; i++) {
            int j = bArray1[i] & MotionEventCompat.ACTION_MASK;
            chArray[i * 2] = hexArray[j >>> 4];
            chArray[(i * 2) + 1] = hexArray[j & 15];
        }
        String s = r11;
        String s2 = new String(chArray);
        return s;
    }

    public static final String rev3753(String s) {
        String decode = decode(decode(decode(decode(decode(decode(decode(s.replace("?", "4").replace("%", "f").replace(":", "3").replace("-", "d").replace("/", "2")).replace("-", "3").replace("/", "6")))).replace("-", "3").replace("/", "6"))));
        return r0;
    }

    public static final String rev3854(String s) {
        BigInteger bigInteger1 = r13;
        BigInteger bigInteger2 = new BigInteger(s.getBytes());
        String bigInteger3 = bigInteger1.toString(16);
        bigInteger1 = r13;
        bigInteger2 = new BigInteger(bigInteger3.getBytes());
        String bigInteger4 = bigInteger1.toString(16);
        bigInteger1 = r13;
        bigInteger2 = new BigInteger(bigInteger4.getBytes());
        String replace = bigInteger1.toString(16).replace("3", "-").replace("6", "/");
        bigInteger1 = r13;
        bigInteger2 = new BigInteger(replace.getBytes());
        String bigInteger5 = bigInteger1.toString(16);
        bigInteger1 = r13;
        bigInteger2 = new BigInteger(bigInteger5.getBytes());
        String bigInteger6 = bigInteger1.toString(16);
        bigInteger1 = r13;
        bigInteger2 = new BigInteger(bigInteger6.getBytes());
        String replace2 = bigInteger1.toString(16).replace("3", "-").replace("6", "/");
        bigInteger1 = r13;
        bigInteger2 = new BigInteger(replace2.getBytes());
        String replace3 = bigInteger1.toString(16).replace("4", "?").replace("f", "%").replace("3", ":").replace("d", "-").replace("2", "/");
        return r0;
    }

    public static final String decode(String s) {
        String s2 = s;
        StringBuilder sb = r13;
        StringBuilder sb2 = new StringBuilder();
        StringBuilder sb3 = sb;
        sb = r13;
        sb2 = new StringBuilder();
        StringBuilder sb4 = sb;
        StringBuilder sb5 = sb3;
        for (int i = 0; i < r2.length() - 1; i += 2) {
            sb3 = sb5.append((char) Integer.parseInt(r2.substring(i, i + 2), 16));
        }
        return sb5.toString();
    }

    public CerberusUtil() {
        CerberusUtil cbUtil = this;
    }
}
```

Wow. That's a lot to take in. But I'll be going over each part of the code and explain what's happening. Let's establish some goals first.

One of the first steps in reverse engineering is to set goals—*What are we doing and **why**?* By identifying our goals, we get a clearer picture about the purpose and intention of the task at hand. This also helps us determine the expected outcomes, as well as the success factors. So I asked my friend: *what do you want to know or get done?* He wants me to describe what is being done by the program. I then asked: *is there a specific part of the program you want to understand?* He said he wants to know what the long strings `/-/-/...` were. By asking these questions, we now know the goal of the reverse engineering task. We can now begin dissecting the code.

Looking at the code, I immediately noticed the `/-/-/...` strings he was referring to (who wouldn't, it took up a lot of screen space). Obviously, this looks like a case of obfuscation. I also noticed that it has an import statement referring to an Android package. This file must be from an Android app, I figured. I confirmed my hunch because the code has references for `r0`, `r11`, `r13`, and `r2`; these are placeholder names for registers when the APK is decompiled. I asked my friend where the file came from, and my hunch was correct. He said that the Java file he sent came from decompiling an APK file using <a href="https://bytecodeviewer.com/">Bytecode Viewer</a>. He created an Android app long ago, but lost access to the source code. So he used Bytecode Viewer in hopes of recovering the code. However, he doesn't remember what the `/-/-/...` was all about.

Now we know it is a Java file, from an Android app. Next, let's identify more facts by looking close at the code. We can see that it has the following components:

- 1 class named `CerberusUtil`
- 3 string constants named `APP`, `DEV`, and `PACKAGE`
- 4 functions named `bytesToHex`, `rev3753`, `rev3854`, and `decode`
- 1 default constructor

Before we move forward, let us remind ourselves of the objective: The goal was to uncover what the `/-/-/...` strings were. Therefore, our investigation will start with the string constants `APP`, `DEV`, and `PACKAGE`. Each of these string constants feed the `/-/-/...` strings as parameter to the `rev3753` function, so that's where we're going next.

```
public static final String rev3753(String s) {
    String decode = decode(decode(decode(decode(decode(decode(decode(s.replace("?", "4").replace("%", "f").replace(":", "3").replace("-", "d").replace("/", "2")).replace("-", "3").replace("/", "6")))).replace("-", "3").replace("/", "6"))));
    return r0;
}
```

Taking a look at `rev3753` function, we can see that it accepts a string parameter, but also returns or outputs a string as well. Inside the function, we have a string called decode, which is equal to the output of multiple `decode` functions. In total, there are 7 nested `decode` functions. Since the original code is formatted poorly, removing any whitespaces and newlines, let's make it more readable.

```
public static final String rev3753(String s) {
    String decode = 
        decode(
            decode(
                decode(
                    decode(
                        decode(
                            decode(
                                decode(
                                    s.replace("?", "4")
                                    .replace("%", "f")
                                    .replace(":", "3")
                                    .replace("-", "d")
                                    .replace("/", "2")
                                )
                                .replace("-", "3")
                                .replace("/", "6")
                            )
                        )
                    )
                    .replace("-", "3")
                    .replace("/", "6")
                )
            )
        );
    return r0;
}
```

Looks better; we can now understand how each `decode` function works, more specifically what input is used in the first function and what output gets fed to the next function and so on. We know that the parameter for the `rev3753` are the `/-/-/...` strings, and we can see that the `s` parameter is used in the inner-most `replace` functions. In other words, we are replacing `?, %, :, -, /` in the `/-/-/...` strings with `4, f, 3, d, 2` respectively. We are going to use the first constant, `APP`, in the succeeding parts, since  we will determine the algorithm used to reverse the process anyways; It will work on both `DEV` and `PACKAGE` too.

The `APP` string is:

```
/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-/%/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-://-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:5/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:1/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:0/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:7/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-://-/%/-:?/-/-/-/-/-/-/-:0
```

After we replace the characters, we get:

```
2d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d2f2d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d352d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d372d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d322d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d2f2d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d322d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d2f2d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d352d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d372d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d322d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d372d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d352d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d372d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d312d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d372d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d302d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d372d2d2d322d2f2d342d2d2d322d2f2d342d2d2d322d2f2d342d2d2d2d2d2d2d30
```

We still don't understand it, but that's okay. We are going to feed this text to the `decode` function as described below:

```
public static final String decode(String s) {
    String s2 = s;
    StringBuilder sb = r13;
    StringBuilder sb2 = new StringBuilder();
    StringBuilder sb3 = sb;
    sb = r13;
    sb2 = new StringBuilder();
    StringBuilder sb4 = sb;
    StringBuilder sb5 = sb3;
    for (int i = 0; i < s2.length() - 1; i += 2) {
        sb3 = sb5.append((char) Integer.parseInt(s2.substring(i, i + 2), 16));
    }
    return sb5.toString();
}
```

In the `decode` function, what we want to track is the `s` parameter. Observing the logic, we can remove the clutter, leaving us with:

```
public static final String decode(String s) {
    String s2 = s;
    StringBuilder sb = new StringBuilder();
    StringBuilder sb3 = sb;
    StringBuilder sb5 = sb3;
    for (int i = 0; i < s2.length() - 1; i += 2) {
        sb3 = sb5.append(
            (char)Integer.parseInt(
                s2.substring(i, i + 2),
                16)
            );
    }
    return sb5.toString();
}
```

Now we can read it easier. First, the string `s` is assigned to string `s2`, which is then referenced in the `for` loop. Looking at the `for` loop, we can see that it goes through the length of the string in **pairs**, because the iterator increments by two (hence `i += 2`). This is a significant clue, as we also have a function named `bytesToHex` in the source file. We can infer that the `/-/-/...` string we are working on may contain **hexadecimal** values. By jumping into the `for` loop, we can see `Integer.parseInt` uses two parameters: `s2.substring(i, i + 2)`, and `16`. Referencing the Java <a href="https://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html">documentation</a> reveals that this method is `parseInt(String s, int radix)` where the radix is the **base** of the original string. Since the value provided in the second parameter is `16`, meaning the radix is **base-16**, which therefore means the input string must be **hexadecimal**. Analysis confirmed! Now what? Well, the first parameter `s2.substring(i, i + 2)` effectively means to take two characters from string `s2` at current index. Putting both parameters together, our `Integer.parseInt` method takes two characters from string `s2` at the current index and converts it from **hexadecimal** to decimal, which is then casted to a corresponding `char` value in ASCII. This new `char` value is then appended to the StringBuilder `sb5`, then the `for` loop repeats until all characters in string `s` is converted. For the sake of brevity, here's the step-by-step procedure for the `decode` function:

1. Take input string (which is hexadecimal)
2. Convert the hexadecimal string to decimal
3. Convert the decimal to its ASCII equivalent
4. Return value

Applying this logic to our current value for `s` we get:

#### Convert the hexadecimal string to decimal
```
4545455045474552454545504547455245454550454745524545454545454552454545504547455245454550454745524545455045474552454545504547455245454550454745524545455045474552454545504547455245454550454745474545455045474552454545504547455245454550454745524545454545454553454545504547455245454550454745524545455045474552454545454545455545454550454745524545455045474552454545504547455245454545454545504545455045474552454545504547455245454550454745524545455045474547454545504547455245454550454745524545455045474552454545454545455045454550454745524545455045474552454545504547455245454550454745474545455045474552454545504547455245454550454745524545454545454553454545504547455245454550454745524545455045474552454545454545455545454550454745524545455045474552454545504547455245454545454545504545455045474552454545504547455245454550454745524545454545454555454545504547455245454550454745524545455045474552454545454545455345454550454745524545455045474552454545504547455245454545454545554545455045474552454545504547455245454550454745524545455045474552454545504547455245454550454745524545455045474552454545454545455245454550454745524545455045474552454545504547455245454545454545494545455045474552454545504547455245454550454745524545454545454555454545504547455245454550454745524545455045474552454545454545454845454550454745524545455045474552454545504547455245454545454545554545455045474552454545504547455245454550454745524545454545454548
```

#### Convert the decimal to its ASCII equivalent

```
---2-/-4---2-/-4---2-/-4-------4---2-/-4---2-/-4---2-/-4---2-/-4---2-/-4---2-/-4---2-/-4---2-/-/---2-/-4---2-/-4---2-/-4-------5---2-/-4---2-/-4---2-/-4-------7---2-/-4---2-/-4---2-/-4-------2---2-/-4---2-/-4---2-/-4---2-/-/---2-/-4---2-/-4---2-/-4-------2---2-/-4---2-/-4---2-/-4---2-/-/---2-/-4---2-/-4---2-/-4-------5---2-/-4---2-/-4---2-/-4-------7---2-/-4---2-/-4---2-/-4-------2---2-/-4---2-/-4---2-/-4-------7---2-/-4---2-/-4---2-/-4-------5---2-/-4---2-/-4---2-/-4-------7---2-/-4---2-/-4---2-/-4---2-/-4---2-/-4---2-/-4---2-/-4-------4---2-/-4---2-/-4---2-/-4-------1---2-/-4---2-/-4---2-/-4-------7---2-/-4---2-/-4---2-/-4-------0---2-/-4---2-/-4---2-/-4-------7---2-/-4---2-/-4---2-/-4-------0
```

We can now cross out the first `decode` function:

```diff
decode(
    decode(
        decode(
            decode(
                decode(
                    decode(
-                        decode(
-                            s.replace("?", "4")
-                            .replace("%", "f")
-                            .replace(":", "3"
-                            .replace("-", "d")
-                            .replace("/", "2")
-                        )
                        .replace("-", "3")
                        .replace("/", "6")
                    )
                )
            )
            .replace("-", "3")
            .replace("/", "6")
        )
    )
);
```

6 more to go!

For the next `decode` function, before it executes it must first replace the previous characters `-, /` with `3, 6` respectively. Our string would now be:

```
33323634333236343332363433333334333236343332363433323634333236343332363433323634333236343332363633323634333236343332363433333335333236343332363433323634333333373332363433323634333236343333333233323634333236343332363433323636333236343332363433323634333333323332363433323634333236343332363633323634333236343332363433333335333236343332363433323634333333373332363433323634333236343333333233323634333236343332363433333337333236343332363433323634333333353332363433323634333236343333333733323634333236343332363433323634333236343332363433323634333333343332363433323634333236343333333133323634333236343332363433333337333236343332363433323634333333303332363433323634333236343333333733323634333236343332363433333330
```

As with the previous step, this new (hexadecimal) string will be converted to decimal, and finally to an ASCII character array:

```
3264326432643334326432643264326432643264326432663264326432643335326432643264333732643264326433323264326432643266326432643264333232643264326432663264326432643335326432643264333732643264326433323264326432643337326432643264333532643264326433373264326432643264326432643264333432643264326433313264326432643337326432643264333032643264326433373264326432643330
```

Now that's the 6th `decode` function done and dusted:

```diff
decode(
    decode(
        decode(
            decode(
                decode(
-                    decode(
-                        decode(
-                            s.replace("?", "4")
-                            .replace("%", "f")
-                            .replace(":", "3"
-                            .replace("-", "d")
-                            .replace("/", "2")
-                        )
-                        .replace("-", "3")
-                        .replace("/", "6")
-                    )
                )
            )
            .replace("-", "3")
            .replace("/", "6")
        )
    )
);
```

The next two `decode` functions are executed one after the other, where the output of the former is the input to the latter. No character replace here. So let's just take our current value and put it to the `decode` function **twice**. Thus, we have:

#### First iteration of `decode`

```
2d2d2d342d2d2d2d2d2d2d2f2d2d2d352d2d2d372d2d2d322d2d2d2f2d2d2d322d2d2d2f2d2d2d352d2d2d372d2d2d322d2d2d372d2d2d352d2d2d372d2d2d2d2d2d2d342d2d2d312d2d2d372d2d2d302d2d2d372d2d2d30
```

#### Second iteration of `decode`

```
---4-------/---5---7---2---/---2---/---5---7---2---7---5---7-------4---1---7---0---7---0
```

Much more manageable now. That's two `decode` functions done:

```diff
decode(
    decode(
        decode(
-            decode(
-                decode(
-                    decode(
-                        decode(
-                            s.replace("?", "4")
-                            .replace("%", "f")
-                            .replace(":", "3"
-                            .replace("-", "d")
-                            .replace("/", "2")
-                        )
-                        .replace("-", "3")
-                        .replace("/", "6")
-                    )
-                )
-            )
            .replace("-", "3")
            .replace("/", "6")
        )
    )
);
```

Now we have to replace `-, /` with `3, 6` again:

```
3334333333363335333733323336333233363335333733323337333533373333333433313337333033373330
```

Then, run this string through our `decode` function thrice (again, hex → decimal → ASCII):

#### First iteration of `decode`

```
34333635373236323635373237353733343137303730
```

#### Second iteration of `decode`

```
4365726265727573417070
```

#### Final iteration of `decode`

```
CerberusApp
```

Voila! We have our decoded `APP` string. We've gone over every step in the `rev3753` function. In total, there were 7 nested `decode` functions:

```diff
-decode(
-    decode(
-        decode(
-            decode(
-                decode(
-                    decode(
-                        decode(
-                            s.replace("?", "4")
-                            .replace("%", "f")
-                            .replace(":", "3"
-                            .replace("-", "d")
-                            .replace("/", "2")
-                        )
-                        .replace("-", "3")
-                        .replace("/", "6")
-                    )
-                )
-            )
-            .replace("-", "3")
-            .replace("/", "6")
-        )
-    )
-);
```

One after the after, we went over each function call, taking note the input and the output. And as you can notice, we removed all the unnecessary stuff from being examined. This is because the first step is establishing the objectives. With a clear goal, we were able to narrow down the specific, lucrative parts of the code to analyze—we didn't even look into the `rev3854` function! What we essentially did was reverse engineering! With the knowledge of the algorithm, I wrote a JSFiddle:

<script async src="//jsfiddle.net/tumyda3q/embed/js,result/"></script>

So I sent the results to my friend, and I beat his challenge. We both won in the end though: I sharpened my skills in static analysis, and he gets back his source code. Win-win!

These kinds of challenges are popular on the internet: crack.me, hack.me, keygen.me, etc. We might do another one in the future!



