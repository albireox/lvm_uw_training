# lvm_uw_training

## Getting ready to observe with LVM

### Share your public SSH key

An [SSH key](https://www.ssh.com/academy/ssh-keys) is necessary to access the LVM servers.

If you don't have one, you can generate one using the following steps (this should work for most distributions of Linux and macOS but not directly for Window):

1. Open a new terminal
2. Create a directory for your SSH keys: `mkdir -p ~/.ssh`
3. Run the following command to generate a new SSH key (you can replace `<your-uw-id>` with your UW ID or anything else you want to identify your key with):

    ```bash
    ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_lvm_<your-uw-id>
    ```

4. When asked for a passphrase, you can leave it empty if you want to avoid typing it every time you use the key. If you want to add a passphrase, you will be asked to type it twice.
5. Check that the pair of public and private ssh keys were created in the `~/.ssh` directory:

    ```bash
    $ ls ~/.ssh
    total 216
    -rw-------   1 gallegoj  staff   3.3K Nov 16 12:11 id_lvm_gallegoj
    -rw-r--r--   1 gallegoj  staff   745B Nov 16 12:11 id_lvm_gallegoj.pub
    ```

6. Share the *public* (the one ending in `.pub`) key by dropping it [here](https://www.dropbox.com/request/nC5nA1NKQJqJo0oaG6ui). Sharing your public SSH key is always safe but never share your private key with anybody.
7. Create a `config` file in the `~/.ssh` directory with the following content (or add this content to an existing `config` file):

    ```bash
    Host lvm-obssever
        IdentityFile ~/.ssh/id_lvm_<your-uw-id>
        User observer
        HostName 10.8.38.27
        ProxyCommand ssh -A -W %h:%p lvm-hub
        LocalForward 59000 10.8.38.27:5901
        LocalForward 18888 camaras-02.lco.cl:443
    ```

8. Once your public key is added to the server (this may take a few days), you can access the server by running the following command:

    ```bash
    ssh lvm-observer
    ```

    The SSH connection will also forward some port from the LVM observer machine to your local computer so you can access the VNC and the site webcams (more on this later).

If you already have an SSH key that you want to use, you don't need to create a new one, just send us that one but please make sure it's an RSA key.

If you are using Windows, you can try following [these instructions](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement) to generate an SSH key, using an app like [PuTTY](https://www.putty.org), or installing the Windows Subsystem for Linux (WSL). Please note that we can't provide support for Windows because we don't have a lot of experience with it. Ultimately you need to be able to generate an RSA key pair and be able to use it to SSH and forward the above ports with it.

### VNC Viewer

Although you may not need to use this often, there is a remote machine in the LVM servers to which you can connect via VNC (a form of remote desktop). To do that you first need to SSH to the `lvm-observer` machine and then use a VNC client. In macOS you can use the built-in Screen Sharing app. Otherwise we recommend using [RealVNC](https://www.realvnc.com/en/connect/download/viewer/).

1. Install the VNC Viewer app.
2. Open the app.
3. Create a new conneciton going to `File` -> `New connection...`.
4. In the `VNC Server` field, type `localhost:59000`. Give the connection a name if you want.
5. Click `OK` to save the connection.
6. Double click the connection to connect to the remote machine. Make sure you have SSH'd to the `lvm-observer` machine before trying to connect via VNC.
7. You should now see a desktop environment that you can interact with.

### Sign up for the SDSS-V Slack

Communication with the on-site observers and between LVM observers is done through the SDSS-V Slack. You can also use this to ask questions to the LVM team, coordinate observations, and to see notifications and critical alerts from the Overwatcher system.

1. An invitation will be sent to your email. Follow the instructions to sign up.
2. Once you are signed up, you can access the Slack workspace [here](https://app.slack.com/) or add it to the Slack app.
3. Make sure you join the following channels:

    - `#lvm-core-observations-team`
    - `#lvm-dupont-observing`
    - `#lvm-notifications`
    - `#lvm-overwatcher`
    - `#lvm-alerts`
4. If you need to ask questions, especially if something happens during observing that you don't know how to handle, you can ask in the `#lvm-core-observations-team` channel. If nobody responds, try DMing `@gallegoj` or `@ejohnston`.

## Resources

- [LVM WebApp](https://lvm-web.lco.cl)
- [LVM Observer Guide](./LVM-LVM%20observing%20guide-161124-200209.pdf)
- [Overwatcher User Guide](./COS-Overwatcher's%20Guide%20to%20the%20Galaxy%20(the%20Universe,%20and%20Everything%20Else)-161124-200137.pdf)
- [Video on observing with the Overwatcher](https://www.dropbox.com/scl/fi/nt9m76bmbc8q22s6sv6i0/Overwatcher_Overview_video.mp4?rlkey=el8j8yq3hd52zvf83x2w9ki84&dl=0)
