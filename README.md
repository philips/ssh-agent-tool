# ssh-agent OAUTH 2.0 bridge

Nearly everyone who does server infrastructure has an ssh key. This is a strong
public-key based identity. Not only that but it is often the only identity that
is available when people remote into a host via ssh thanks to "ssh-agent
forwarding".

This tool lets you play with the ssh-agent and shows how it can be used to sign
and verify challenge-responses. Eventually I want to use this code as an
identity exchange from say SSH public key to OAUTH 2.0 JWT.

## Manual

### List Public Keys

```
$ ssh-agent-tool list-keys
00: ssh-rsa	AAAAB3NzaC1yc2EAAAA...y+1EoNzsPkY2aw==	/Users/philips/.ssh/id_rsa
01: ssh-rsa	AAAAB3NzaC1yc2EAAAA...oYVfmKAMjmKkV	/Users/philips/.ssh/id_rsa_old
```

### Sign

```
$ ssh-agent-tool sign $(echo foobar | base64)
00: ssh-rsa fStlJeCQW8...Glu5ZHNuc=
01: ssh-rsa gtLeSrdor...rgQb9Qp/SDw==
```

### Challenge Verify

```
$ ./ssh-agent-tool verify $(echo foobar | base64) ssh-rsa fStlJeCQW8...Glu5ZHNuc=
00: verified: true
01: verified: false
$ ./ssh-agent-tool verify $(echo foobar | base64) ssh-rsa gtLeSrdor...rgQb9Qp/SDw==
00: verified: false
01: verified: true
```
