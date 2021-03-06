This pages describes manually installing Trilium on your server.

## Requirements

Trilium is a [node.js](http://nodejs.org/) application. Supported (tested) version of node.js is latest 10.X.X (note that node.js 10 older than 10.5.0 are buggy and don't work).

You can check your node version with this command (node.js needs to be installed):
~~~~
node --version
~~~~

If your linux distribution has only outdated version of node.js, you can take a look at the [[installation instructions|https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions]] on node.js website which covers most popular distributions.

### Dependencies

There are some dependencies required mainly to compile image support. You can see command for Debian and its derivatives (like Ubuntu) below:

~~~~
sudo apt install libpng16-16 libpng-dev pkg-config autoconf libtool build-essential nasm libx11-dev libxkbfile-dev
~~~~

## Installation 

### Download
You can either download source code zip/tar from [[latest release|https://github.com/zadam/trilium/releases/latest]] or clone git repository from stable branch with ```git clone -b stable https://github.com/zadam/trilium.git```

## Installation
~~~
cd trilium

# download all node dependencies
npm install
~~~~

## Run

~~~~
cd trilium

# using nohup to make sure trilium keeps running after user logs out
nohup node src/www &
~~~~

Application by default starts up on port 8080, so you can open your browser and navigate to http://localhost:8080 to access Trilium (replace "localhost" with your hostname).

## TLS

Don't forget to [[configure TLS|TLS configuration]], which is required for secure usage!