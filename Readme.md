Create `group_vars/all` and change default role variables

Generate certificate files with:

    openssl req -x509 -batch -nodes -days 3650 -newkey rsa:2048 -keyout forwarder.key -out forwarder.crt -subj /CN=<hostname of ELK server goes here>
