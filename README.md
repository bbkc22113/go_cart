
<p align="center">
<img src ="sample_images/logo-trimmed.jpg" width="65%">
<img src ="sample_images/detail.jpg" width="62%">
</p>

<p align="center">
<img src ="sample_images/example_cartograms.jpg" width="45%">
</p>


<hr>

[![release](http://github-release-version.herokuapp.com/github/Flow-Based-Cartograms/go_cart/release.svg?style=flat)](https://github.com/Flow-Based-Cartograms/go_cart/releases/latest)
[![Maintenance](https://img.shields.io/badge/maintained-yes-green.svg)](https://GitHub.com/Naereen/StrapDown.js/graphs/commit-activity)
[![HitCount](http://hits.dwyl.io/Flow-Based-Cartograms/go_cart.svg)](http://hits.dwyl.io/Flow-Based-Cartograms/go_cart)
[![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?url=https%3A%2F%2Fgithub.com%2FFlow-Based-Cartograms%2Fgo_cart&text=Create%20your%20own%20cartograms%20today%20-%20and%20see%20what%20your%20world%20really%20looks%20like%21&hashtags=data%2Cmap%2Ccartogram%2Ctech)


We present a fast <a href="https://en.wikipedia.org/wiki/Cartogram" target="\_blank" title="What are Cartograms?">cartogram</a> generator written in C. It uses the flow-based algorithm devised by <a href="https://doi.org/10.1073/pnas.1712674115" target="\_blank">Gastner, Seguy & More</a>.

This readme explains how to set up and use this software. In doing so, it uses data from the United States presidential election, 2016. This data is included in the repository.

**Note:** Any images generated using this software should be referenced to:

Gastner, M., Seguy, V., & More, P. (2018). Fast flow-based algorithm for creating density-equalizing map projections. *Proceedings of the National Academy of Sciences USA*, **115**:E2156-E2164.

**BibTeX Entry:**

```
@article{gastner_seguy_more_2018,
	title={Fast flow-based algorithm for creating density-equalizing map projections},
	author={Gastner, Michael T. and Seguy, Vivien and More, Pratyush},
	DOI={10.1073/pnas.1712674115},
	journal={Proceedings of the National Academy of Sciences of the United States of America},
	year={2018},
	volume = {115},
	number = {10},
	pages = {E2156--E2164}
}
```

## Input data format
The cartogram expects two input files:

1. a `.gen` file containing the Cartesian coordinates for the map regions under consideration. For example, for the 2016 US presidential election data we provide `usa_low48splitME_conic.gen` (in the `sample_data` folder) which includes the coordinates for the boundaries of the different states. Here is a description of the file format. The content of the file is in the left block of text. The right block of text is a line-by-line explanation that is not part of the `.gen` file.

    ```
    2302 Maine02              ID for region followed by optional description.  
    0.302204 -0.188090        Pairs of x-, y-coordinates. Orientation along  
    0.302716 -0.187835        outer boundaries must be clockwise. If a  
    ...                       polygon has a hole, the inner boundary must be  
    0.303897 -0.193159        anticlockwise.  
    0.302204 -0.188090        Regions are not permitted to overlap. This is  
    END                       *not* checked by this code!  
    2301 Maine01  
    0.333358 -0.200693  
    ...  
    0.333358 -0.200693  
    END  
    2301 Maine01              IDs can repeat if a region consists of  
    0.334699 -0.204771        multiple polygons.  
    ...  
    0.334699 -0.204771  
    END                       Each polygon terminates with END.  
    END                       One more END signals the end of the file.
    ```

2. a `.dat` file containing the data (such as population) for each region, according to which these will be scaled. For the 2016 US presidential election data we provide `usa_electors.dat` (also in the `sample_data` folder) which provides the number of electors for each state.

<!-- We can add some explanation about the files here if needed -->

## Building the cartogram generator

### Dependencies

#### macOS
You must have `Xcode Command Line Tools` and the `brew` package manager <a href="https://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/" title="How to install Xcode CLT & brew?" target="\_blank">installed</a> (and updated) on your computer.

#### Ubuntu/Debian Linux
No additional dependencies. Your default `apt-get` package manager should work fine.

### Building (macOS & Linux)

1. Open a terminal, clone the repository, and navigate to its root directory.

2. Run the provided automatic build script (you may need to grant sudo priveleges):
```
chmod a+x autobuild.sh && ./autobuild.sh
```

3. You should have an executable by the name of `cartogram` in your root directory.

**Note:** If you run into issues, look at the [**Troubleshooting**](#troubleshooting) section below.

## Running the cartogram generator

1. Navigate to the directory with the `cartogram` executable. By default, this should be the root directory of the project.

2. Run the executable using the following command format:
```
./cartogram <.gen file> <.dat file>
```
For the provided 2016 US presidential election data, run the following command:
```
./cartogram sample_data/usa_low48splitME_conic.gen sample_data/usa_electors.dat
```

**Note:** In our repository, we also include data for India and China's GDP, segmented by their states/provinces.

To generate the India GDP cartogram, run:
```
./cartogram sample_data/india_noLD_conic.gen sample_data/india_gdp.dat
```

To generate the China GDP cartogram, run:
```
./cartogram sample_data/china_withSARandTWN_conic.gen sample_data/china_gdp.dat
```

3. You should see two generated files - `map.eps` showing the original map, and `cartogram.eps` showing the generated cartogram.

On macOS, open using
```
open <filename>.eps
```
On Linux, open using
```
evince <filename>.eps
```
Replace `<filename>` with the name of the file (`map` or `cartogram`) you wish to open.

For our 2016 US election example, `map.eps` should look like:

<p align="center">
<img src ="sample_images/US_map.png" width="65%">
</p>

`cartogram.eps` should look like:

<p align="center">
<img src ="sample_images/US_election_2016_cartogram.png" width="65%">
</p>

## Troubleshooting
### Error: Don't understand 'm' flag!

If you see the following line in your output, you may want to take a look at <a href="https://github.com/dmlc/xgboost/issues/1945" target="\_blank">this solution</a>. Once you have implemented the solution from the link, follow the standard build instructions above.
```
FATAL:/opt/local/bin/../libexec/as/x86_64/as: I don't understand 'm' flag!
```

### Manually building the generator

In case you run into problems while building the generator using the automated script, you could try manually carrying out some of the steps.

#### macOS
1. Install fftw and gcc.
```
brew install fftw && brew link fftw
brew install gcc
```
2. Note down the version of `gcc` installed. For example, if `gcc-7.1.0` is installed, your version would be `7` (not 7.1.0).

3. Go into the source code directory and open the `Makefile`.
```
cd cartogram_generator
vi Makefile
```
4. Change the line `CC = gcc` to `CC = gcc-[your-version-number]` where '_your-version-number_' is the number you noted above. Then save the file using the vi command `:wq`.

5. Go back to the root directory.
```
cd ..
```

6. Run the remaining steps.
```
chmod a+x scripts/semi_autobuild.sh && scripts/semi_autobuild.sh
```

#### Ubuntu/Debian Linux
1. Make sure everything is up to date. Then install fftw and gcc.
```
sudo apt-get update
sudo apt-get install libfftw3-3 libfftw3-dev
sudo apt-get install build-essential
```

2. Make sure you are in the root directory, and then run the remaining steps.
```
chmod a+x scripts/semi_autobuild.sh && scripts/semi_autobuild.sh
```
