# ASCII vs Unicode

## ASCII character

Computers can only understand numbers, so an ASCII code is the numerical representation of a character such as `a` or `@`.

ASCII (American Standard Code for Information Interchange) is the most common format for text files in computers and on the Internet. In an ASCII file, each alphabetic, numeric, or special character is represented with a 7-bit binary number (a string of seven 0s or 1s). **128** possible characters are defined.

> If someone says they want your CV however in ASCII format, all this means is they want 'plain' text with no formatting such as tabs, bold or underscoring - the raw format that any computer can understand. This is usually so they can easily import the file into their own applications without issues.

## Unicode

Fundamentally, computers just deal with numbers. They store letters and other characters by assigning a number for each one. Before Unicode was invented, there were hundreds of different systems, called character encodings, for assigning these numbers. These early character encodings were limited and could not contain enough characters to cover all the world's languages. Even for a single language like English no single encoding was adequate for all the letters, punctuation, and technical symbols in common use.

Early character encodings also conflicted with one another. That is, two encodings could use the same number for two different characters, or use different numbers for the same character. Any given computer (especially servers) would need to support many different encodings. However, when data is passed through different computers or between different encodings, that data runs the risk of corruption.

**The Unicode Standard provides a unique number for every character, no matter what platform, device, application or language.** It has been adopted by all modern software providers and now allows data to be transported through many different platforms, devices and applications without corruption.

## ASCII vs Unicode

Unicode is a superset of ASCII, and the numbers 0–128 in unicode have the same meaning in ASCII.

## Why ASCII and Unicode were created in the first place?

As we know computer only understand 0 or 1. Therefore, we need to have some kind of system that represents each character with unique binary number. For example, `1000001` means `A` and `1000010` means `B`.

### ASCII, Origins

ASCII uses 7 bits to represent a character. By using 7 bits, we can have a maximum of 2^7 (= 128) distinct combinations. Which means that we can represent 128 characters maximum. Most ASCII characters are printable characters of the alphabet such as abc, ABC, 123, ?&!, etc. The others are control characters such as carriage return, line feed, tab, etc.

```
0100101 -> % (Percent Sign - 37)
1000001 -> A (Capital letter A - 65)
1000010 -> B (Capital letter B - 66)
1000011 -> C (Capital letter C - 67)
0001101 -> Carriage Return (13)
```

**ASCII was meant for English only.** Because the center of the computer industry was in the USA at that time. As a consequence, they didn't need to support accents or other marks such as á, ü, ç, ñ, etc. (aka diacritics).

### ASCII Extended

Some clever people started using the 8th bit to encode more characters to support their language (to support "é", in French, for example). Just using one extra bit doubled the size of the original ASCII table to map up to 256 characters (2^8 = 256 characters). And not 2^7 as before (128).

```
10000010 -> é (e with acute accent - 130)
10100000 -> á (a with acute accent - 160)
```

### Unicode, The Rise

ASCII Extended solves the problem for languages that are based on the Latin alphabet... what about the others needing a completely different alphabet? Greek? Russian? Chinese and the likes?

Unicode doesn't contain every character from every language, but it sure contains a gigantic amount of characters.
