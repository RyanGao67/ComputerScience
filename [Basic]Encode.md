# ASCII

First there was the C programming language, then there was ASCII. In ASCII, every letter, digits, and symbols that mattered (a-z, A-Z, 0–9, +, -, /, “, ! etc.) were represented as a number between 32 and 127. Most computers used 8-bits bytes then. This meant each byte could store 2⁸-1= 255 numbers. So each byte (or unit of storage) had more than enough space to store the basic set of english characters.


# Unicode
In order to accommodate the non-english characters, people started going a little crazy on how to use the numbers from 128 to 255 still available on a single byte. Different people would use different characters for the same numbers. Obviously, not only was it the wild wild west, but it quickly dawned that the extra available numbers could not even come close to represent the complete set of characters for some languages.
The Unicode was a brave attempt to create a single character set that could represent every characters in every imaginable language systems. This required a paradigm shift in how to interpret characters. And in this new paradigm, each character was an idealized abstract entity. Also in this system, rather than use a number, each character was represented as a code-point. The code-points were written as: U+00639, where U stands for ‘Unicode’, and the numbers are hexadecimal.

# UTF-8
The final piece we’re missing at this point is a system for storing and representing these code-points. This is what the encoding standards serve. After a couple of hits and misses, the UTF-8 encoding standard was born.
In UTF-8, every code-point from 0–127 is stored in a single byte. Code points above 128 are stored using 2, 3, and in fact, up to 6 bytes.
Remember that each byte consists of 8 bits, and the number of allowed information increases exponentially with the bits used for storage. Thus with 6 bytes (and not necessarily always 6), one could store as many as 2⁴⁸ characters. That would make for a very (very) large scrabble game!
Other benefits of UTF-8 meant that nothing changed from the ASCII so far as the basic english character-set was considered. Life continued to be good for english-speakers (a coincidence?). Everybody else had to hop some trains, but finally everybody agreed to use the same set of standards.
Perhaps the single most important thing to remember from the discussions above is that there is NO string or text, without an accompanying encoding standard. If you are a web-developer like me, you will occasionally encounter this text: Content-Type: text/plain; charset="UTF-8" in the code. This essentially tells the browsers (or any text parsers) the specific encoding being used.

# UTF-16
Now that we know what UTF-8 is, extrapolating our understanding to UTF-16 should be fairly straight-forward. UTF-8 is named for how it uses a minimum of 8 bits (or 1 byte) to store the unicode code-points. Remember that it can still use more bits, but does so only if it needs to.
UTF-16, in the other hand, uses a minimum of 16 bits (or 2 bytes) to encode the unicode characters. Java, whom I have a love-hate relationship with, natively uses this encoding. A Java char (Character) is thus a UTF-16 encoded unicode character and occupies a memory space of 2 bytes. The numerical value ranges from
0x0000 (numerical value: 0) to
0xFFFF (numerical value: 65,536)
This also means UTF-16 is NO longer backwards compatible with ASCII. Remember ASCII only used 1 byte or 8 bits.
