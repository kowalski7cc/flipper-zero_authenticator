# Flipper Authenticator config file description

By default Flipper Authenticator stores all its settings in [`/ext/authenticator/totp.conf`](https://github.com/akopachov/flipper-zero_authenticator/blob/master/totp/services/config/config.c#:~:text=%23define%20CONFIG_FILE_DIRECTORY_PATH,totp.conf%22) file.

File format is standard for Flipper Zero device. Each line has one setting identified by key, where key and value are separated by `:` symbol.

## Available keys

<details>
<summary><h3 style="display: inline">Filetype</h3></summary>
<p>

**Type:** const string

**Default value:** `Flipper TOTP plugin config file`

**Description:** File type definition. Used internally. Should not be updated manually

</p>
</details>

<details>
<summary><h3 style="display: inline">Version</h3></summary>
<p>

**Type:** const unsigned int

**Default value:** `2`

**Description:** File version. Used internally. Should not be updated manually.

</p>
</details>

<details>
<summary><h3 style="display: inline">BaseIV</h3></summary>
<p>

**Type:** array of bytes

**Default value:** none

**Description:** Initialization vector (IV) which is getting generated randomly at first app start. It is used to setup encryption subsytem. Should not be updated manually.

**Important note:** changing or loosing this value will lead to incorrect decryption of all the encrypted data in the application and as a result it will not be possible to generate valid TOTP tokens

</p>
</details>

<details>
<summary><h3 style="display: inline">Crypto</h3></summary>
<p>

**Type:** array of bytes

**Default value:** none

**Description:** Used internally to verify user's PIN. Should not be changed manually.

**Important note:** changing or loosing this value will lead to incorrect PIN verification and it will not be possible to signin into app

</p>
</details>

<details>
<summary><h3 style="display: inline">Timezone</h3></summary>
<p>

**Type:** float

**Default value:** 0.000000

**Description:** Timezone offset **in hours**. Need to be modified manually. Because of Flipper Zero API doesn't provide an access to timezone offset it is necessary to set it manually for correct TOTP tokens generation. You may find your timezone offset (or another name is "UTC offset") [here](https://www.timeanddate.com/time/zone/timezone/utc) or on any other website found in google.

**Important note:** if your timezone offset is negative, use negative sign, like this `-2.0`, however if your timezone offset is positive DO NOT use explicit positive sign, just put offset without any sign like this `2.0`

</p>
</details>

<details>
<summary><h3 style="display: inline">PinIsSet</h3></summary>
<p>

**Type:** bool

**Default value:** none

**Description:** Used internally to identify whether user set PIN or not. Should not be changed manually.

**Important note:** changing or loosing this value will lead to incorrect PIN verification and it will not be possible to signin into app

</p>
</details>

<details>
<summary><h3 style="display: inline">NotificationMethod</h3></summary>
<p>

**Type:** enum (available options are `0`, `1`, `2`, `3`)

**Default value:** `3`

**Description:** How to notify user when new token is generated or badusb mode is activated. Possible values are:

* `0` - do not notify
* `1` - notify using sound only
* `2` - notify using vibro only
* `3` - notify using sound and vibro

</p>
</details>

<details>
<summary><h3 style="display: inline">TokenName</h3></summary>
<p>

**Type:** string

**Default value:** none

**Description:** Token name which will be visible in the UI and used just to let user identify token. Can be modified manually.

</p>
</details>

<details>
<summary><h3 style="display: inline">TokenSecret</h3></summary>
<p>

**Type:** array of bytes OR string

**Default value:** none

**Description:** Token secret. It can be either an array of encrypted bytes OR pure unencrypted token secret.

**Important note:** if app finds pure unencrypted token in config file app will encrypt it and replace in a config file for security purposes

</p>
</details>

<details>
<summary><h3 style="display: inline">TokenAlgo</h3></summary>
<p>

**Type:** enum (available options are: `sha1`, `sha256`, `sha512`)

**Default value:** `sha1`

**Description:** Token hashing algorithm to be used to generate TOTP code. If you don't know which one to use - use `sha1`.

</p>
</details>

<details>
<summary><h3 style="display: inline">TokenDigits</h3></summary>
<p>

**Type:** enum (available options are `6` and `8`)

**Default value:** `6`

**Description:** Defines TOTP code length. If you don't know which to use - use `6` as majority of websites requires 6-digits code.

</p>
</details>

## Example config file

```text
Filetype: Flipper TOTP plugin config file
Version: 2
BaseIV: AD F2 DE F3 31 92 C8 77 4B EB BF FE 7D E1 27 51
Crypto: FE CC 38 99 28 A9 28 6B BC E1 E3 92 B9 02 8A DF
Timezone: 2.000000
PinIsSet: true
NotificationMethod: 3
TokenName: Test plain
TokenSecret: 95 6B CE 3E 2F 01 AF 29 B2 9A DE CA E7 EF F5 B1
TokenAlgo: sha1
TokenDigits: 6
TokenName: Verifyr sha256
TokenSecret: SSECIUHGRYRCRBCNKKXPUQBLBGEQZ3PKNA7TA7TQV6IL5WDFU62TNNT3NHKVWRCQWF4QTSE4IGLG4S7RGY3LDMVDZVMAGB2ARPG7XYQ
TokenAlgo: sha256
TokenDigits: 6
TokenName: Verifyr sha512 8
TokenSecret: 3KKGABEJ4CKS5AHBZBDHFFNKUZHN6D7TKUGI3T7SHEUBAMIAPBUBWQNCMEEGEJX2LF23PYAFUCSRNVQ2ENOQWLHISCOJQCU2SCND4CI
TokenAlgo: sha512
TokenDigits: 8
```
