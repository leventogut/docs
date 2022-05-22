---
title: Kali Linux
summary: Kali Linux related tops.
authors:
    - Levent Ogut
date: 2018-07-10
tags:
    - Linux
    - Kali
    - Security
    - metapackages
---

## Kali suite install on Debian 11 via meta packages

Replace `kali-rolling` to your preferred branch.

```shell
echo 'deb https://http.kali.org/kali kali-rolling main contrib non-free' > /etc/apt/sources.list.d/kali.list
```

```shell
sudo wget -q -O – https://archive.kali.org/archive-key.asc | apt-key add
```

```shell
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com ED444FF07D8D0BF6 
```

```shell
sudo apt update && apt -y install kali-linux-headless
```

## References & further reading

- [Breakdown of Kali's Metapackages](https://www.kali.org/docs/general-use/metapackages/)
- [An explanation of Kali's branches](https://www.kali.org/docs/general-use/kali-branches/)
