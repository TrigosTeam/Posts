## Compilation error when installing R packages on Mac - gfortran not found

Sometimes we run into an error like this when installing R packages on Mac:

```R
make: /opt/R/arm64/bin/gfortran: No such file or directory
make: *** [mvt.o] Error 1
ERROR: compilation failed for package ‘mvtnorm’
```

This is because the package uses C and relies on gcc package installed on Mac to compile it. The following info might help with the installation.

---

### Make sure you have Xcode, homebrew and gcc installed on your laptop.

Check if Xcode is available.
```bash
xcode-select --install 
```

Check if homebrew is available. It is normally installed at the following path: `/opt/homebrew`

Sometimes you need to add the path of homebrew to PATH for it to work. Check if `/opt/homebrew/bin` is included in the PATH.
```bash
echo $PATH
```

If not, add the path.
```bash
export PATH=$PATH:/opt/homebrew/bin
```

Install homebrew if it's missing.
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Install gcc.
```bash
brew install gcc
```

### If you have everything above, but it's still not working.
We need to add the path of fortran to R so that R can find it.

Go to `~/.R` folder. Create one if you don't have it. 
```bash
mkdir ~/.R
```

Create a file `Makevars` in the folder .R and put the following lines in.
FC = /opt/homebrew/Cellar/gcc/12.2.0/bin/gfortran
F77 = /opt/homebrew/Cellar/gcc/12.2.0/bin/gfortran
FLIBS = -L/opt/homebrew/Cellar/gcc/12.2.0/lib/gcc/12

### Try now
Restart Rstudio and install the package.
