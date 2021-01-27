---
title: "PinebookDiaries: glueing Pandoc"
date: 2021-01-13T16:34:43+01:00
author: "svemagie"
tags: [sysadmin, pinebook, markdown]
---

My trusted [PinebookPro](https://www.pine64.org/pinebook-pro/) won't install [pandoc](https://pandoc.org/) per default. 

Pandoc is [Haskell](https://www.haskell.org/), and Haskell isn't (binary-)available on Arch Linux / Manjaro based Distributions. But there is an debian-arm64 Version of it, so let's install it. It's a bit of an hack, so pacman and yay will complain later. I'll figure out how to avoid it - or even better: how to do it correctly.


1. install some tools and depencies:
	
	``` 
	sudo pacman -Sy texlive-core # texconfig; needed to get pdflatex working
	sudo pacman -Sy texlive-bin  # pdflatex; needed to create pdf's with pandoc
	sudo pacman -Sy rpmextract
	sudo pacman -Sy dpkg

2. install libpcre as dependency of pandoc
 
	```
	mkdir libpcre
	cd libpcre
	wget http://ftp.altlinux.org/pub/distributions/ALTLinux/Sisyphus/aarch64/RPMS.classic/libpcre3-8.44-alt1.aarch64.rpm
	rpmextract.sh libpcre3-8.44-alt1.aarch64.rpm
	cd lib64
	sudo cp * /usr/lib
	sudo ldconfig
        # ldconfig complains, but it seems to work...
        # ldconfig: /usr/lib/libtranscript.so.1 is not a symbolic link
        # ldconfig: /usr/lib/libpcreposix.so.3 is not a symbolic link
        # ldconfig: /usr/lib/libpcre.so.3 is not a symbolic link

	cd ../..
	
3. fetch the pandoc binaries *.deb from an debian mirror:

	``` 
	wget http://ftp.br.debian.org/debian/pool/main/p/pandoc/pandoc-data_2.9.1.1-3_all.deb
	wget http://ftp.br.debian.org/debian/pool/main/p/pandoc/pandoc_2.9.2.1-1+b1_arm64.deb

4. install pandoc_data

	```
	dpkg -i pandoc-data_2.9.1.1-3_all.deb
	
6. install pandoc_bin
 
	```
	mkdir pandoc_bin
	cd pandoc_bin
	wget https://debian.pkgs.org/11/debian-main-arm64/pandoc_2.9.1.1-3_arm64.deb.html
	# the bin does not install due to dependency problems (although it executes fine), so unpack and copy manually
	ar x pandoc_2.9.1.1-3_arm64.deb
	unxz data.tar.xz
	tar -xvf data.tar
	sudo cp ./usr/bin/pandoc /usr/bin	
	

\* found on https://forum.manjaro.org/t/installing-pandoc-on-pinebook-pro/13763/12
