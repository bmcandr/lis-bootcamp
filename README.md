# LIS Boot Camp

This repository contains a collection of information intended to rapidly orient new members of NASA's Land Information System (LIS) team. <!--just team members or new users altogether?-->

<img src='images/LIS_logo-FINAL.png' width='25%' align='right'>

## Overview of NASA's Land Information System

NASA's Land Information System (LIS) is a software framework for high performance terrestrial hydrology modeling and data assimilation developed with the goal of integrating satellite and ground-based observational data products and advanced modeling techniques to produce optimal fields of land surface states and fluxes. LIS is used to study land surface processes and land-atmosphere interactions, using "best available observations" to force and constrain the models. The LIS framework also enables a number of applications including:

* weather and climate model initialization
* water resources management
* natural hazards management

The LIS framework is comprised of three components:

* **Land Information System (LIS)** - the modeling and data assimilation framework
* **Land Data Toolkit (LDT)** - supports the data preprocessing needs for LIS (e.g., parameter data processing, data assimilation support, forcing bias correction)
* **Land Verification Toolkit (LVT)** - environment for model benchmarking and evaluation

<img src='images/LIS-components.png' align='center'>

<!--expand this section -->

For more background on LIS, see the following articles:

<!-- add links -->
* [Kumar et al. (2006): Land Information System: An interoperable Framework for High Resolution Land Surface Modeling, Environmental Modeling and Software, Vol 21, pp 1402-1415.](http://prhouser.com/houser_files/Kumar2006.pdf)

* [Peter-Lidard et al. (2007): High-performance earth system modeling with NASA/GSFC’s Land Information System, Innovations in Systems and Software Engineering, 3(3),157-165.](http://prhouser.com/houser_files/LIS2007.pdf)

-----

## LIS Bootcamp Documents

<!-- add description of these documents? -->

* [Glossary of Terms and Concepts](LIS-glossary.md)
* [Getting Started with LIS on NCCS Discover](guides/LIS-on-NCCS-discover.md)

-----

## Important Links

* [LIS Website](https://lis.gsfc.nasa.gov/)
    * Documentation
        * [LIS Documentation](https://lis.gsfc.nasa.gov/documentation/lis)
            * Planning to contribute? Check out the LIS Developers' Guide before you get started.
        * [LDT Documentation](https://lis.gsfc.nasa.gov/documentation/ldt)
        * [LVT Documentation](https://lis.gsfc.nasa.gov/documentation/lvt)
    * [Frequently Asked Questions](https://lis.gsfc.nasa.gov/faq-page)
* [LIS GitHub Page](https://github.com/NASA-LIS)
    * This is where the LIS source code lives!
* [NASA's Modeling Guru Forum](https://modelingguru.nasa.gov/community/atmospheric/lis)
    * Need help with LIS? Search the forum and ask for help!

-----

### Contributing to the LIS Boot Camp

This is a living document and we welcome your contributions! To contribute, please follow the workflow described in the [Working with GitHub](https://github.com/NASA-LIS/LISF/blob/master/docs/working_with_github/working_with_github.adoc) documentation to fork, clone, and initiate a pull request with your changes.

This document is created using Markdown styling which is automatically rendered on GitHub and easily converted to PDF or other filetypes. Check out the [GitHub Guide to Markdown](https://guides.github.com/features/mastering-markdown/) and/or this [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) for more information.

Please follow these style conventions for consistency:

* Generally follow GitHub-flavored Markdown conventions
* Indentation = 4 spaces
* Use asterisks (\*) to create list items
* Limited in-line HTML is acceptable (e.g., for images, super/subscript)
