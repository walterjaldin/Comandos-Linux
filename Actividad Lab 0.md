
## Estudiante: Jaldin Gonzales Walter
## Materia: SCO420

### Crear un grupo con el nombre "tecnologos" (puede usar otros parámetros personalizados en la creación de la cuenta del usuario)

```
debian@debian:~$ sudo groupadd -g 1501 tecnologos
```

#### Para ver el grupo creado podemos usar el siguiente comando

```
tail -n1 /etc/group
```

![[Pasted image 20250827233808.png]]



### Crear un usuario que pertenezca al grupo **_tecnologos_** y el usuario se llame "adminNet"

```
debian@debian:~$ sudo useradd -m -s /bin/bash -g tecnologos adminNet
```

- `-m` → crea home `/home/adminNet`.
    
- `-s /bin/bash` → define shell.
    
- `-g tecnologos` → define grupo primario.

#### Le asignamos una password

```
sudo passwd adminNet
```

![[Pasted image 20250827234148.png]]


### Luego describa que pasos se debe seguir para que un usuario sea un usuario **sudo**.

```
sudo usermod -aG sudo adminNet
```

#### Verificamos que se haya agregado correctamente
```
groups adminNet
```

![[Pasted image 20250827234505.png]]

### Verifique los archivos de configuración relacionados con la creación de las cuentas de usuarios y saque sus conclusiones.

```
	/etc/passwd 
	/etc/shadow
	/etc/group
```

Los archivos `/etc/passwd` y `/etc/shadow` contienen información crítica: UID, GID, contraseñas encriptadas, fechas de expiración.

### Finalmente determine cuando expira las cuentas creadas y cuando se actualizan las contraseñas de los usuarios. Explique

```
debian@debian:~$ sudo chage -l adminNet 
```
![[Pasted image 20250827234910.png]]
- **Last password change** → última vez que se cambió la contraseña.
    
- **Password expires** → fecha límite para cambiar (si configurado).
    
- **Account expires** → fecha de caducidad de la cuenta.
    
- **Minimum days** → tiempo mínimo para cambiar.
    
- **Maximum days** → tiempo máximo antes de expirar.
    
- **Warning days** → aviso antes de expirar.

#### Configurar Expiracion
```
sudo chage -M 90 -W 7 -I 30 adminNet

```
- `-M 90` → expira cada 90 días.
    
- `-W 7` → avisa 7 días antes.
    
- `-I 30` → inactiva 30 días después de expirar.