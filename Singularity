Bootstrap: library
From: ubuntu:20.04

%environment
	export PATH=$PATH:/meme/bin:/meme/libexec/meme-5.5.2:/samtools/bin

%post
	apt update && apt install -y \
		wget \
		build-essential \
		zlib1g-dev \
		libncurses5-dev \
		libbz2-dev \
		liblzma-dev \
		libcurl4-openssl-dev \
		git \
		python2

	# Make python2 the default & access with 'python' alias
	update-alternatives --install /usr/bin/python python /usr/bin/python2 1
	update-alternatives --config python

	# Install CRC-mapper python dependencies
	wget https://bootstrap.pypa.io/pip/2.7/get-pip.py # Fetch get-pip.py for python 2.7 
	python2 get-pip.py
	pip install numpy scipy networkx

	# Download & install MEME suite
	wget https://meme-suite.org/meme/meme-software/5.5.2/meme-5.5.2.tar.gz
	tar -xf meme-5.5.2.tar.gz
	cd meme-5.5.2
	./configure --prefix=/meme --enable-build-libxml2 --enable-build-libxslt
	make
	make install
	cd ..

	# Download & install Samtools
	wget https://github.com/samtools/samtools/releases/download/1.17/samtools-1.17.tar.bz2
	tar -xf samtools-1.17.tar.bz2
	cd samtools-1.17
	./configure --prefix=/samtools
	make
	make install
	cd ..

	# Download and install crcmapper
	git clone https://github.com/mireia-bioinfo/CRCmapper.git

%test
	samtools --version
	meme -version
	python --version
	pip --version
	python /CRCmapper/CRCmapper.py -h

%runscript
	cd /CRCmapper
	exec python CRCmapper.py "$@"

