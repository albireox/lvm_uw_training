# Using SSH keys on Windows

First, install WSL by opening Powershell as administrator (Windows key -> type in Powershell, right click and run as administrator).

Then, run

```console
wsl --install
```

to install [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install). Once the installation is complete, open Ubuntu (Windows key -> type in Ubuntu). It'll take a moment to finish installing, then will ask you to generate a new username and password.

Once you're in the terminal, generate a public + private SSH key with

```console
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_lvm_yourusername
```

You don't need to enter a password in for this step.

Type `ls ~/.ssh` to see the two keys - the key ending in `.pub` is your public keyfile. Copy it to your Windows Documents folder with

```console
cp ~/.ssh/id_lvm_yourusername.pub /mnt/c/Users/<your-user>/Documents/
```

and send it to José. Create a SSH config file with

```console
touch ~/.ssh/config
```

then open it in a terminal text editor with

```console
nano ~/.ssh/config
```

Paste the following into that file (Ctrl+V or right-click), replacing `id_lvm_yourusername` with your key filename.

```console
Host lvm-sdss5-hub
    HostName lvm-hub.lco.cl
    User sdss5
    ForwardAgent yes
    ProxyCommand ssh -i ~/.ssh/id_lvm_yourusername -W  %h:%p sdss5@sdss5-gateway.lco.cl
    IdentityFile ~/.ssh/id_lvm_yourusername
Host lvm-observer
    IdentityFile ~/.ssh/id_lvm_yourusername
    User observer
    HostName 10.8.38.27
    ProxyCommand ssh -A -W %h:%p lvm-sdss5-hub
    LocalForward 59000 10.8.38.27:5901
    LocalForward 18888 camaras-02.lco.cl:443
```

Then Ctrl+O to write and Ctrl+X to quit. Finally, change the permissions of your SSH files with

```console
chmod 600 ~/.ssh/*
```

Once José adds the files to the server, try `ssh`-ing by opening Ubuntu and typing `ssh lvm-observer` to see if it worked.
