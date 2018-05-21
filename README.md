# recrypt
Command line toolkit to re-encrypt PGP data

## quick-recrypt

Usage: quick-recrypt [ GPG_OPTIONS ]

Recrypt an OpenPGP-encrypted file or stream to a new recipient. By default,
input and output are STDIN and STDOUT respectively, although these can be
overridden with the standard GnuPG options "-d" and "-o".

A debug log is printed on STDERR.

GPG_OPTIONS is a list of GnuPG command-line options that will be passed to the
decryption and/or encryption processes. Since the majority of GnuPG options
apply only to either decryption or encryption, this normally does what one
expects. Only a subset of GnuPG options are supported:

Decryption:

    --no-mdc-error
    --no-crc-error
    --allow-multiple-messages
    --ignore-time-conflict
    --ignore-valid-from

Encryption:

    --recipient, -r
    --hidden-recipient, -R
    --sign, -s
    --armor, -a
    --output, -o
    --throw-key-ids
    --for-your-eyes-only
    --set-filename
    --default-key
    --default-recipient-self

Both:

    --no-default-keyring
    --keyring
    --secret-keyring
    --homedir
    --options
    --debug-level
    --debug
    --debug-all

Any option not on the above whitelist is ignored. Note that it is not currently
possible to use a different keyring for encryption than for decryption.
