# User & Security Management

Linux is a **multi-user operating system**, meaning multiple users can access the system simultaneously.  
Security in Linux is enforced through **users, groups, ownership, and permissions**, ensuring controlled and secure access to system resources.

---

## 1.1 Creating Users and Granting Sudo Access

### Concept

- The **root user** has unrestricted access to the system and can perform any operation.
- Direct root usage is risky because accidental commands can damage the system.
- `sudo` (Superuser Do) provides **controlled privilege escalation**, allowing trusted users to execute administrative commands safely.
- Linux follows the **principle of least privilege**, where users are given only the permissions they need.
- Using `sudo` also improves **auditing**, as administrative actions are logged per user.


### Commands

```bash

sudo adduser username
sudo usermod -aG sudo username

```

- adduser creates a new user along with a home directory and default configuration.

- usermod -aG sudo adds the user to the sudo group, granting administrative privileges when required.

- After this, the user can execute privileged commands using sudo instead of logging in as root.

- “Instead of using the root account directly, Linux uses sudo to provide secure and auditable administrative access. During my internship, I created users and granted sudo access based on operational needs.”

---

## 1.2 File Ownership (`chown`)

### Concept

- File ownership determines which **user or group** controls a file.
- Only the **root user** or the current owner can change ownership.
- Ownership is commonly changed when files are shared between users or services.
- File ownership is important in multi-user systems to ensure the correct user or service has access to required files while preventing unauthorized access.

### Commands

```bash

sudo chown username filename
sudo chown :groupname filename
sudo chown username:groupname filename

```


#### Explanation

- username → changes file owner

- :groupname → changes group ownership

- username:groupname → changes both owner and group

---

## 1.3 File Permissions `chmod`

## Concept

- Linux permissions define what actions users can perform on a file or directory.
- Execute permission on a directory allows a user to enter and access that directory.
- Without execute permission, files inside the directory cannot be accessed—even if read permission exists.

- There are three permission types:
  
> **Read (r)** – view file contents  
> **Write (w)** – modify file  
> **Execute (x)** – run file or access directory


#### Permission Values (Octal Representation)

- Permission	Value

| Permission | Value |
|------------|-------|
| Read (r)   | 4     |
| Write (w)  | 2     |
| Execute (x)| 1     |


- Permissions are assigned as a 3-digit number:

Owner | Group | Others

#### Common Example
```

chmod 755 filename

```

#### Meaning:

- Owner → read, write, execute (7)

- Group → read, execute (5)

- Others → read, execute (5)

#### Symbolic Mode Examples

- chmod u+rwx filename
- chmod g+rx filename
- chmod o-r filename

- Linux permissions provide fine-grained access control. During my internship, I used chmod and chown to manage secure file access between users and services.

## 1.4 Can Two Users Have Different Permissions on the Same File?

Answer

Yes.


- This allows multiple users to interact with the same file with different access levels.

- This design enables secure collaboration in multi-user environments without duplicating files.

- Linux controls access using:

- **File ownership**
- **Group membership**
- **Permission bits**
  - User
  - Group
  - Others

