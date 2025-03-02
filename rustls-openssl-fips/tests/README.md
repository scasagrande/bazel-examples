The contents of this folder have been taken from rustls-openssl with only minor modifications.

The changes were mostly around using ring instead of aws-lc-rs for some tests. This was done just for simplicity
of the demo in order to avoid having to compile aws-lc-sys. To accommodate this change, some tests were tweaked
or skipped inorder to avoid cryptographic algorithms that ring does not support. 

Source: https://github.com/tofay/rustls-openssl/tree/0.2.0/tests
