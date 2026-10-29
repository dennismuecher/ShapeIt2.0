# ShapeIt 2.0

[![Latest release](https://img.shields.io/github/v/release/dennismuecher/ShapeIt2.0)](https://github.com/dennismuecher/ShapeIt2.0/releases)

This software allows extracting the gamma-ray strength function for a given nucleus, based on a 2D matrix of gamma-ray energy vs excitation energy. The matrix is assumed to be in units of keV.

ShapeIt 2.0 runs as a web application: the analysis backend is written in C++ and served via ROOT's RWebWindow infrastructure, while the user interface runs in your browser with ROOT histograms rendered via JSROOT.

To run ShapeIt 2.0, change to the `ShapeIt2.0/` directory and say `root WebShapeIt.cxx`. The interface will open automatically in your default browser.

The documentation of ShapeIt 2.0 is here: https://dennismuecher.github.io/ShapeIt2.0/

Currently, ShapeIt 2.0 is tested using ROOT 6.40.02 under macOS 14.5 (Sonoma). Please report any bugs or incompatibilities with different operating systems or ROOT versions, thank you!

Dennis
