# OpenSource Data Analytics Platform
### Based on Ubuntu 16.04


## 1. System package  installation

    sudo apt-get -y update

### 1.a Move Ubuntu Launcher Bar to bottom(Optional)

    gsettings set com.canonical.Unity.Launcher launcher-position Bottom

### 1.b Install VM-Tools(Optional, VM only)

    sudo apt-get install -y open-vm-tools*

### 1.c Install Openssh-Server  

    sudo apt-get install -y openssh-server

### 1.d Disable Guest login on Ubuntu
    echo "allow-guest=false" |sudo tee -a /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf

### 1.f Install Packages(Gcc/python/libblas etc ..)

    sudo apt-get -y install build-essential gfortran python3-dev python3-pip python-dev python-pip g++ libicu-dev libbz2-dev libblas3 libblas-doc libblas-dev liblapack3 liblapack-doc liblapack-dev build-essential autoconf automake gfortran libpcre3-dev libatlas-dev liblapack-dev libreadline6-dev texinfo texlive-base libfreetype6-dev libpng12-dev libjs-mathjax fonts-mathjax git python-qt4 qt4-designer pyqt4-dev-tools python-qt4-doc python-gtk2-dev

### 1.g Install Cisco VPN Client(Optional,For G-FW traversal :XD)

    sudo apt-get -y install network-manager-vpnc

#### 1.g.a Modify vpn configuration

    sudo vi /etc/vpnc/default.conf

    IPSec gateway *server-ip*
    IPSec ID *groupid*
    IPSec secret *group_psk*
    Xauth username *username*
    Xauth password *password*

#### 1.g.b Make connection

    sudo vpnc-connect
    sudo vpnc-disconnect

## 2.CUDA Installation(Optional,IFF you have Nvidia-Graphic-Card)

### 2.a Install nVidia CUDA Driver                      
    sudo add-apt-repository -y ppa:graphics-drivers/ppa
    sudo apt-get update
    sudo apt-get install -y nvidia-367
    sudo reboot

### 2.b Validation
    cat /proc/driver/nvidia/version
    sudo nvidia-smi

### 2.c Install CUDA

    wget http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda_7.5.18_linux.run
    sudo sh cuda_7.5.18_linux.run --override

#### 2.c.1 Dialog during install

    Do you accept the previously read EULA? (accept/decline/quit): accept
    You are attempting to install on an unsupported configuration. Do you wish to continue? ((y)es/(n)o) [ default is no ]: *yes*
    Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 352.39? ((y)es/(n)o/(q)uit): *no <--------NO DRIVER this step*
    Install the CUDA 7.5 Toolkit? ((y)es/(n)o/(q)uit): *yes*
    Enter Toolkit Location [ default is /usr/local/cuda-7.5 ]:
    Do you want to install a symbolic link at /usr/local/cuda? ((y)es/(n)o/(q)uit): *yes*
    Install the CUDA 7.5 Samples? ((y)es/(n)o/(q)uit): *yes*
    Enter CUDA Samples Location [ default is /home/kevin ]:
    Installing the CUDA Toolkit in /usr/local/cuda-7.5 ...

#### 2.c.2 Set environment parameters
    echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
    echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
    source ~/.bashrc

#### 2.c.3 Check CUDA
    nvcc -V

### 2.d Install  CUDNN
    wget http://developer.download.nvidia.com/compute/redist/cudnn/v4/cudnn-7.0-linux-x64-v4.0-prod.tgz
    tar xvf cudnn-7.0-linux-x64-v4.0-prod.tgz
    sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
    sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
    sudo chmod a+r /usr/local/cuda/lib64/libcudnn*


## 3. Install LAMP (Optional,if you want some Datavisualize on Web)
    sudo apt-get -y install vsftpd screen apache2 mysql-server  php-mysql php libapache2-mod-php php-mcrypt php-mbstring

### 3.a Install  MERN stack for Data Visualize(Recommend)
    MERN = MongoDB + Express(or koa later ) + React + Nodejs
    https://github.com/ogotaiking/data-visual-web-framework


## 4.Install pip

### 4.a Local mirror(Optional, IFF you in China:XD)
#### 4.a.1 Temp use in command
    pip -i http://pypi.douban.com/simple --trusted-host pypi.douban.com  

#### 4.a.2 set .pip file
    sudo mkdir ~/.pip
    sudo vi ~/.pip/pip.conf     
    <-append the following config->
    [global]
    index-url = http://mirrors.aliyun.com/pypi/simple/
    [install]
    trusted-host=mirrors.aliyun.com

## 5.Install packages
This step may consider both Python2 and Python3 environment

### Packages may include the Machine Learning Lib for the following area:
- Quant/Finance
- Network Security
- Spider
- Deep Learning

### 5.a Update pip
    sudo pip2 install --upgrade pip
    sudo pip3 install --upgrade pip
    sudo pip3 install --upgrade setuptools

### 5.b Numpy & pandas

    sudo pip2 install numpy scipy matplotlib cython numexpr sympy bottleneck pandas
    sudo pip3 install --upgrade cython  numpy scipy numexpr sympy bottleneck pandas pandas-datareader scikit-learn

### 5.c zipline

    sudo apt-get -y install libmysqlclient-dev
    sudo pip3 install mysqlclient SQLAlchemy zipline


### 5.d Misc dependency lib
    sudo apt-get -y install libzmq-dev libffi-dev libxml2-dev libssl-dev libtiff5-dev libjpeg8-dev zlib1g-dev libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev python-tk
    sudo pip3 install pyzmq  jinja2 tornado flask pyflux


### 5.e Jupyter
    sudo pip2 install jupyter
    sudo cp /usr/local/bin/jupyter-notebook /usr/local/bin/jupyter-notebook-py2
    sudo pip3 install jupyter
    sudo cp /usr/local/bin/jupyter-notebook /usr/local/bin/jupyter-notebook-py3

#### 5.e.1 Start Notebook in different version 

    python2 /usr/local/bin/jupyter-notebook --ip=0.0.0.0
    python3 /usr/local/bin/jupyter-notebook --ip=0.0.0.0

    or jupyter-notebook-py[2/3]

### 5.f statsmodels(TSA)/networkx(graph)/NLP/Flask(RESTAPI)/Plotly

    sudo pip3 install nose statsmodels  quandl pillow wordcloud scrapy django  paramiko moviepy networkx matplotlib nltk gensim snownlp flask-script flask-cors
    sudo pip3 install plotly flask-compress  hmmlearn arch blaze cvxopt
    sudo pip2 install plotly

### 5.g BS4    
    sudo pip3 install beautifulsoup4
    sudo pip2 install beautifulsoup4

### 5.h theano

    sudo pip3 install theano
    sudo pip2 install theano

### 5.i Graphviz / R / HDF5 / CSVtookit  / PYMC / Tushare   
    sudo apt-get -y install python-pygraphviz graphviz libgraphviz-dev pkg-config  r-base  littler  python-rpy python-rpy-doc r-base-dev vim vim-gnome
    sudo apt-get -y install sysstat
    sudo apt-get -y install libhdf5*
    sudo apt-get -y install liblzo*

    sudo pip3 install pymc tushare csvkit tables
    sudo pip2 install csvkit tables

### 5.j Haskell/Scala/boost for cpp / packet capture
    sudo apt-get -y install tshark wireshark
    sudo apt-get -y install libboost*
    sudo apt-get -y install ghc*
    sudo apt-get -y install scala*

### 5.k ta-lib
    wget http://prdownloads.sourceforge.net/ta-lib/ta-lib-0.4.0-src.tar.gz
    tar vzxf ta-lib-0.4.0-src.tar.gz
    cd ta-lib
    ./configure --prefix=/usr
    make;sudo make install
    cd ..
    wget https://github.com/mrjbq7/ta-lib/tarball/master
    mv master pytalib.tar.gz
    tar vzxf pytalib.tar.gz
    cd mrjbq7-ta-lib-e890769/
    sudo python3 setup.py install

#### 5.k.1 Ta-Lib reference

    http://mrjbq7.github.io/ta-lib/doc_index.html       

### 5.l Jieba     
    git clone https://github.com/fxsjy/jieba.git
    cd jieba
    wget https://github.com/fxsjy/jieba/raw/master/extra_dict/dict.txt.big
    cp dict.txt.big jieba/dict.txt
    sudo python3 setup.py install
    cd ..

### 5.m PYMC3    
    sudo pip3 install cairocffi
    sudo pip2 install cairocffi
    sudo pip3 install --process-dependency-links git+https://github.com/pymc-devs/pymc3
    sudo apt-get -y install libsuitesparse-dev
    sudo pip3 install git+https://github.com/njsmith/scikits-sparse.git


### 5.n sk-learn for pandas and seaborn    
    sudo pip3 install sklearn-pandas
    sudo pip3 install pyfolio
    sudo pip3 install seaborn


### 5.o Install Tensorflow   

    GPU Version:
    sudo pip3 install --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.10.0rc0-cp35-cp35m-linux_x86_64.whl

    CPU Version:
    sudo pip3 install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.10.0rc0-cp35-cp35m-linux_x86_64.whl

## 6. Optimization SSD    
    sudo vi /etc/fstab
    UUID=root-fs /               ext4    discard,noatime,nodiratime,errors=remount-ro 0 0
    tmpfs                        /tmp            tmpfs   defaults,noatime,mode=1777 0 0

## 7. VNC setup    
### 7.a install x11vnc
    sudo apt-get install x11vnc -y
    sudo x11vnc -storepasswd /etc/x11vnc.pass

### 7.b change config    
    sudo vi  /lib/systemd/system/x11vnc.service

    [Unit]
    Description=Start x11vnc at startup.
    After=multi-user.target
    [Service]
    Type=simple
    ExecStart=/usr/bin/x11vnc -auth guess -forever -loop -noxdamage -repeat -rfbauth /etc/x11vnc.pass -rfbport 12306 -shared
    [Install]
    WantedBy=multi-user.target

### 7.c service
    sudo systemctl enable x11vnc.service
    sudo systemctl daemon-reload

## 8. Install JAVA8    
    sudo add-apt-repository -y ppa:webupd8team/java
    sudo apt-get update
    sudo apt-get install -y oracle-java8-installer

    sudo update-alternatives --config java
    sudo vi /etc/environment
    add the following line
    JAVA_HOME="/usr/lib/jvm/java-8-oracle"

    source /etc/environment


## 9.Editor and Terminal

### 9.a TERMINATOR    
    sudo apt-get -y install terminator

    TERMINATOR is a fantastic Terminal                    
     ctrl+shift  T: Tab
                 E: split on right
                 I: new windoe
                 O: split on bottom


### 9.b Atom/Geany/Notepad++        
    sudo add-apt-repository -y ppa:webupd8team/atom
    sudo add-apt-repository -y ppa:notepadqq-team/notepadqq
    sudo apt-get update
    sudo apt-get -y install atom geany notepadqq

