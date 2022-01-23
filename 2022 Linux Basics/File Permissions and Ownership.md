# File Permissions and Ownership

<br>

|  File type  |  Identifier |
| -- | -- |
|  Directory  |  d  |
|  Regular file  |  -  |
|  Character device  |  c  |
|  Link  |  |  |
|  Socket file  |  s  |
|  Pipe  |  p  |
|  Block device  |  b  |

<br>

**Check Ownership**

- `ls -d`: list directory

      ls -d media

- `ls -ld`: list directory information

      ls -ld media
      drwxr-xr-x 1 forest forest 4096 Apr 15  2020 media

- `ls -l *`

      ls -l *

      total 1468
      -rw-r--r-- 1 bob bob  73016 Apr 15  2020 Cypress.jpeg
      -rw-r--r-- 1 bob bob  73016 Apr 15  2020 Redwood.jpeg
      -rw-r--r-- 1 bob bob  10108 Apr 15  2020 Orangutan.jpeg
      
      ...

      -rw-r--r-- 1 bob bob  13984 Apr 15  2020 Conifer.jpeg

<br>

**File Permissions**

|  Bit  |  Purpose  | Octal value   |
| -- | -- | -- |
| r   | Read   |  4  |
| w   | Write   |  2  |
|  x  | Execute   |  1  |
|-|No permission| 0|

<br>

**Composite permission**

|  bit comb  | Octal comb |  Octal sum  |
| -- | -- | -- |
| rwx   | 4 + 2 + 1   |  7  |
| rw- | 4 + 2 |  6  |
| r-x   | 4 + 1   |  5  |
| -wx   | 2 + 1   |  3  |

**Command examples**

  - `chmod <permission> file`: change mode. Change permissions.

Change permission by bit

        chmod u+rwx file-name

        chmod ug+r-w file-name

        chmod o-rwx file-name

        chmod u+rwx, g+r-x, o-rwx test-file

<br>

Change permission by Octal

- Full access to owner, groups and others

        chmod 777 file-name

- Read and execute access to owner, groups and others

        chmod 555 file-name

- Read and write access to owner and group. No access to others

        chmod 660 file-name

- Full permission to owner, read and execute access to group, no permission to others. 

        chmod 750 file-name

<br>
<br>

**Change ownership**

`chwon owner:group file`

- Change owner to forest and group to devops.

      chown forest:devops file

- Change owner to forest. Group unchanged. 

      chwon forest android.apk

- Change owner group to group called devops. 

      chgrp devops android.apk 
