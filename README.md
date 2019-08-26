
<p align="center">
  <img width="556" height="112" src="https://github.com/vidhanarya/GSoC2019-Orbitdeterminator/blob/master/src/logo.png">
</p>

#### Organisation: [AerospaceResearch.net](http://aerospaceresearch.net/)

#### Project: [OrbitDeterminator](https://github.com/aerospaceresearch/orbitdeterminator)

####  Mentors: <pre>     [Aakash Deep Chhonkar](https://github.com/aakash525)<br>     [Arya Das](https://github.com/aryadas98)<br>     [Nilesh Chaturvedi](https://github.com/Nilesh4145)
</pre>

### Overview

The **OrbitDeterminator** package provides tools to compute the orbit of a satellite from positional measurements. It supports both cartesian and spherical coordinates for the initial positional data, two filters for smoothing and removing errors from the initial data set and finally four methods to choose from for preliminary orbit determination. The package is labeled as an open-source scientific package and can be helpful for projects concerning space orbit tracking.

###  Objectives:

In this project, functionality to take input from a mix of ground stations and observations of the different format will be added to OrbitDeterminator. Key features of this project will be:


- An easy to use interface for the community where the existing methods and algorithms can be visualized
- A provision for the addition of observation inputs of all formats from a single station or a mix of many stations to OrbitDeterminator
- Development of standard input and output parameters to ensure the conversion of input data to our standard input format and its further processing
- Cleaning of the current codebase with the ultimate result of a proficient beta release having all the new features incorporated

### Code:
Following code has been submitted during the GSoC coding period:

Phase 1 - To give users the ability to choose filters and methods available for orbit determination, I have used "**Inquirer**" package of python as shown below.
<p align="center">
  <img width="640" height="410" src="https://github.com/vidhanarya/GSoC2019-Orbitdeterminator/blob/master/src/user-option.gif">
</p>

Previously, the data files were scattered in different directories of the project, so, I restructured the code such that all the input data files are placed in a single directory for input data. Users now need to put the data in the 'obs_data' directory to process their input files. Changes can be found in the commit below:

[Commit](https://github.com/aerospaceresearch/orbitdeterminator/commit/64cb12304b83294e6f4b562275a1cdf21fcc12eb) - Added options for the user to select plots of all the methods used in determining orbit and moved all the observational data in a single directory to improve the code structure

---

During phase 2, I was working on conversion scripts for input data from one format to another. This script includes parsing of data from input string but as I was working on the script, one of the community member Krishna Birla submitted a script for data parsing. So, I scrapped my work on data parsing script and continued development of format conversion on the script submitted by Krishna.
The R.D.E. format of satobs does not support range value observation so we could not add support for R.D.E. format in orbitdeterminator. 

**Format Detection Script**: I did not want the user to explicitly add the format of the file given as an argument for the program so I developed a script to detect the format of the program which worked in following ways:
A general U.K. format observation is `970120120181707191411226783000001119161509-1302496 1  50598270500667+6 +8   190R` here each column has defined meaning, similarly, a general I.O.D. format observation is `23794 96 010A   2701 G 20040506012614270 17 25 1100114-184298 38 I+020 10 01189`. For now, scripts have been developed such that cartesian [t, x, y, z] format data is accepted in '.csv' file format and observation data from satobs is accepted in '.txt' format. This is because most of the observations from satobs have an inconsistent number of columns in each line, so it will be problematic to process such observations in '.csv' file. To differentiate between observational formats, I made a simple program which returns format as cartesian if the number of column in each line is '4' (cartesian observation have fixed number of column i.e [t, x, y, z]). If the number of columns is greater than 4 and less than 80 (max no. of characters U.K. of I.O.D. format observation can have) than I checked if specific substring of the given observation is a valid date or not because I.O.D. format has column 24- 31 assigned for UTC date and U.K. format has column 12-17 assigned for date. Test cases for format detection were also added to check if the script is working as expected.

**Format Conversion**: After the format is detected, I added conversion methods for *convert_format.py* script. Converting satobs format to Cartesian format was an obvious choice for standard input since, our program was written keeping in mind [t, x, y, z] format of observations, moreover, it's easy to use x, y, z type of data for plotting and visualization purposes. For conversion of satobs format to cartesian, I defined a function for conversion of Longitude, Latitude and Altitude obtained from parsing of IOD/UK data to x, y and z data. For this conversion, I used earth as WGS84 object and used all the formula accordingly.

**Code structure**: I integrated *automated.py* with *main.py* script so that it can also be used from the main script with *--automate* or *-a* flag. I also added functionality to handle multiple files in multiple formats so that user don't have to manually resolve data from multiple files possibly in multiple formats. This was done by taking multiple file name as input with `python main.py --file file1 file2 file3`. Here program will process every file by first detecting its format, if not cartesian then converting the detected format to cartesian format and lastly appending all the data into a single array.
Changes can be found in the below pull request:

[Pull Request 1](https://github.com/aerospaceresearch/orbitdeterminator/pull/169) - Defined cartesian format as standard input, integrated automate script in main.py such that it can now be accessed with '-a or --automate' tag as `python main.py --automate`, added function to detect format of input file, convert the format to standard input of the program and give user the option to use multiple input files in multiple formats.

___

**Test Campaign**: During Phase 2 and some part of Phase 3, we organized a test campaign with Satobs community to observe NOAA satellites and use the observed data for testing of the program with known results. Emails and result of the test-campaign can be found in google drive shared folder [Emails](https://drive.google.com/drive/folders/17K1yleBHyZw_c_vOYpbwx5IJRmoQfz2i) and [observed data](https://drive.google.com/drive/folders/1BzrT5r9JB3B8IFZrp_yGsvQjq9qtCocz)
We didn't get the requisite data from members of satobs people and the data that Andreas recorded was for orbit determination using doppler shift pattern. So, I simulated the data for U.K. format of observation and used that data for testing of the program since results were already known to us.

**Testing and Code Structure**: Since the data that I used for simulation was impractical (perigee inside the surface of the earth), I had to use another format conversion function with the assumption of spherical coordinates. I still left the actual code with the earth as WGS84 object so that the user can use also use that function. I extensively tested execution of the program with U.K. formatted observation, multiple files in the same format, multiple files in a different format and also added test cases for the same. For better handling of the files, added separate function for file handling thus improving code structure of main.py script. I found that all the orbital elements were within 1% range of known results (99% accuracy), which can be further improved by using WGS84 assumption for code conversion.

**Install Scripts**: I built install scripts for macOS and Linux based systems, which automatically clones and install all the dependencies of the program. It also gives the user the ability to use the program with *orbitdeterminator* command such that `orbitdeterminator -f arg1 arg2 ...`. I also added a detailed instruction on *README.md* for the usage of the script.

**Remaining work**: Documentation build of the project hosted at "readthedocs" is failing because of "pykep" package used in the project. Pykep is only available at a limited number of platforms that is why we have to use separate install scripts for macOS and Linux. Usage guide for macOS/Windows is also different from Linux because of unavailability of Pykep on these platforms, this has caused "Conda" to be an indirect dependency for Windows and macOS operating systems. Currently, we do not have any alternative to pykep hence we need to manually write script for *lamberts_kalman* function that we are using from pykep.

[Pull Request 2](https://github.com/aerospaceresearch/orbitdeterminator/pull/175) - Added simulated data for U.K. format, added test cases for scripts from Pull Request 1, defined separate function for file handling to improve code structure, conducted extensive testing of the program for all the features added during GSoC and finally added easy to use install scripts for Unix based systems (Linux and macOS)

All the commits pushed to OrbitDeterminator project by me can be found [**here**](https://github.com/aerospaceresearch/orbitdeterminator/commits/dev?author=vidhanarya) (Includes commits to the library before GSoC started)

---

### Results:

Following is the summary of all the work done during GSoC:

- Improved documentation of the project with easy to follow guidelines and also added a contribution guide for new developers to help them set up the working environment
- Give the user the option to tweak all the possible filters and methods available for the filter with an easy-to-use interface. User can also save the filtered input data to his desired location
- Added support for I.O.D. and U.K. observational format which is used by the community such as satobs thus improving the reach of the program
- Added test cases, better file handling and improved overall structure of the codebase with all the functions now accessible from a single script (main.py)
- Built simple install scripts for Linux and macOS (Unix based systems) with detailed usage guide so that user can get a ready-to-use program with developing environment by executing a single script. Also, added an option for users to use the program with 'orbitdeterminator' command in the terminal by using the install scripts.

**Things I would like to work on after GSoC**:

- Remove the dependency on 'pykep' package which is not available for many systems
- Provide a setup file for Windows users for easy installation of the program
- Add support for the current location of the satellite-based on the timestamp data
- Built an interactive graphical user interface for the program to attract users with a non-technical background

### Conclusion:

It has been an awesome experience working on this project, I can't believe how much I learned during these 3 months of GSoC. Huge thanks to all my mentors for the help and guidance, I got to learn a lot from them. They always gave enough time to clear my doubts and helped me when I was stuck. Also, they gave a better direction to the project than what I was planning to go for. I also thank Andreas for helping us out by communicating with Satobs community on our behalf and always clearing whatever doubts I had. This project would have been a lot tougher and longer without you all to guide me. 

I strongly intend to continue contributing to **AerospaceReasearch.net** in all the ways I can, because it's just too much fun.
Also, thanks to **Google** for this amazing opportunity and the generous funding.
