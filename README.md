# HashLoginSystem
Hash and encrypt, PHP examples
Example of an encrypted password hash storage in PHP, uses bcrypt for hashing and AES-128 in CBC mode for encryption. It uses defuse/php-encryption package for crypto operations. Do not encrypt just the passwords, encrypt only password hashes for extra security.

Usage
Install defuse/php-encryption via Composer first, or at least copy the Crypto.php file to your project
Don't write your own encryption functions
Key
Generate 128-bit key (in PHP hexdec-chars string) using

echo preg_replace('/(..)/', '\x$1', bin2hex(openssl_random_pseudo_bytes(16)));
or by running openssl rand -hex 16 | sed s/\\\(..\\\)/\\\\x\\1/g in bash
The key should be stored in the following format: "\xf3\x49\xf9\x4a\x0a\xb2 ...". Do NOT encode the $key with bin2hex() or base64_encode() or similar, they may leak the key to the attacker through side channels.

Files
example-encrypthash.php - Encrypted password hash storage, uses bcrypt + AES-128-CBC with PKCS#7 padding and SHA-256 HMAC authentication using Encrypt-then-MAC approach
example-hash.php - Password hash storage, uses bcrypt.
functions-encrypthash.php - Functions used by example-encrypthash.php
tests/encrypthash.php - Tests for encrypted hash functions
tests/hash.php - Tests for hash functions
Tests
Simple tests are included, run them with php tests/hash.php and php tests/encrypthash.php.
Requirements
To use PHP runtime library requires:

C extension: PHP 5.5, 5.6, or 7.
PHP package: PHP 5.5, 5.6 or 7.
Installation
C Extension
Prerequirements
To install the c extension, the following tools are needed:

autoconf
automake
libtool
make
gcc
pear
pecl
On Ubuntu, you can install them with:

sudo apt-get install -y php-pear php5-dev autoconf automake libtool make gcc
On other platforms, please use the corresponding package managing tool to install them before proceeding.

Installation from Source (Building extension)
To build the c extension, run the following command:

cd ext/google/protobuf
pear package
sudo pecl install protobuf-{VERSION}.tgz
Installation from PECL
When we release a version of Protocol Buffers, we will upload the extension to PECL. To use this pre-packaged extension, simply install it as you would any other extension:

sudo pecl install protobuf-{VERSION}
PHP Package
Installation from composer
Simply add “google/protobuf” to the ‘require’ section of composer.json in your project.

Protoc
Once the extension or package is installed, if you wish to generate PHP code from a .proto file, you will also want to install the Protocol Buffers compiler (protoc), as described in this repository's main README file. The version of protoc included in the latest release supports the --php_out option to generate PHP code:

protoc --php_out=out_dir test.proto
Usage
For generated code: https://developers.google.com/protocol-buffers/docs/reference/php-generated

Known Issues
Missing native support for well known types.
Missing support for proto2.
No API provided for clear/copy messages.
No API provided for encoding/decoding with stream.
Map fields may not be garbage-collected if there is cycle reference.
No debug information for messages in c extension.
HHVM not tested.
C extension not tested on windows, mac, php 7.0.
Message name cannot be Empty.
