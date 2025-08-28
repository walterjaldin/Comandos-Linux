# 📝 Bitácora Inicial de Comandos – Curso Linux Unhatched
**Autor:** Walter Jaldin Gonzales (cowboy)  

> Nota: No todas las utilidades comparten exactamente las mismas opciones en todas las distribuciones; aquí se listan las más comunes/portables. Usa `man <comando>` para validar en tu sistema.

---

## 1) Navegación y Exploración de Archivos/Directorios

### 1.1 `ls` — Lista archivos y directorios
**Sintaxis:**  
```bash
ls [OPCIONES] [RUTA...]
```
**Opciones clave:**  
- `-a` muestra *todos* (incluye ocultos `.`/`..`).  
- `-A` muestra ocultos excepto `.` y `..`.  
- `-l` formato largo (permisos, dueño, tamaño, fecha).  
- `-h` tamaños legibles (con `-l`).  
- `-S` ordenar por tamaño; `-t` por fecha; `-r` invierte orden.  
- `-R` recursivo.  
- `-1` una entrada por línea.  
- `-d` lista el propio directorio, no su contenido.  
- `-i` muestra inodo.  
- `--color=auto` colorea según tipo.  
**Ejemplo:**  
```bash
ls -lahS /var/log
```

### 1.2 `cd` — Cambia de directorio
**Sintaxis:**  
```bash
cd [RUTA]
```
**Argumentos útiles:** `..` (subir), `-` (volver al anterior), `~` (home).  
**Ejemplo:**  
```bash
cd -     # volver al último directorio visitado
```

### 1.3 `pwd` — Muestra ruta absoluta actual
**Sintaxis:**  
```bash
pwd [-L|-P]
```
- `-L` lógica (con enlaces simbólicos), `-P` física (resuelve symlinks).  
**Ejemplo:**  
```bash
pwd -P
```

### 1.4 `file` — Identifica tipo de archivo
```bash
file [OPCIONES] ARCHIVO...
```
- `-b` sin nombre delante; `-i` tipo MIME.  
**Ejemplo:** `file -i imagen.bin`

### 1.5 `stat` — Metadatos detallados
```bash
stat [OPCIONES] ARCHIVO
```
- `-c` formato personalizado.  
**Ejemplo:** `stat -c "%A %h %U %G %s %y %n" *`

---

## 2) Gestión de Archivos y Directorios

### 2.1 `cp` — Copia archivos/directorios
```bash
cp [OPCIONES] ORIGEN... DESTINO
```
- `-r`/`-R` recursivo (directorios).  
- `-a` modo archivado (preserva permisos, dueños, tiempos, enlaces).  
- `-p` preserva atributos.  
- `-i` interactivo (pregunta antes de sobrescribir).  
- `-u` solo si ORIGEN es más nuevo o no existe en DESTINO.  
- `-v` verboso; `-n` no sobrescribe; `-t DIR` especifica destino.  
**Ejemplo:**  
```bash
cp -a /etc/skel/. /home/nuevo_usuario/
```

### 2.2 `mv` — Mueve o renombra
```bash
mv [OPCIONES] ORIGEN... DESTINO
```
- `-i` preguntar; `-n` no sobrescribir; `-v` verboso; `-t DIR`.  
**Ejemplo:** `mv -i reporte.txt ~/Documentos/`

### 2.3 `rm` — Elimina archivos
```bash
rm [OPCIONES] ARCHIVO...
```
- `-r` recursivo; `-f` fuerza (omite errores); `-i` preguntar; `-v` verboso; `-d` elimina directorio vacío.  
**Ejemplo:** `rm -rf build/`

### 2.4 `mkdir` — Crea directorios
```bash
mkdir [OPCIONES] DIR...
```
- `-p` crea padres; `-m` modo (permisos); `-v` verboso.  
**Ejemplo:** `mkdir -p -m 755 /srv/www/{html,logs}`

### 2.5 `rmdir` — Elimina directorios vacíos
```bash
rmdir [-p|-v] DIR...
```
**Ejemplo:** `rmdir -p proyectos/antiguo/vacio`

### 2.6 `touch` — Crea/actualiza tiempos
```bash
touch [OPCIONES] ARCHIVO...
```
- `-a` atime; `-m` mtime; `-t [[CC]YY]MMDDhhmm[.ss]`; `-d "FECHA"`; `-c` no crear si no existe.  
**Ejemplo:** `touch -d "2023-12-31 23:59" cierre.log`

### 2.7 `ln` — Enlaces duros o simbólicos
```bash
ln [OPCIONES] ORIGEN [DESTINO]
```
- `-s` simbólico; `-f` fuerza; `-n` tratar destino como archivo normal; `-v` verboso.  
**Ejemplo:** `ln -s /opt/app/bin/app /usr/local/bin/app`

---

## 3) Visualización y Conteo

### 3.1 `cat` — Concatena/muestra
```bash
cat [OPCIONES] ARCHIVO...
```
- `-n` numera; `-b` numera solo líneas no vacías; `-s` comprime líneas vacías; `-A` muestra tabs/nl.  
**Ejemplo:** `cat -n script.sh`

### 3.2 `less` — Paginador interactivo
```bash
less [OPCIONES] ARCHIVO
```
- `-N` números de línea; `+F` seguimiento (como `tail -f`).  
- Buscar: `/patrón` (↓), `?patrón` (↑); `n`/`N` repite.  
**Ejemplo:** `less -N /var/log/syslog`

### 3.3 `head` / `tail` — Inicio/fin de archivo
```bash
head [-n N|-c BYTES] ARCHIVO
tail [-n N|-c BYTES|-f] ARCHIVO
```
**Ejemplos:**  
```bash
head -n 20 notas.md
tail -f /var/log/auth.log
```

### 3.4 `nl` — Numerar líneas
```bash
nl [OPCIONES] ARCHIVO
```
**Ejemplo:** `nl -ba notas.md`

### 3.5 `wc` — Conteos
```bash
wc [OPCIONES] ARCHIVO...
```
- `-l` líneas; `-w` palabras; `-c` bytes; `-m` caracteres.  
**Ejemplo:** `wc -l *.log`

---

## 4) Búsqueda, Filtros y Expresiones Regulares

### 4.1 `grep` — Busca texto (regex)
```bash
grep [OPCIONES] PATRON [ARCHIVO...]
```
**Opciones clave:**  
- `-i` ignora mayúsc./minúsc.; `-v` invierte (muestra lo que **no** coincide).  
- `-E` ERE (regex extendidas); `-F` busca *literal* (rápido).  
- `-r`/`-R` recursivo; `-n` nº línea; `-H` nombre archivo; `-o` solo coincidencia.  
- `-w` palabra completa; `-x` línea completa.  
- `-A NUM` después; `-B NUM` antes; `-C NUM` contexto.  
- `--color=auto` resalta coincidencias.  
**Ejemplos:**  
```bash
grep -Rni --color=auto "error|fail" /var/log
grep -E "^[0-9]{3}-[0-9]{2}-[0-9]{4}$" datos.txt
```

**Mini‑chuleta regex (ERE):**  
- `^` inicio, `$` fin, `.` cualquiera, `[...]` clase, `[^...]` negación.  
- `*` 0+ repetic.; `+` 1+; `?` 0 o 1; `{m,n}` rango.  
- `(...)` grupo; `|` OR; `\b` borde de palabra (en `grep -P`).  
> Para `\b` y lookaheads usa `grep -P` (Perl) si tu grep lo soporta.

### 4.2 `find` — Busca archivos por atributos
```bash
find RUTA [CONDICIONES] [ACCIONES]
```
**Condiciones comunes:**  
- `-name "patrón"` / `-iname` (insensible).  
- `-type f|d|l` (archivo, dir, link).  
- `-size +100M` (`c` bytes, `k` KB, `M` MB, `G` GB).  
- `-mtime -7` modificado en últimos 7 días; `-mmin -30` en últimos 30 min.  
- `-user USR` `-group GRP` `-perm 0644` `-maxdepth N`.  
**Acciones:** `-print`, `-delete`, `-exec CMD {} \;`, `-exec CMD {} +`.  
**Ejemplos:**  
```bash
find /var/log -type f -name "*.log" -mtime -3 -exec gzip {} \;
find . -maxdepth 1 -type f -size +1G -print
```

### 4.3 `locate`/`updatedb` — Índice rápido (mlocate/plocate)
```bash
locate PATRON
sudo updatedb
```
**Ejemplo:** `locate sshd_config`

### 4.4 `xargs` — Construye/encadena comandos
```bash
xargs [OPCIONES] COMANDO
``>
- `-0` separador NUL (con `find -print0`).  
**Ejemplo (seguro):**  
```bash
find . -type f -name "*.tmp" -print0 | xargs -0 rm -f
```

---

## 5) Cuentas de Usuario y Grupos

### 5.1 `id` — Identidad actual
```bash
id [USUARIO]
```

### 5.2 `adduser` (Debian/Ubuntu) — Asistente de alta
```bash
sudo adduser [OPCIONES] USUARIO
```
- `--home DIR` home personalizado.  
- `--shell RUTA` shell (p.ej. `/bin/bash`).  
- `--disabled-password` crea sin contraseña (útil con claves/`passwd` luego).  
- `--gecos "Nombre,Oficina,Tel1,Tel2"` metadatos.  
**Ejemplo:**  
```bash
sudo adduser --shell /bin/bash --gecos "Walter,,," cowboy
```

### 5.3 `useradd` — Alta “de bajo nivel”
```bash
sudo useradd [OPCIONES] USUARIO
```
- `-m` crea home; `-M` no crea home.  
- `-d DIR` home; `-s SHELL` shell; `-c "COMENT"` GECOS.  
- `-g GRP` grupo primario; `-G g1,g2` grupos suplementarios.  
- `-u UID` id usuario; `-e FECHA` caducidad; `-f DÍAS` inactividad tras caducar.  
- `-k DIR` copia de skeleton; `-r` sistema (UID bajo); `-N` no crea grupo del mismo nombre.  
- `-p HASH` **hash** de contraseña (no texto plano; usar `openssl passwd -6`).  
**Ejemplo:**  
```bash
sudo useradd -m -s /bin/bash -G sudo,adm -c "Walter" cowboy
```

### 5.4 `passwd` — Gestiona contraseñas
```bash
sudo passwd [OPCIONES] USUARIO
```
- sin opciones: cambiar contraseña.  
- `-e` expira ahora; `-l` bloquea; `-u` desbloquea; `-S` estado;  
- `-n D` mínimo días; `-x D` máximo; `-w D` aviso; `-i D` inactivo tras expirar.  
**Ejemplo:** `sudo passwd -l cowboy`

### 5.5 `chage` — Ciclo de vida de contraseña
```bash
sudo chage [OPCIONES] USUARIO
```
- `-l` lista; `-M` máx; `-m` mín; `-W` aviso; `-E` fecha de expiración; `-d` última mod.  
**Ejemplo:** `sudo chage -M 90 -W 7 cowboy`

### 5.6 Grupos
```bash
sudo groupadd [-g GID] GRUPO
sudo groupmod [-n NUEVO] [-g NUEVO_GID] GRUPO
sudo groupdel GRUPO
```
**Ejemplo:** `sudo groupadd -g 1500 devops`

### 5.7 `usermod` — Modifica cuentas
```bash
sudo usermod [OPCIONES] USUARIO
```
- `-aG GRUPOS` añade a grupos (con `-a` para no sobrescribir).  
- `-g GRP` grupo primario; `-G g1,g2` grupos suplementarios.  
- `-s SHELL` shell; `-d DIR` cambia home (`-m` mueve contenidos).  
- `-L` bloquea; `-U` desbloquea; `-e FECHA` caducidad; `-u UID` cambia UID.  
**Ejemplo:** `sudo usermod -aG docker,sudo cowboy`

### 5.8 `gpasswd` — Admin de `/etc/group`
```bash
sudo gpasswd -a USUARIO GRUPO   # añade
sudo gpasswd -d USUARIO GRUPO   # quita
```
**Ejemplo:** `sudo gpasswd -a cowboy adm`

### 5.9 Borrar usuarios
```bash
sudo deluser USUARIO                 # Debian/Ubuntu helper
sudo deluser --remove-home USUARIO
sudo userdel [-r] USUARIO            # -r elimina home/mailspool
```

### 5.10 `su` y `sudo`
```bash
su - [USUARIO]     # cambia a usuario (login shell)
sudo COMANDO       # ejecuta como root (o con -u USR)
sudo -u www-data whoami
```
- Editar sudoers: `sudo visudo` (y/o `/etc/sudoers.d/`)

---

## 6) Permisos, Propiedad y ACL

### 6.1 `chmod` — Cambia permisos
```bash
chmod [OPCIONES] MODO ARCHIVO...
```
- **Numérico:** `u g o` → `r=4 w=2 x=1` → `chmod 750 file`  
- **Simbólico:** `u/g/o/a` + `+/-/=` + `rwxXst`  
- `-R` recursivo; `-c/-v` reporta cambios.  
**Ejemplos:**  
```bash
chmod 640 informe.txt
chmod u=rwx,go=rx -R /var/www
```

### 6.2 `chown` / `chgrp` — Dueño y grupo
```bash
chown [OPCIONES] USR[:GRP] ARCHIVO...
chgrp [OPCIONES] GRP ARCHIVO...
```
- `-R` recursivo; `--from=ANT` condiciona.  
**Ejemplo:** `sudo chown -R www-data:www-data /srv/app`

### 6.3 `umask` — Permisos por defecto
```bash
umask [MASCARA]
```
- Máscara típica usuarios: `022` (archivos 644, dir 755).  
**Ejemplo:** `umask 027`

### 6.4 ACLs (`getfacl`/`setfacl`)
```bash
getfacl RUTA
setfacl -m u:usuario:rwx RUTA
setfacl -m g:grupo:rx RUTA
setfacl -d -m u:usuario:rwX DIR   # ACL por defecto para nuevos
setfacl -b RUTA                   # borra ACL
```
> Requiere FS con ACL habilitadas (ext4/xfs…)

### 6.5 Atributos en ext* (`lsattr`/`chattr`)
```bash
lsattr RUTA
sudo chattr +i archivo  # inmutable
sudo chattr +a archivo  # solo append
```

---

## 7) Procesos y Servicios

### 7.1 `ps` — Lista procesos
```bash
ps aux            # estilo BSD
ps -ef            # estilo UNIX
ps -eo pid,ppid,user,pcpu,pmem,stat,tty,time,cmd --sort=-pcpu
```
**Opciones útiles:**  
- `-e` todos; `-f` completo; `-o` formato; `--sort` ordenar.  

### 7.2 `pgrep` / `pkill`
```bash
pgrep -a nginx        # muestra PID(s)
sudo pkill -9 nginx   # termina por nombre
```

### 7.3 `kill` / `killall`
```bash
kill -SIGTERM PID
kill -9 PID
sudo killall -HUP rsyslogd
```

### 7.4 Prioridad: `nice` / `renice`
```bash
nice -n 10 comando
sudo renice -n -5 -p PID
```

### 7.5 Monitoreo: `top` / `htop`
- `top` básico, `htop` interactivo (F9 matar, F6 ordenar).  
**Ejemplo:** `htop`

### 7.6 systemd: `systemctl` y `journalctl`
```bash
sudo systemctl status nginx
sudo systemctl start|stop|restart nginx
sudo systemctl enable|disable nginx
sudo systemctl is-enabled nginx
sudo systemctl daemon-reload
journalctl -u nginx --since "today"   # logs del servicio
journalctl -xe -f                     # errores en vivo
```

---

## 8) Disco, FS y Sistema

### 8.1 `df` / `du` — Uso de disco
```bash
df -hT                    # por FS (humano + tipo)
du -sh DIR                # total de DIR
du -h --max-depth=1 DIR   # desglose por subdir
```

### 8.2 Bloques y montajes
```bash
lsblk -f            # dispositivos, UUID, FS
mount | column -t   # montajes actuales
sudo mount /dev/sdb1 /mnt/usb
sudo umount /mnt/usb
```

### 8.3 Memoria y kernel
```bash
free -h
uname -a
cat /etc/os-release   # o: lsb_release -a
hostnamectl
date
timedatectl
```

---

## 9) Red básica

```bash
ip addr show
ip link show
ip route show
ping -c 4 8.8.8.8
traceroute 1.1.1.1           # paquete: inetutils-traceroute o traceroute
ss -tulpen                   # sockets (reemplaza netstat)
curl -I https://example.com
wget https://example.com/archivo.tar.gz
```

---

## 10) Compresión/Empaquetado

### 10.1 `tar` — Empaquetar/extraer
```bash
tar -czvf backup.tgz DIR     # crear (.tgz: gzip)
tar -xzvf backup.tgz -C /tmp # extraer en /tmp
tar -cJf backup.txz DIR      # xz
tar -tf archivo.tar          # listar contenido
# Flags: -c crear, -x extraer, -t listar, -z gzip, -j bzip2, -J xz, -v verbose, -f archivo, -C DIR destino
# Excluir:
tar -czf site.tgz --exclude=".git" --exclude="node_modules" .
```

### 10.2 gzip/bzip2/xz
```bash
gzip -9 archivo    # comprime (reemplaza por .gz)
gunzip archivo.gz  # descomprime
xz -T0 archivo     # xz multihilo
```

### 10.3 zip/unzip
```bash
zip -r proyecto.zip proyecto/
unzip proyecto.zip -d /tmp/proyecto
```

---

## 11) Paquetería (Debian/Ubuntu y Arch)

### 11.1 APT (Debian/Ubuntu)
```bash
sudo apt update
sudo apt upgrade
sudo apt install PAQUETE
sudo apt remove PAQUETE
sudo apt purge PAQUETE
sudo apt autoremove
apt search TEXTO
apt show PAQUETE
```
**dpkg:**  
```bash
sudo dpkg -i paquete.deb
dpkg -l | less
dpkg -L PAQUETE      # archivos instalados por el paquete
dpkg -S /ruta/archivo  # a qué paquete pertenece
```

### 11.2 pacman (Arch/CachyOS)
```bash
sudo pacman -Syu               # sync + upgrade
sudo pacman -S PAQUETE
sudo pacman -Rns PAQUETE       # remove + deps + configs huérfanas
pacman -Qi PAQUETE             # info
pacman -Ql PAQUETE             # lista archivos
pacman -Ss TEXTO               # buscar
```

---

## 12) Buenas prácticas administrativas (mini‑guía práctica)

- Ver usuarios conectados y sesiones: `w`, `who`, `last`.  
- Auditar cambios de permisos en un árbol: `find /srv -type f -perm /o+w`.  
- Buscar fallos de sudo: `journalctl -p err -g sudo --since "yesterday"`.  
- Revisar intentos fallidos de SSH: `grep -i "Failed password" /var/log/auth.log | wc -l` (Debian).  
- Ver qué archivos cambian “hoy”: `find /var/www -type f -mtime -1 -print`.  
- Ver permisos efectivos con ACL: `getfacl -R /srv/www`.  
- Probar regex antes de usarlas en `grep`: `printf "a1\na2\nb3\n" | grep -E '^[ab][0-9]$'`.

---

## 13) Tabla rápida: permisos numéricos
- `7` = `rwx`, `6` = `rw-`, `5` = `r-x`, `4` = `r--`, `3` = `-wx`, `2` = `-w-`, `1` = `--x`, `0` = `---`
- Ejemplos: `640` (u=rw, g=r, o=—), `750` (u=rwx, g=rx, o=—)
- Bits especiales: `4xxx` setuid, `2xxx` setgid, `1xxx` sticky (`chmod 1777 /tmp`)

---

## 14) Glosario rápido
- **UID/GID**: Identificadores de usuario/grupo.  
- **GECOS**: Campos informativos de cuenta.  
- **ACL**: Listas de control de acceso por usuario/grupo sobre ficheros.  
- **Inodo**: Metadatos de un archivo en el FS.  

---
