```bash
gpg --gen-key  # generate new key
gpg --list-keys # list public-addresses of public-keys stored on machine 
gpg --list-secret-keys # list public-addresses of secret-keys stored on machine
gpg --delete-keys <public_address> # delete public key stored on machine
gpg --delete-secret-keys <public_address> # delete secret key stored on machine 
gpg --edit-key <gpg-public-key> # remove key expiration
>expire      # `>` represents you are in gpg cli
>0   (for never expire)
>save

# ------------ encrypt and decrypt ------------------------------
# create a new file named `ssh1.zip.zip` by encrypting `ssh1.zip.gpg` file
gpg -e -r <gpg_public_key> ssh1.zip  # -e==encrypt, -r==recipient

# this will only work if you have gpg_private_key (public_key of which is used to encrypt the file) stored on machine
gpg -d -o ssh2.zip ssh1.zip.gpg  # -d==decrypt, -o==output 
``

# ------------ export and import ------------------------------
# export from machine 1
gpg --output public.gpg --armor --export <gpg-public-key>
# --output  name of exported file
# --armor   ascii armored output
# --export  export-keys
gpg --output private.pgp --armor --export-secret-key <gpg-public-key>

# import on machine 2
gpg --import public.pgp  # if you have imported private-key, public-key will be generated automatically
gpg --import private.pgp
```

- GnuPG (GNU Privacy Gaurd) is a complete and free implementation of the OpenPGP standard defined by PGP
