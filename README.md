# The Simplified Futuristic Connectivity Protocol

The Simplified Futuristic Connectivity Protocol is a networking protocol reusing the
[core concepts of the FCP/`cjdns`](https://github.com/cjdelisle/cjdns/blob/master/doc/Whitepaper.md),
but stripping as much complexity as possible from the protocol, to make
it easier to implement -- and entirely documented.

The address space is left unspecified rather than enforced to be fc00::/8 at
the protocol level.

## Cryptographic layer

The [CryptoAuth](https://github.com/cjdelisle/cjdns/blob/master/doc/Whitepaper.md#the-cryptoauth)
specification is reuse as-is.
I believe it to already be the minimalist design to fulfill its goals.

## Switch layer

The Switch Header is changed from this layout:

```
                    1               2               3
    0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
 0 |                                                               |
   -                          Route Label                          -
 4 |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
 8 |   Congest   |S| V |labelShift |            Penalty            |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

to this one:

```
                    1               2               3
    0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
 0 |                                                               |
   -                                                               -
 4 |                                                               |
   -                           Route Label                         -
 8 |                                                               |
   -                                                               +
12 |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
16 |                            Reserved                           |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

The Route Label expansion is to deal with horizon limits observed in
cjdns/Hyperboria.

The Reserved bytes must be set to 0x00.

Other fields are moved because they were not used except for the
labelShift. The labelShift becomes unnecessary with the longer
Route Label.

## Routing Layer

The concept of Encoding Scheme is removed from the protocol, because it
was used as a way to deal with horizon limits.
