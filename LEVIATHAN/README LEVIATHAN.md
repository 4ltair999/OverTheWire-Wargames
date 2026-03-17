
___________________

# Leviathan Index

### GitHub

- [Level 0](https://www.google.com/search?q=%23level-0)
- [Level 1](https://www.google.com/search?q=%23level-1)
- [Level 2](https://www.google.com/search?q=%23level-2)
- [Level 3](https://www.google.com/search?q=%23level-3)
- [Level 4](https://www.google.com/search?q=%23level-4)
- [Level 5](https://www.google.com/search?q=%23level-5)
- [Level 6](https://www.google.com/search?q=%23level-6)

---

# Level 0

- In this exercise we have to inspect one of the files that are closest to us.
    

![[Pasted image 20260124012610.png]]

### password:

```
3QJ3TgzHDq
```

---

# Level 1

- In this exercise we work with an **ELF binary**.
    
    -(The **ELF format (Executable and Linkable Format)** is a **binary file standard** mainly used in Unix and Linux operating systems.)
    

```
./check: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=990fa9b7d511205601669835610d587780d0195e, for GNU/Linux 3.2.0, not stripped
```

- When we inspect it with `ls -l` we can see that it has the **SUID bit enabled**.

```
-r-sr-x---   1 leviathan2 leviathan1 15084 Oct 14 09:27 check
```

- This already opens a door for us to execute this binary as the file owner and obtain the password.
- We are going to use the **ltrace** tool.


![[Pasted image 20260124012610.png]]

### password:

```
NsN1HwFoyN
```

#### Why use ltrace?

- `ltrace` focuses on tracing the calls that a program makes to dynamic libraries during its execution.  
    It is widely used because it provides a lot of details related to errors made in these interactions.

- In this exercise, we analyzed the execution flow by providing an incorrect password so that `ltrace` could do its job and give us all the information about this interaction, revealing where the real password is.


---

# Level 2

## Version 1 (escape method)

- For this exercise we take advantage of a **Command Injection**, noticing that we are dealing with an **ELF binary** like in the previous exercise, but this time it runs a **cat** command that reads the files we pass to it.

![[Captura de pantalla 2026-01-25 044822.png]]

- With this understood, we need to design a method to bypass that `cat` and execute a suitable exploit.

- For this, it is necessary to use **mktemp -d** to create a directory in which we will create a file with the following content:

```
'file;bash'
```

- After that, we execute the binary with the following command:

```
./printfile /tmp/tmp.KzHoUcHNKA/file\;bash
```

- First, we are calling the binary. Then we pass the path to our temporarily created file, but with one exception: our file `file;bash` is written as `file\;bash`.
- We use the backslash as an **escape**, which helps close the first command and start a second one (all thanks to `;` 😉), allowing `bash` to be executed.
- With a shell obtained as **leviathan3**, we can extract the password.

![[Captura de pantalla 2026-01-25 040049.png]]

### password:

```
f0n8h2iWLP
```

## Version 2 (symlink method)

- For this exercise we use a vulnerability called **Argument Splitting** or, more technically, **Word Splitting**, and additionally we make use of a **symlink**.
- As in the previous exercise, we are dealing with an ELF binary that prints the contents of a file using `cat`. With this in mind, we are going to bypass that `cat`.
- First, we need to understand two very important concepts:

### 1️⃣ Argument Splitting / Word Splitting

- When we leave a space in a **shell**, it can be interpreted as the end of an instruction.

> For the shell, a space is not just a character — it is the signal that says:  
> **“Here one instruction/file ends and the next one begins.”**

### 2️⃣ Symlink (symbolic link)

- A **symlink** is a special file that contains a reference to another file or directory.
- With the above understood, we can now define the attack path:


![[Captura de pantalla 2026-01-25 213302.png]]

- First, inside **/tmp/** (in our own directory), we need to create the **symlink** that will point to the path where the password is stored.

```
ln -s /etc/leviathan_pass/leviathan3 /tmp/gabs/test
```

- With this set, we need to create another file that will act as bait using **Argument Splitting**. Let’s see what happens:

- When you type something like **"test 2".txt** in the terminal, if it is not properly sanitized, the shell will interpret it as two instructions:  
    `test 2.txt` and then just `test`.

- This allows `test` to reference the password we need, tricking the shell and extracting it.

```
system("/bin/cat /tmp/holaa/test 2.text" /bin/cat: /tmp/holaa/test: No such file or directory
```

### password

```
f0n8h2iWLP
```

---

# Level 3

- This exercise presents us with a binary that asks for a password to proceed. We will analyze it using **ltrace**.

![[Captura de pantalla 2026-01-26 031949.png]]

- Here we notice something very important: it uses **strcmp()**.

### strcmp()

- The **`strcmp`** function compares two strings lexicographically.  
    If `strcmp(hola, snlprint)` returns **-1**, it means the first string is **less than** the second one.

- This happens because the function compares the ASCII values of each character. If the first character differs, it continues comparing subsequent characters until it finds a difference.

- Thanks to **ltrace**, we can deduce that there was an initial attempt where `strcmp()` compared the main key with an incorrect input (visible by the `-1` result).

- Later, we see a second `strcmp()` that fails because it gives us the opportunity to input the same value that is already there, causing it to compare against itself and break the binary logic.

![[Captura de pantalla 2026-01-26 031350.png]]

- When it matches, not only does it succeed, but it also gives us a **shell**.

- With this shell, we just need to take advantage of the **SUID privilege** we obtained by analyzing the binary. We execute the binary again, but this time we append a `cat` command:

```
cat /etc/leviathan_pass/leviathan4
```

![[Captura de pantalla 2026-01-26 030816.png]]

- And it works! We get the password:

```
WG1egElCvO
```

_____________

# Level 4

- The challenge consisted of performing a **conversion from raw binary data to plain text (ASCII)** to obtain the credential.

![[Captura de pantalla 2026-01-26 051413.png]]

- We use:

```
https://cyberchef.org/
```

![[Captura de pantalla 2026-01-26 051617.png]]

### Password:

```
0dyxT7F4QD
```

---

# Level 5

- In this exercise we are presented with a binary that fails while trying to load a **.log** file.

```
leviathan5@leviathan:~$ ./leviathan5  Cannot find /tmp/file.log
```

- We will exploit this as an attack vector by linking `/tmp/file.log` to the password file using a symlink.

![[Captura de pantalla 2026-01-27 014344.png]]

```
ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
```

- Once this is done, we run the binary again to see if it works:

```
leviathan5@leviathan:~$ ./leviathan5
szo7HDB88w
```

- There we have the password:

```
szo7HDB88w
```

### Real-world application:

- In this exercise we deal with a very important vulnerability called a **symlink race condition**, which works by creating a link from a **predictable path** in a **globally writable directory** to a location that contains sensitive information such as passwords.


---

# Level 6

- In this exercise we are presented with a binary that asks us for **4 digits**.

- Initially, I analyzed it using **strace** and **ltrace** to understand how the binary works. Since I did not find any special filters, I chose to analyze its responses to different inputs.

![[Captura de pantalla 2026-01-27 164821.png]]

- If we pay attention, we see that if our input contains letters, the output is `=0`, but if we only use numbers, it reflects `=1111`.

- This gives us the idea that the binary only interprets numeric input.

    With this deduced, and knowing that there are **no filters**, **no anti-bruteforce mechanisms**, and **no rate limiting**, we opt for a **bruteforce attack**.

- We use a **bash script** for this:

```
for i in {0000..9999}; do echo -ne "\rProbando: $i"; ./leviathan6 $i 2>&1 | grep -v "Wrong" && echo -e "\n¡GANASTE! Código: $i" && break; done
```

![[Captura de pantalla 2026-01-27 163310.png]]

- As we can see, once we find the correct 4 digits, it elevates us to **leviathan7**. From there, we just need to retrieve the password:

```
qEs5Io5yM8
```

---

# Level 7

![[Captura de pantalla 2026-01-27 172219.png]]