javascript-blowfish
===================

Blowfish encryption library Javascript, jquery,coffeescript (blowfish.js)

Blowfish is block cipher, block length is _8 byte_.

### Text data encryption (ASCII/text)

It you want to encrypt _string information_ (like text-message, or json, xml):
use _trimZeroes_ method (see bellow Example 1).

#### Example: ECB mode, default

```javascript
var bf = new Blowfish("secret key");
var encrypted = bf.encrypt("secret message");
var decrypted = bf.decrypt(encrypted);
decrypted = bf.trimZeroes(decrypted); // for string/text information 
console.log(decrypted);
```

### Binary data encryption 

If you want to encrypt _binary data_, you must provide
encrypt function with string length multiple by 8.

**Example:**

Input string for encryption: `"asdf"` (4 bytes) is not enough.
Blowfish want 8-byte string (or 16, 24, 32,...)

So my lib automaticaly pad string with zeros: `"asdf\0\0\0\0"`
If you want to prevent such behaviour you should pad input data to block size.

Additional info about padding: [Using Padding in Encryption](http://www.di-mgt.com.au/cryptopad.html) (@lucnap suggested)

After decryption we will get not `"asdf"`, but `"asdf\0\0\0\0"` string.




#### Example 2: CBC mode (better for encrypting long messages and images).

For CBC you need additional key (CBC Vector) which length should be 8 bytes.

```javascript
var bf = new Blowfish("key", "cbc");
var encrypted = bf.encrypt("secret message", "cbcvecto");
var decrypted = bf.decrypt(encrypted, "cbcvecto");
```

Blowfish when encrypt produces binary string as result.
It's not usable for example, to copy paste. We could encode it
to base64 text format:

#### Example 3: with base64 encoded output

```javascript
var bf = new Blowfish("key");

// Encrypt and encode to base64
var encrypted = bf.base64Encode(bf.encrypt("secret message"));
console.log(encrypted);

// Decrypt
var encrypted = bf.base64Decode(encrypted);
var decrypted = bf.decrypt(encrypted);
```
