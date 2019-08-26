<p align="center">
  <img width="556" height="112" src="https://github.com/vidhanarya/GSoC2019-Orbitdeterminator/blob/master/src/logo.png">
</p>

#### Organisation: [AerospaceResearch.net](http://aerospaceresearch.net/)

#### Project: [OrbitDeterminator](https://github.com/aerospaceresearch/orbitdeterminator)

####  Mentors: <pre>     [Aakash Deep Chhonkar](https://github.com/aakash525)<br>     [Arya Das](https://github.com/aryadas98)<br>     [Nilesh Chaturvedi](https://github.com/Nilesh4145)
</pre>

### Overview

The **OrbitDeterminator** package provides tools to compute the orbit of a satellite from positional measurements. It supports both cartesian and spherical coordinates for the initial positional data, two filters for smoothing and removing errors from the initial data set and finally four methods to choose from for preliminary orbit determination. The package is labeled as an open source scientific package and can be helpful for projects concerning space orbit tracking.

###  Objectives:

In this project, functionality to take input from a mix of ground stations and observations of different format will be added to OrbitDeterminator. Key features of this project will be:


- An easy to use interface for the community where the existing methods and algorithms can be visualized
- A provision for the addition of observation inputs of all formats from a single station or a mix of many stations to OrbitDeterminator
- Development of standard input and output parameters so as to ensure the conversion of input data to our standard input format and its further processing
- Cleaning of the current codebase with the ultimate result of a proficient beta release having all the new features incorporated

### Code:

Following code has been submitted during GSoC coding period:

[Commit](https://github.com/aerospaceresearch/orbitdeterminator/commit/64cb12304b83294e6f4b562275a1cdf21fcc12eb) - Added options for user to select plots of all the methods used in determining orbit and moved all the observational data in a single directory to improve the code structure

[Pull Request 1](https://github.com/aerospaceresearch/orbitdeterminator/pull/169) - Defined cartesian format as standard input, integrated automate script in main.py such that it can now be accesed with '-a or --automate' tag as `python main.py --automate`, added function to detect format of input file, convert the format to standard input of the program and give user the option to use multiple input files in multiple formats.

Organised a test campaign with Satobs community to observe NOAA satellites and report the observed data for testing. Emails and result of the test-campaign can be found in google drive shared folder [Emails](https://drive.google.com/drive/folders/17K1yleBHyZw_c_vOYpbwx5IJRmoQfz2i) and [observed data](https://drive.google.com/drive/folders/1BzrT5r9JB3B8IFZrp_yGsvQjq9qtCocz)

[Pull Request 2](https://github.com/aerospaceresearch/orbitdeterminator/pull/175) - Added simulated data for U.K. format, added test cases for scripts from Pull Request 1, defined separate function for file handeling to improve code structure, conducted extensive testing of the program for all the features added during GSoC and finally added easy to use install scripts for unix based systems (Linux and macOS)

All the commits pushed to OrbitDeterminator project by me can be found [**here**](https://github.com/aerospaceresearch/orbitdeterminator/commits/dev?author=vidhanarya) (Includes commits to the library before GSoC started)

### Conclusion:

It has been an awesome experience working on this project, I can't believe how much I learned during these 3 months of GSoC. Huge thanks to all my mentors for the help and guidance, I got to learn a lot from them. They always gave enough time to clear my doubts and helped me when I was stuck. Also, they gave a better direction to the project than what I was planning to go for. I also thank Andreas for helping us out by communicating with Satobs community on our behalf and always clearing whatever doubts I had. This project would have been a lot tougher and longer without you all to guide me. 

I strongly intend to continue contributing to **AerospaceReasearch.net** in all the ways I can, because it's just too much fun.
Also, thanks to **Google** for this amazing opportunity and the generous funding.
