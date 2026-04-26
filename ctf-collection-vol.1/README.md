<img src="assets/ctf-logo.png" width="700">

# CTF Collection Volume 1 – Write-up

The CTF Collection Volume 1 room is a beginner-friendly set of challenges designed to introduce a variety of fundamental techniques used in Capture The Flag competitions. Instead of focusing on a single topic, this room explores multiple areas such as steganography, cryptography, metadata analysis, and basic web exploitation.

While the tasks are relatively simple, they provide a solid foundation for understanding how to approach different types of challenges and, more importantly, how to think when solving them.

In this write-up, I will walk through my thought process for all the 20 tasks, focusing not only on the tools used but also on why each step was taken.

## Task 1 – Base64 Decoding

The first task presented an encoded string. Based on its format (ending with `=` and readable character set), it strongly resembled Base64 encoding.

To confirm this, I used:

```echo "encoded_string" | base64 -d```

This successfully revealed the flag.

<img src="assets/task1-decode.png" width="700">

## Task 2 – Metadata Analysis

The second task involved an image file. Instead of looking at the image visually, I checked for hidden information inside its metadata.

Using:

```exiftool image.jpg```

I was able to extract metadata fields, where the flag was embedded.

<img src="assets/task2-metadata.png" width="700">

# Task 3 – Steganography

This task suggested hidden data inside an image, likely using steganography.

I used:

```steghide extract -sf image.jpg```

In this case, no passphrase was required (simply pressing Enter). In harder challenges, the passphrase would need to be identified through further enumeration or analysis.

<img src="assets/task3-steganography.png" width="700">

# Task 4 – Hidden Text in Web Page

At first glance, the page appeared empty or contained only black text. However, this is often a trick.

By either:

+ Highlighting the page with the mouse, or
+ Inspecting the HTML source

I discovered hidden text containing the flag.

<img src="assets/task4-highlight.png" width="700">

# Task 5 – QR Code Decoding

This task involved a QR code. There are multiple ways to approach this:

+ Using a phone camera
+ Using tools like `zbarimg` on Kali
+ Online decoders

For this task, I decided to use:

```zbarimg qrcode.png```

<img src="assets/task5-qrcode.png" width="700">

Which directly returned the decoded content containing the flag.

# Task 6 – Inspecting an ELF Binary

The challenge provided a 64-bit ELF binary. Instead of executing it immediately, I chose to inspect its contents.

Since TryHackMe flags typically start with “THM”, I filtered the output accordingly:

```strings hello.hello | grep THM```

This extracts readable strings and narrows the results to likely flag candidates.

The flag appeared directly in the output.

<img src="assets/task6-filtering.png" width="700">

# Task 7 – Base58 / Encoded String

This task involved an encoded string that didn’t match common formats like Base64.

After identifying the encoding as Base58, I decoded it with:

```echo "encoded_string" | base58 -d```

<img src="assets/task7-decode.png" width="700">

# Task 8 – Caesar Cipher (ROT Cipher)

The text appeared readable but didn’t make sense, suggesting a substitution cipher.

The hint indicated that it was a Caesar cipher, so I used [dCode](https://www.dcode.fr/en) to test multiple shifts at once.

One of the outputs (ROT7) revealed the flag.

<img src="assets/task8-caesarcipher.png" width="700">

# Task 9 – Inspecting Page Source

This task required inspecting the HTML source of the TryHackMe page itself.

By right-clicking → Inspect i found the flag hidden in the HTML.

<img src="assets/task9-htmlinspection.png" width="700">

# Task 10 – Corrupted PNG File

The file appeared to be a PNG, but running:

```file spoil.png```

showed it as generic “data”, indicating that the header was likely corrupted.

Since PNG files have a well-known magic number, I opened the file in a hex editor and corrected the header to the proper signature.

<img src="assets/task10-hexeditor.png" width="700">

After fixing the header, the file was properly recognized and could be opened normally.

<img src="assets/task10-flag.png" width="700">
