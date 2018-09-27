Contact - Update

#### UPDATE:

I've updated my PGP setup. I now use an explicit "master key".
The master key resides inside a network isolated VM and is only used
to sign my other keys.

Right now, I use (besides my master key) two keys:

1. The blog signing key
2. My (personal) Email key

#### My master key:

```
pub   rsa4096 2018-09-25 [C]
      6B5B 746D 864E 8C50 40BA  3DDF 3FC2 A82D EEFD BEC1
uid           [ultimate] Florian Brandes (Master Signing Key)
```
#### My blog signing key:

```
pub   rsa4096 2017-07-14 [SC] [expires: 2019-09-26]
      EBE9 6693 4FE9 D027 2253  A747 7350 8CAD 3E13 066C
uid           [ultimate] Florian Brandes (Blog signing key)
sub   rsa4096 2017-07-14 [E] [expires: 2019-09-26]
```

#### My email key:

```
pub   rsa4096 2018-09-22 [SC] [expires: 2019-09-26]
      6C7C 130D 706D F5A7 DB8A  C74F 8FD6 7E87 40FE ECA5
uid           [ultimate] Florian Brandes <florian.brandes@posteo.de>
uid           [ultimate] Florian Brandes <florian.brandes@gmx.de>
sub   rsa4096 2018-09-22 [E] [expires: 2019-09-26]
sub   rsa4096 2018-09-22 [S] [expires: 2019-09-26]
```


I also updated my email. I am moving to @posteo.de (which I consider a better
privacy oriented company). I am still registered at @gmx.de, but will only answer
from the new @posteo.de address.

I will update the keys accordingly on the keyservers and on keybase.io

Tags: contact, email, about, gpg
