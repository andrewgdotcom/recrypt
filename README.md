# recrypt
Command line toolkit to re-encrypt PGP data

## quick-recrypt

Usage: quick-recrypt [ GPG_OPTIONS ]

Recrypt an OpenPGP-encrypted file or stream to a given recipient(s), using
current OpenPGP defaults. This is particularly useful for migrating data that
was encrypted with obsolete options, so that it can be safely used in future.

By default, input and output are STDIN and STDOUT respectively, although these
can be overridden with the standard GnuPG options "-d" and "-o".

A debug log is printed on STDERR.

GPG_OPTIONS is a list of GnuPG command-line options that will be passed to the
decryption and/or encryption processes. Since the majority of GnuPG options
apply only to either decryption or encryption, this normally does what one
expects. Only a subset of GnuPG options are supported:

Decryption:

    --decrypt, -d
    --no-mdc-error
    --no-crc-error
    --ignore-time-conflict
    --ignore-valid-from
    --skip-verify
    --try-secret-key
    --try-all-secrets

Encryption:

    --recipient, -r
    --hidden-recipient, -R
    --sign, -s
    --armor, -a
    --output, -o
    --throw-key-ids
    --for-your-eyes-only
    --set-filename
    --local-user, -u

Both:

    --no-default-keyring
    --keyring
    --secret-keyring
    --homedir
    --options
    --debug-level
    --debug
    --debug-all

The meaning of each of these options is the same as defined in the gpg manual.
Any option not on the above whitelist is ignored. Note that it is not currently
possible to use a different keyring for encryption than for decryption.

Also note that it is not possible (unlike gpg) to specify multiple flags in
a single word, e.g. `-sa`. Each flag must be given separately, i.e. `-s -a`.
