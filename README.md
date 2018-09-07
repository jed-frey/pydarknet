# pydarknet

pydarknet is a Python module for [Darknet, an open source neural network framework written in C and CUDA. It is fast, easy to install, and supports CPU and GPU computation](https://pjreddie.com/darknet/).



# Installation

Directly through ```pip```, in a virtual environment:

	python3 -mvenv venv
	source venv/bin/activate
	pip install --upgrade pip wheel setuptools # Upgrade pip, install wheel & setuptools.
	pip install git+https://github.com/jed-frey/pydarknet2.git#egg=pydarknet2

For development, using the examples, running tests, etc:

    git clone --recurse-submodules --jobs=8 https://github.com/jed-frey/pydarknet2.git
    cd pydarknet2
    make env # Create a local virtual environment.
    source venv/bin/activate # Activate the virtual environment.
    python3 setup.py build
    python3 setup.py develop

	# Run tests.
	pytest
	# Run jupyter notebook
	jupyter notebook
	#


# ```darknet.py``` Usage

```pydarknet``` includes a command line entry point to control darknet's source & weights. It is built with [Click](http://click.pocoo.org/) and should be fairly easy to use for those familiar with the command line.

```
$ darknet.py darknet --help
Usage: darknet.py darknet [OPTIONS] COMMAND [ARGS]...

  Manage local darknet folder.

Options:
  --help  Show this message and exit.

Commands:
  build  Build darknet.
  clone  Clone a darknet repository.
```

## Configuration

The default configuration values for ```pydarknet``` can be controlled through environmental variables. This allows you to have multiple darknet installations and builds:

    DARKNET_ROOT

Root directory for darknet, default: ```~/.darknet```

    DARKNET_CLONE_URL

URL to clone for darknet, default: ```https://github.com/jed-frey/darknet.git``` (relies on some changes to the Makefile)

    DARKNET_WEIGHT_DIR

Directory to the weights, default: DARKNET_ROOT/weights

    DARKNET_WEIGHT_URL

Base URL to where to download weights, default: ```https://pjreddie.com/media/files/```

If you already have ```libdarknet.so``` built and weights downloaded, you can put this in your ```.bashrc``` or however else you like to set environment variables. And skip the **Clone Darknet** and **Build Darknet** sections.

    export DARKNET_ROOT=/opt/darknet
    export DARKNET_WEIGHT_DIR=/opt/myweights/

## Clone Darknet.

- Use ```/tmp/darknet``` as the darknet directory.
- Use ```https://github.com/jed-frey/darknet.git``` [default] as repo URL.

All of these are functionally equivalent, different ways to do the same thing.

Using ```darknet.py``` entry point, using Click options.

    $ darknet.py darknet clone --root=/tmp/darknet --url=https://github.com/jed-frey/darknet.git

From within Python, explicitly setting the keyword arguments.

    $ python3
    >>> import pydarknet
    >>> dn = pydarknet.Darknet(root="/tmp/darknet")
    >>> dn.clone(clone_url="https://github.com/jed-frey/darknet.git")

Using ```darknet.py``` entry point using environment variables.

    $ export DARKNET_ROOT=/tmp/darknet
    $ export DARKNET_CLONE_URL=https://github.com/jed-frey/darknet.git
    $ darknet.py darknet clone

From within Python, entry point using environment variables.

    $ export DARKNET_ROOT=/tmp/darknet
    $ export DARKNET_CLONE_URL=https://github.com/jed-frey/darknet.git
    $ python3
    >>> import pydarknet
    >>> dn = pydarknet.Darknet() # Root read from env variables.
    >>> dn.root
    '/tmp/darknet'
    >>> dn.clone() # Clone URL read from env variables.

## Building Darknet.

Darknet compilation options are controlled through flags read by Makefile.

- Use ```/tmp/darknet``` as the darknet directory.
- Compile with OpenCV & OpenMP, without GPU. (Because my laptop doesn't have a GPU...)

All of these are functionally equivalent, different ways to do the same thing.

From the shell:

    $ cd /tmp/darknet
    $ export OPENCV=1
    $ export OPENMP=1
    $ make -j8

Oneliner:

    $ cd /tmp/darknet
    $ make -j8 OPENCV=1 OPENMP=1

Using ```darknet.py``` entry point, using Click options.

    $ darknet.py darknet build --opencv --openmp --root=/tmp/darknet

From within Python:

    $ python3
    Python 3.6.5 (default, Apr  1 2018, 05:46:30)
    [GCC 7.3.0] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>> import pydarknet
    >>> dn = pydarknet.Darknet(root="/tmp/darknet")
    >>> dn.build(opencv=True, openmp=True)
    >>> assert dn.exists

You can [should] check that the libdarknet shared library was built correctly:

    $ ldd libdarknet.so | grep -i openmp
        libopenmpt.so.0 => /usr/lib/x86_64-linux-gnu/libopenmpt.so.0 (0x00007f8946ba4000)

    $ ldd libdarknet.so | grep -i opencv
        libopencv_highgui.so.3.2 => /usr/lib/x86_64-linux-gnu/libopencv_highgui.so.3.2 (0x00007f482292e000)
    ...


## Using libdarknet




# Development.

- Use ```check.sh``` to check any source code.
- Use ```fix.sh``` to lazily fix most issues
- Use ```all_check.sh``` to check all pydarknet source code.
- Use ```all_fix.sh``` to lazily fix most issues in all pydarknet source code.


# Motivation

- I want to do image detection for my 2TB+ of photos I've taken, basically Example/
- Darknet provided the best 'batteries included' tutorial, with weights.
- The Python examples were Python2 and needed some prettification.
- Need to practice ctypes stuff.

# Issues

If it *doesn't* work, open an issue: https://github.com/jed-frey/pydarknet/issues/new

If you would like additional features, open an issue: https://github.com/jed-frey/pydarknet/issues/new



## Ignore

    export PATH=${PATH}:/usr/local/cuda-9.2/bin
    export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/cuda-9.2/lib64
