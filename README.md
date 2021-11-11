
# Instructions on how to build a new gcc version

# First clone the gcc trunk
```
git clone git://gcc.gnu.org/git/gcc.git SomeLocalDir
```
# Change to the directory 
```
cd SomeLocalDir
```
# Compile dependencies
```
contrib/download_prerequisites
```
# Create a build directory
```
mkdir build
cd build
```
# Configure the build, notice the prefix and suffix
```
../configure -v --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=x86_64-linux-gnu --prefix=/usr/local/gcc-12.0.0 --enable-checking=release --enable-languages=c,c++,fortran --disable-multilib --program-suffix=-12.0
```
# Compile
```
make -j
```
# Move the binaries to the folder prefix points to
```
sudo make install
```
# Set these lines to to ~/.bashrc
```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/gcc-12.0.0/lib:/usr/local/gcc-12.0.0/lib64
export PATH=/usr/local/gcc-12.0.0/bin:$PATH
```

# If you want cmake to default to the new compiler also set these variables
```
export CC=/usr/local/gcc-12.0.0/bin/gcc-12.0
export CXX=/usr/local/gcc-12.0.0/bin/g++-12.0
export FC=/usr/local/gcc-12.0.0/bin/gfortran-12.0
```


# If experiencing weird GLIB errors during execution or compilation the standard library binary points to wrong file
# This command can be used to check which GLIB version does the library support
```
strings /some/lib/path/libstdc++.so.6 | grep GLIBCXX
```
# For me the command was
```
strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX
```

# Instead of trying to make the link point to correct file make sure that the LD_LIBRARY_PATH variable contains both
# the lib/ and lib64/ folders of the newly compiled compiler


# For (slightly) more check 
https://iamsorush.com/posts/build-gcc11/
