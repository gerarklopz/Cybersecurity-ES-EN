# Sending an Encrypted Message with Encryption Systems EN-Ver-🇺🇸🇬🇧

## Introduction

In this practice lab, we will learn how to encrypt and decrypt messages using different encryption systems in Linux and Windows. We will use Base64 encoding for basic message transformation, and OpenSSL AES-256-CBC encryption for a more advanced and secure approach.

## Objective

The objective is to encrypt a message and send it to a person, providing them with clear instructions on how to decrypt it on different operating systems. Additionally, we will see how conversions from text to binary and other intermediate formats are performed in the encryption process.

---

### 1. Sending an Encrypted Message in Linux

#### 1.1 Encrypting with Base64

The first step is to encrypt the message using Base64 encoding. We run the following command to convert the message to Base64:

```bash
echo "Hi Laura, I'm Gerard and you've decrypted the secret message: 'you're a very good teacher' ;)" | base64 > message.b64
```

#### 1.0. Transformations: from Text to Binary, HEX, etc.

Each character in the message is converted to its binary equivalent (8-bit ASCII code). The following shows how the text is converted to binary:

```makefile
# Encrypted Message in Binary
H = 01001000
o = 01101111
l = 01101100
a = 01100001
(space) = 00100000
L = 01001100
a = 01100001
u = 01110101
r = 01110010
a = 01100001
, = 00101100
(space) = 00100000
s = 01110011
o = 01101111
y = 01111001
(space) = 00100000
G=01000111
e=01100101
r=01110010
a=01100001
r=01110010
d = 01100100
(space) = 00100000
y = 01111001
(space) = 00100000
h = 01101000
a=01100001
s=01110011
(space) = 00100000
d = 01100100
e=01100101
s=01110011
c=01100011
i = 01101001
f = 01100110
r=01110010
a=01100001
d = 01100100
o = 01101111
(space) = 00100000
e=01100101
l=01101100
(space) = 00100000
m=01101101
e=01100101
n=01101110
s=01110011
a=01100001
j = 01101010
e=01100101
(space) = 00100000
s=01110011
e = 01100101
c=01100011
r=01110010
e=01100101
t = 01110100
o = 01101111
:=00111010
(space) = 00100000
' = 00100111
e=01100101
r=01110010
e=01100101
s=01110011
(space) = 00100000
m=01101101
u=01110101
y = 01111001
(space) = 00100000
b = 01100010
u=01110101
e=01100101
n=01101110
a=01100001
(space) = 00100000
p = 01110000
r=01110010
o = 01101111
f=01100110
e=01100101
s=01110011
o = 01101111
r=01110010
a=01100001
' = 00100111
(space) = 00100000
;) = 00111011
```

### Process conversion:

Each letter is converted to its binary value using the ASCII table.

The resulting binary string is then grouped into blocks for Base64 encoding.

#### 24-bit Block Grouping (Base64)

Base64 works in blocks of 3 bytes (24 bits), so we group the bits into blocks of 24 bits (3 characters). Each 24-bit block is then divided into four blocks of 6 bits, and each of these blocks is converted into a Base64 symbol.

**Example with "Hello":**

```makefile
H = 01001000
o = 01101111
l = 01101100
a = 01100001
```

**24-bit block:**

```bash
01001000 01101111 01101100
```

**We divide the 24-bit block into 4 6-bit blocks:**
```bash
010010 → S
011011 → G
111101 → 9
101100 → s
```

#### Padding with = if necessary

If the total number of bits is not a multiple of 24, extra zeros are added at the end to complete the block, and the = character is used as padding. This padding ensures that the length of the encoded message is always a multiple of 4 Base64 characters.

**Example with "Hello":**

```makefile
a = 01100001
(space) = 00100000
```

**Incomplete block:**

```yaml
01100001 00100000
```

Let's fill in the blanks:

```yaml
011000 010010 0000 000000
```

Let's convert to Base64:

```bash
011000 → Y
010010 → S
000000 → A
000000 → =
```

So "Hello" in Base64 is "SG9sYQ==".

#### Final Result

After applying the algorithm to the entire message, we obtain its final representation in Base64:

```cpp
SG9sYSBMYXVyYSwgc29 ... (continues with the final output in Base64)
```

This is the original message **"Hi Laura, this is Gerard..."** transformed into Base64, ready to be transmitted or stored safely.

🔹 **Explanation of the Base64 command in Linux**:

`echo` prints the message to the terminal.

`| base64` converts the message to Base64.

`> message.b64` saves the output to a file named message.b64.

To verify the content, we can use:

```bash
cat message.b64
```

#### 1.2 Send the file

Now, the message.b64 file can be sent to the desired person, either by email, USB, or even through a virtual mailbox.

---

### 2. Decrypt in Windows (PowerShell)

#### 2.1 Instructions for the recipient

The message recipient should save the message.b64 file to their desktop and run the following command in PowerShell:

```powershell
Get-Content "$env:USERPROFILE\Desktop\message.b64" | ForEach-Object { [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($_)) }
```

🔹 **Explanation**:

`Get-Content` gets the contents of the file.

`FromBase64String` converts the Base64 text back to plain text.

`UTF8.GetString` displays it correctly on the screen.

---

#### 2.2 Decrypting in Windows (CMD with OpenSSL)

If the recipient has OpenSSL installed on Windows, you can use the following CMD command to decrypt the message:

```cmd
openssl aes-256-cbc -pbkdf2 -a -d -in message.enc -k SuperSecretKey
```

#### 2.3 Encrypting in Linux

To encrypt a message in Linux with OpenSSL:

```bash
echo "Hi Laura, this message is encrypted with AES-256" | openssl aes-256-cbc -pbkdf2 -a -e -out message.enc -k SuperSecretKey
```

🔹 **OpenSSL Command Explanation**:

`openssl aes-256-cbc` uses the AES algorithm with a 256-bit key in CBC mode.

`-pbkdf2` strengthens the key security.

`-a` converts the output to Base64 format.

`-e` indicates that we are encrypting.

`-out message.enc` saves the encrypted message to the file message.enc.

`-k SuperSecretKey` defines the encryption key.

#### 2.4 Decrypting in Linux

To decrypt the message in Linux:

```bash
openssl aes-256-cbc -pbkdf2 -a -d -in message.enc -k SuperSecretKey
```

🔹 **Difference**: In this case, we use the `-d` parameter to indicate that we are decrypting the message.

---

### 3. Send a message encrypted with OpenSSL (AES-256-CBC)

When we need **greater security** when sending messages, **encryption** is the best option. While techniques like Base64 convert the message into a format that is easier to handle and transmit, they **do not encrypt** it. **AES-256-CBC** is an encryption technique, which means it **irreversibly transforms the content** so that only the right person can read it.

### What is AES-256-CBC?

- **AES**: "Advanced Encryption Standard" is one of the most widely used encryption algorithms worldwide due to its **robustness and efficiency**.
- **256**: Refers to the size of the **key** used for encryption. The longer the key length, the **more secure** the encryption, because it becomes harder to break.

- **CBC**: Cipher Block Chaining is a **mode of operation** of AES that improves security. Essentially, each ciphertext block depends on the previous one, making an attack more difficult since each ciphertext block cannot be manipulated independently.

---

### 4. Why encrypt with OpenSSL AES-256-CBC?

#### 1. 256-bit symmetric encryption
The AES-256-CBC algorithm uses a **256-bit secret key**, which means that only someone with the **correct key** can decrypt the message. This is **much more secure** than a shorter key, since the possible combinations increase exponentially with the key length.

- **Why 256 bits?**
**256 bits** allow for **2^256 possible combinations**, which is an **astronomical** number of possible keys. With this, an attacker would have to try **billions of combinations** (or more) to break the encryption, something **virtually impossible** with current technology.

#### 2. Symmetric Encryption
AES-256-CBC is a **symmetric cipher**, which means that the **same key** is used to both encrypt and decrypt the message. This makes it efficient, but also means that the key must be **properly protected**.

- **What does "symmetric" mean?**
If someone knows the key, they can encrypt and decrypt the message. It is important to ensure that the key is not revealed to unauthorized persons.

#### 3. CBC (Cipher Block Chaining) Security
CBC mode adds an additional layer of security. In this mode, the **first block** of the message is encrypted normally, but **subsequent blocks** are encrypted so that each block depends on the previous one. This means that even if an attacker has access to some encrypted blocks, they cannot **manipulate** them or **alter the message** without affecting the rest.

- **Why is CBC more secure than other modes?**
If CBC were not used and the blocks were encrypted independently, an attacker could modify a block without affecting the rest of the message. With CBC, this is not possible, since each ciphertext block depends on the previous ones.

---

## Conclusions

We have learned how to encrypt and decrypt messages using Base64 for simple encoding and AES-256-CBC for strong and secure encryption. These methods are useful on both Linux and Windows, ensuring that the privacy and security of your information remains intact during transmission.

Now you can send and receive encrypted messages with greater security! 🚀





