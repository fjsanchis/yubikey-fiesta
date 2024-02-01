# Generate GPG key

## Delete folder
```
rm -rf ~/.gnupg/  
rm -rf ~/Desktop/*
```

## Create password key
```
mkdir ~/Desktop/my_keys
openssl rand -base64 24 > ~/Desktop/my_keys/passwd.txt
```

## Generate key
```
gpg --expert --full-generate-key
   
   $ (8) RSA (set own capabilities)
   $ (s) Sign Certify Encrypt
   $ (e) Certify Encrypt
   $ (q) Certify
   $ size 4096
   $ 0 (no expiry)
   $ y
   $ y

   Insert name and email

   $ (o) OK

   For password copy and paste previous generated password with openssl
```

## Generate SubKeys
```
gpg --expert --edit-key {email registered on the key}
  
  gpg> addkey
    $ (4) Sign only
    $ size 4096
    $ 3y (expire)
    $ y
    $ y
    Paste password key (openssl)

  gpg> addkey
    $ (6) RSA Encrypt only
    $ size 4096
    $ 3y (expire)
    $ y
    $ y
    Paste password key (openssl)

  gpg> addkey
    $ (8) RSA own capabilities
    $ s
    $ e
    $ a 
    $ q
    $ size 4096
    $ 3y (expire)
    $ y
    $ y
    Paste password key (openssl)


```

# Export Keys
```
gpg --armor --export-secret-keys {email} > ~/Desktop/my_keys/master.key
gpg --armor --export {email} > ~/Desktop/my_keys/pub.key
gpg --armor --export-secret-subkeys {email} > ~/Desktop/my_keys/sub.key
gpg --fingerprint --fingerprint {email} >  ~/Desktop/my_keys/fingerprint-txt
```

# Revocation certificate
```
gpg --gen-revoke {email} > ~/Desktop/my_keys/revoke.asc
  $ y
  $ 0
  $ y
```

# Test
```
mv ~/Desktop/my_keys ~/.gnupg/
gpg --list-secret-keys
```

# Restore key
```
rm -rf ~/.gnupg/   

```


# Configure key (regular pc)
```
sudo apt install scdaemon yubikey-manager libpam-yubico libpam-u2f libu2f-udev

ykman info

ykman openpgp info

ykman openpgp keys set-touch -h
ykman openpgp keys set-touch enc on
ykman openpgp keys set-touch sig on
ykman openpgp keys set-touch aut on

```