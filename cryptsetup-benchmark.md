```bash
[zen@arch ~]$ cryptsetup benchmark
# 测试仅使用内存（无存储 IO）。PBKDF2-sha1      2962079 iterations per second for 256-bit key
PBKDF2-sha256    6413308 iterations per second for 256-bit key
PBKDF2-sha512    2369663 iterations per second for 256-bit key
PBKDF2-ripemd160 1219274 iterations per second for 256-bit key
PBKDF2-whirlpool 1083239 iterations per second for 256-bit key
argon2i      12 iterations, 1048576 memory, 4 parallel threads (CPUs) for 256-bit key (requested 2000 ms time)
argon2id     12 iterations, 1048576 memory, 4 parallel threads (CPUs) for 256-bit key (requested 2000 ms time)
#     Algorithm |       Key |      Encryption |      Decryption
        aes-cbc        128b      1825.5 MiB/s      6882.8 MiB/s
    serpent-cbc        128b       124.8 MiB/s       925.8 MiB/s
    twofish-cbc        128b       269.8 MiB/s       604.8 MiB/s
        aes-cbc        256b      1404.6 MiB/s      5988.1 MiB/s
    serpent-cbc        256b       128.3 MiB/s       927.3 MiB/s
    twofish-cbc        256b       275.8 MiB/s       605.6 MiB/s
        aes-xts        256b      5648.6 MiB/s      5665.7 MiB/s
    serpent-xts        256b       808.0 MiB/s       834.4 MiB/s
    twofish-xts        256b       556.6 MiB/s       567.6 MiB/s
        aes-xts        512b      5120.9 MiB/s      5125.6 MiB/s
    serpent-xts        512b       820.0 MiB/s       833.2 MiB/s
    twofish-xts        512b       559.2 MiB/s       566.0 MiB/s
```
