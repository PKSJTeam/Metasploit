# Metasploit

## UnrealIRCd 3.2.8.1 - Backdoor Command Execution (Metasploit)

### Pemasangan UnrealIRCd

unduh programnya di link berikuut [ini](https://www.exploit-db.com/apps/752e46f2d873c1679fa99de3f52a274d-Unreal3.2.8.1_backdoor.tar_.gz)
kemudian ekstrak ke tempat yang sama.

Install dependencies.
```bash
apt-get install build-essential
apt-get install openssl
apt-get install libcurl4-openssl-dev
```

Masuk ke folder Unreal3.2 yang telah diekstrak tadi
```bash
cd Unreal3.2
```

Jalankan perintah berikut
```bash
chmod 777 *;
./Config
```

Akan muncul pertanyaan-pertanyaan.
```
 Do you want to enable the server anti-spoof protection?
 ```
Jawab "Ya"

```
What directory are all the server configuration files in?
```
Lanjut

```
What is the path to the ircd binary including the name of the binary?
```
Lanjut

```
Would you like to compile as a hub or as a leaf?
```
Lanjut

```
What is the hostname of the server running your IRCd?
```
Lanjut

```
 What should the default permissions for your configuration files be?
```
Lanjut

```
 Do you want to support SSL (Secure Sockets Layer) connections?
```
Lanjut

