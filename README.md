# Symbolic Analysis of Threshold Signatures

This repository contains symbolic models from the paper _"An Extended Hierarchy of Security Notions for Threshold Signature Schemes and Automated Analysis of Protocols That Use Them"_ by _Anonymous Author(s)_.

## Content

```
.
├── README.md
├── dkg                 # Distributed Key Generation (DKG)
│   ├── dkg_kc.spthy    # DKG with key commitment
│   ├── dkg_pop.spthy   # DKG with proof-of-possession
│   ├── dkg_rka.spthy   # DKG with rogue-key attacks
│   └── tactics
└── tss                 # Threshold Signature Schemes (TSS)
    ├── tactics
    └── threshold_signature.spthy
```

## Dependencies

All models are built for the [Tamarin prover](https://tamarin-prover.com/) and tested with version 1.8.0.

```
$ tamarin-prover --version
tamarin-prover 1.8.0, (C) David Basin, Cas Cremers, Jannik Dreier, Simon Meier, Ralf Sasse, Benedikt Schmidt, 2010-2023

This program comes with ABSOLUTELY NO WARRANTY. It is free software, and you are welcome to redistribute it according to its LICENSE, see 'https://github.com/tamarin-prover/tamarin-prover/blob/master/LICENSE'.

maude tool: 'maude'
 checking version: 2.7.1. OK.
 checking installation: OK.
Generated from:
Tamarin version 1.8.0
Maude version 2.7.1
Git revision: 4ae2db93af83e4d0ac3105fd29ae90a4c3555512, branch: master
```

## Flags

We use Tamarin's built-in pre-processor to generate models of different threshold signature schemes. Specifically, flags can be used to (1) choose attribute levels in our hierarchy, and (2) choose between accountable and privacy-preserving signatures.

For simplicity, we write _Attr. = y:x_ as _Attr.-y_. Therefore, e.g., _SiGu = aLRhPP-4_ is referred to as _SiGu-4_.

**Signer Guarantees (SiGu)**

| SiGu | Flag      | Model    |
|:----:|:---------:|:---------|
|   0  | -D=SIGU_0 | TSS      |
|   1  | -D=SIGU_1 | TSS      |
|   2  | -D=SIGU_2 | TSS      |
|   3  | -D=SIGU_3 | TSS      |
|   4  | -D=SIGU_4 | TSS      |

**State Corruption (Corr)**

| Corr | Flag      | Model    |
|:----:|:---------:|:---------|
|   0  | (Default) | DKG, TSS |
|   1  | -D=CORR_1 | TSS      |
|   2  | -D=CORR_2 | TSS      |
|   3  | -D=CORR_3 | DKG      |

**Key Generation Channels (KGCh)**

| KGCh | Flag      | Model    |
|:----:|:---------:|:---------|
|   0  | (Default) | DKG, TSS |
|   1  | -D=KGCH_1 | DKG      |
|   2  | -D=KGCH_2 | DKG      |

**Signing Channels (SiCh)**

| SiCh | Flag      | Model    |
|:----:|:---------:|:---------|
|   0  | (Default) | TSS      |
|   1  | -D=SICH_1 | TSS      |
|   2  | -D=SICH_2 | TSS      |

**Unforgeability (UF)**

|  UF     | Flag       | Model    |
|:-------:|:----------:|:---------|
| SUF-CMA | (Default)  | TSS      |
| EUF-CMA | -D=EUF_CMA | TSS      |

**Privacy**

| Type               | Flag               | Model    |
|:-------------------|:-------------------|:---------|
| Accountable        | ACCOUNTABLE        | TSS      |
| Privacy-preserving | PRIVACY_PRESERVING | TSS      |


For example, to generate a privacy-preserving {0,1,0,2}-EUF-CMA-secure signature scheme, we use the following command:

```
$ tamarin-prover threshold_signature.spthy -D=SIGU_0 -D=CORR_1 -D=SICH_2 -D=PRIVACY_PRESERVING -D=EUF_CMA
```
