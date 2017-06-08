# Caffe-tutorial
Caffe installation and some basic tutorials 


## Installation
This installation focus on the CPU version caffe.
I ran these steps on my macOS 10.12.5 and connected the caffe with python 2.7 which is build-in mac

### Brew install
Brew is a very convenient package manager on mac.

" Homebrew installs the stuff you need that Apple didn't" quoted from Brew's website.

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

### Dependency install

```
brew install -vd snappy leveldb gflags glog szip lmdb
brew install hdf5 opencv
brew upgrade libpng
brew tap homebrew/versions
```

### Edit OpenCV installation file

Key the following code in the terminal

`brew edit opencv`

and replace 

```
args « "-DPYTHON#{py_ver}_LIBRARY=#{py_lib}/libpython2.7.#{dylib}"
args « "-DPYTHON#{py_ver}_INCLUDE_DIR=#{py_prefix}/include/python2.7"
```

With

```
args « "-DPYTHON_LIBRARY=#{py_prefix}/lib/libpython2.7.dylib"
args « "-DPYTHON_INCLUDE_DIR=#{py_prefix}/include/python2.7"
```

### Install protobuf 2.6.1

You can choose where to install this file on your own. 
I prefer to install my dependency at my root.

```
  curl -LO https://github.com/google/protobuf/releases/download/v2.6.1/protobuf-2.6.1.tar.bz2
  tar xvjf protobuf-2.6.1.tar.bz2  
  rm protobuf-2.6.1.tar.bz2  
  cd protobuf-2.6.1  
  ./configure
   make  
  make check  
  sudo make install  
  cd python  
  sudo python2.7 setup.py build  
  sudo python2.7 setup.py install
```


### Install dependencies

```
brew install --build-from-source -vd boost159 boost-python159
brew link --force boost159
```

If you encounter the problem that you don't have permission to unlink or like 

`Homebrew: Could not symlink, /usr/local/bin is not writable`

You have to key in the following code to link your boost.

```
sudo chown -R `whoami`:admin /usr/local/bin
sudo chown -R `whoami`:admin /usr/local/share
```

### Clone Caffe

```
git clone https://github.com/BVLC/caffe.git` 
cd caffe
git checkout 20feab5771ae5cbb257cfec85e0b98da06269068
cp Makefile.config.example Makefile.config
```



### Change the Command Line Tool (CLT) 

We have to change the Command Line Tool, log in apple developer website and download Xcode Command Line Tools for 7.3. Link:

[https://developer.apple.com/downloads/](url)

After installation, key in the following code to change the CLT.

`
sudo xcode-select --switch /Library/Developer/CommandLineTools
`

### Make


```
cmake -DCPU_ONLY=ON
make -j8 all
sudo -H easy_install pip
```


If encounter the situation, `pkg_resources.DistributionNotFound:`

`
sudo python -m easy_install pip
`


```
pip install --user -r python/requirements.txt
sudo -H pip install protobuf
make -j8 pytest
```


When enconter `python2 command not found`

`cd caffe/CMakeFiles/pytest.dir/build.make`

find `python2` and replace it into `python`



### Set Path
Key the following line in the terminal to guide python to use the correct package path:

`
export PYTHONPATH=~/caffe/python:$PYTHONPATH   (your path to caffe/ python)
`


### Try it

`python -c 'import caffe' `

If there is nothing happened, congrats you finish the installation.