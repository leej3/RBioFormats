# R interface to Bio-Formats

*RBioFormats* is an R package which provides an interface to the OME [Bio-Formats](https://github.com/ome/bioformats) Java library. It facilitates reading of proprietary image data and metadata in R.

## Installation

First, make sure you have [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 1.8 or higher installed.
To install *RBioFormats* use the `biocLite` installation script in order to resolve the dependency on the Bioconductor package *[EBImage](http://biocondcutor.org/packages/EBImage)*.

```r
if (!require("BiocManager", quietly=TRUE)) install.packages("BiocManager")
BiocManager::install("aoles/RBioFormats")
```

### Mac OS X

Mac OS comes with a legacy Apple Java 6. In order to use *RBioFormats*, you will need to update your Java installation to a newer version provided by Oracle.

1. Install [Oracle JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html).

2. Update R Java configuration by executing from the command line (you might have to run it as a super user by prepending `sudo` depending on your installation).
```
R CMD javareconf
```

3. Re-install *rJava* from sources in order to properly link to the non-system
Java installation.
```r
install.packages("rJava", type="source")
```

You can verify your configuration by running the following commands. This should return the Java version string corresponding to the one downloaded and installed in step 1.

```r
library(rJava)
.jinit()
.jcall("java/lang/System", "S", "getProperty", "java.runtime.version")
## [1] "1.8.0_112-b16" 
```

## Documentation

To get started with using *RBioFormats* have a look at the [package vignette](https://rawgit.com/aoles/RBioFormats/master/vignettes/RBioFormats.html) .

### FAQ

See my [answers on Stack Overflow](http://stackoverflow.com/search?q=user:A2792099+rbioformats) for solutions to some common and less common questions.

### Caveats

#### The `java.lang.OutOfMemoryError` error

If you get the `java.lang.OutOfMemoryError: Java heap space` error, try increasing the maximum heap size by supplying the -Xmx parameter before the Java Virtual Machine is initialized. For example, use

```r
options( java.parameters = "-Xmx4g" )
library( "RBioFormats" )
```

to override the default setting and assign 4 gigabytes of heap space to the Java environment.

Information about the current Java heap space limit can be retrieved by `checkJavaMemory()`.

#### Use with BiocParallel

Each R process needs a separate JVM instance. For this, load the package in the parallelized function, e.g.,

```r
bplapply (files, function(f) {
  library(RBioFormats)
  ...
})
```
