Design parameters
=================

recrypt
-------

* stream-based
    * can we use multiple pipes to pass data and metadata between de/en halves?
* decrypt operation piped to encrypt operation
* command-line flags to override defaults in decrypt and encrypt stages (separately)
* encrypt operation should encrypt to same recipient keys as original message
    * what happens if not all recipient keys are available?
* optional stdout filter (e.g. pipe through sed)
* optional (?) signature operation over recrypted data using local privkey
* signed recrypt log

```
                                              ,-------------------,--> stderr
stdin --> decryptor --stderr--> stderr-mangler --args--> encryptor --> stdout
                   `--stdout--> stdout-filter? --stdin---^
```

The stdin, stdout and stderr of the overall process can be diverted to files.

stderr-mangler takes the metadata output of the decryptor and uses it to
construct parameters for the encryptor. This only works for metadata that is
output at the start of the decryption process. Any other parameters *must* be
provided. We should also be able to override the parameters on the command line
(e.g. if we want to encrypt to a different key).

The recryptor is effectively the stderr-mangler. If we write this in perl,
we can open a coprocess for the decryptor, buffer the stdout pipe while we read
the leading metadata, construct the encryptor command and then connect the
decryptor stdout pipe to the encryptor stdin.

stdout-filter applies a stream filter module to the plaintext between the
decryptor and the encryptor. This means that the new file is not equivalent
to the old, and must be used with extreme caution. DO NOT IMPLEMENT YET

recrypt-maildir
---------------

* wrapper for recrypt
* scans a maildir for specified badness (e.g. obsolete encryption options)
* pipes bad mime parts through recrypt
* appends recrypt log as extra mime part (signed?)
* (optionally?) appends original as extra mime part
* (optionally) rewrites mime structure to sanitise exfiltration attacks

How do we manage passphrases? Let the agent handle it, or cache ourselves?
This could get very annoying if signing of messages does not cache. Or if we
are using a smartcard, then there's not much we can do...
