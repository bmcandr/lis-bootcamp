# Glossary of Terms/Concepts

<!--Add new items in appropriate place to maintain alphabetic order!-->

The following terms and concepts will likely be encountered as you work with LIS. *This document is still in the information collection phase, edits for clarity and brevity to come...*

* **Batch Script**
    * a series of commands to be executed by the command-line interpreter, stored in a plain text file. A batch file may contain any command the interpreter accepts interactively and use constructs that enable conditional branching and looping within the batch file, such as IF, FOR, and GOTO labels. The term "batch" is from batch processing, meaning "non-interactive execution", though a batch file may not process a batch of multiple data.
    * Source: <https://en.wikipedia.org/wiki/Batch_file>
* **Bias**
    * **Bias correction**
    * **Bias correction algorithm**
        * **Bias-blind**
            * **Standard normal deviate-based scaling**
            * **CDF matching**
        * **Bias-aware**
* **Calibration**
    * Overview (highway traffic modeling): <http://www.dot.state.mn.us/trafficeng/modeling/workshop/C09-ModelCalibration.pdf>
* **Cold start**
* **Coupled**
    * Overview: <https://www.star.nesdis.noaa.gov/ngmmf/documents/Coupled_Modeling_Tutorial.pdf>
        >In Earth-system models, the term "coupling" is often used in two ways. The first is "offline" coupling, where output from one model is passed to another model for computation of some variable. The second is "online" coupling, where the feedbacks are allowed to pass between the two models. Online coupling is synonymous with "fully coupled".
        >
        >A good example is when wildfires are modeled with coupled meteorology-chemistry models. Historically, the aerosol emissions from the fire would be modeled by a separate chemistry model that only used the meteorology model as an input (e.g. offline coupling). However, now you can fully couple (e.g. online coupling) the chemistry with the weather such that the heat and aerosol released from the fire perturbs the meteorological/radiation calculations in the next iterated time step.
        * Source: <https://earthscience.stackexchange.com/questions/4792/meaning-of-coupling-in-modelling>
* **Data assimilation**
    * a mathematical discipline that seeks to optimally combine theory (usually in the form of a numerical model) with observations
        * <https://en.wikipedia.org/wiki/Data_assimilation>
    * In the LIS framework, the Land Data Toolkit (LDT) provides data assimilation functionality
    * **Assimilation algorithms**
        * **EnKF (Ensemble Kalman Filter)**
        * **EKF (Extended Kalman Filter)**
        * **EnKS (Ensemble Kalman Smoother)**
        * **DI (Direct insertion)**
    * **Analysis increments**
    * **Innovations**
    * **Measurement model**
    * **Forward model**
    * **Measurement error standard deviation**
* **Data compression**
    * (this has been a recent learning experience in the Air Force project)
    * Reducing file sizes?
* **Debugging**
    * The process of finding and resolving defects or problems within a computer program that prevent correct operation of computer software or a system.
        * Source: <https://en.wikipedia.org/wiki/Debugging>
    * There are a variety of tools used in debugging, including:
        * [TotalView](https://totalview.io/products/totalview) - debugging software that provides the specialized tools you need to quickly debug, analyze, and scale high-performance computing (HPC) applications. This includes highly dynamic, parallel, and multicore applications that run on diverse hardware — from desktops to supercomputers.
        * [valgrind](https://valgrind.org/) - an instrumentation framework for building dynamic analysis tools. There are Valgrind tools that can automatically detect many memory management and threading bugs, and profile your programs in detail.
        * "Old school" print statements - embed print statements in your code to output variables and status messages to the console
* **Ensembles**
    * **Downscale**
    * **Upscale**
    * **Number of ensembles**
    * **Ensemble mean**
    * **Ensemble spread**
* **ESMF**
    >The **E**arth **S**ystem **M**odeling **F**ramework (ESMF) is high-performance, flexible software infrastructure for building and coupling weather, climate, and related Earth science applications. The ESMF defines an architecture for composing complex, coupled modeling systems and includes data structures and utilities for developing individual models.
    >
    >The basic idea behind ESMF is that complicated applications should be broken up into coherent pieces, or components, with standard calling interfaces. In ESMF, a component may be a physical domain, or a function such as a coupler or I/O system. ESMF also includes toolkits for building components and applications, such as regridding software, calendar management, logging and error handling, and parallel communications.
    * Source: <https://www.earthsystemcog.org/projects/esmf/>
* **Fluxes**
* **Forcings**
    * what it means and list of requirements)
    * From LIS 7.0 User Guide:
        >The boundary conditions describing the (upper) atmospheric fluxes are known as "forcings". LIS makes use of model derived data as well as satellite and ground-based observational data as forcings. The land surface models are typically run using model derived data. The observational data are used to overwrite the model derived data, whenever they are available.
    * **Upscaling/downscaling**
* **Git/GitHub**
    * **Git** is a free and open source distributed version-control system (VCS) for tracking changes in source code during software development (although it is not limited to source code/software development).
        * Git runs *locally*
        * [Official Website](https://git-scm.com/)
    * **GitHub** is a commercial service that provides a *remote* hub for git repositories to enable collaboration and dissemination of code
        * **Pull Request**
* **GRIB**
    * GRIB (**GRI**dded **B**inary) is a general purpose, bit-oriented data exchange format. It is an efficient vehicle for transmitting large volumes of gridded data to automated centers over high-speed telecommunication lines using modern protocols. By packing information into the GRIB code, messages (or records * the terms are synonymous in this context) can be made more compact than character oriented bulletins, which will produce faster computer-to-computer transmissions. GRIB can equally well serve as a data storage format, generating the same efficiencies relative to information storage and retrieval devices.
* **Grid**
    * **Resolution**
        * impacts for hydrologic and atmospheric models
* **HDF5**
    * The Hierarchical Data Format version 5 (HDF5), is an open source file format that supports large, complex, heterogeneous data. HDF5 uses a "file directory" like structure that allows you to organize data within the file in many different structured ways, as you might do with files on your computer. The HDF5 format also allows for embedding of metadata making it self-describing.
    * [Alt Overview](https://www.earthdatascience.org/courses/use-data-open-source-python/hierarchical-data-formats-hdf/) -- includes guide to working with HDF5 in Python
    * [HDF Group Website](https://www.hdfgroup.org/solutions/hdf5/)
    * [HDF5 Overview](https://www.neonscience.org/about-hdf5)
* **Initial conditions**
* **Interpolation**
    * **Bilinear vs. budget bilinear**
    * **Nearest-neighbor**
* **Land Surface Models (LSMs)**

    * LSMs simulate the exchange of water and energy fluxes <!--too vague-->
    * Land Surface Modeling - Science Background:

    > Global climate and the global carbon cycle are, in a direct sense, controlled by exchanges of water, carbon, and energy between the terrestrial biosphere and atmosphere. Thus models of these processes are essential for the purpose of developing predictive capability for the Earth's climate on all time scales, including seasonal climate prediction as well as natural climate fluctuations and human-induced climate change on decadal time scales.
    >
    > Early efforts to introduce parameterizations of land processes into global climate models were driven by the understanding that development of the atmospheric planetary boundary layer (including clouds and precipitation) is strongly affected by redistribution of incoming radiative energy at the land surface into sensible and latent heat fluxes. Application of increasingly sophisticated land-surface models has shown that water-carbon-energy exchanges are tightly coupled, that large-scale interannual variations in climate produce substantial variations in atmosphere-biosphere carbon exchanges and that changes in atmospheric concentrations of CO<sub>2</sub> can influence vegetation physiology directly. Most current land-surface models are can be associated with three broad types with respect to representation of vegetation as described by Foley (1995a):
    >
    > * soil-vegetation-atmosphere transfer schemes (SVATS)
    > * potential vegetation models (PVMs)
    > * terrestrial biogeochemistry models (TBMs).
    >
    > Development of more realistic treatment of biospheric processes in land-surface schemes for GCMs has resulted in explicit representation of biospheric carbon exchanges thereby interweaving the carbon cycle with energy and water cycles.
    >
    > The first generation of soil-vegetation-atmosphere transfer schemes (SVATS) evolved from simple bucket schemes focusing on soil water availability (Manabe, 1969), through the schemes of Deardorff (1978), to the biosphere-atmosphere transfer scheme (BATS) of Dickinson et al. (1986) and the Simple Biosphere (SiB) of Sellers et al. (1986). The latter was the first land-surface scheme that explicitly modeled plant physiology in a GCM (General Circulation Model or Global Climate Model). Aside from differences in which processes were included in these schemes, some schemes focused on more complex methods for the spatial treatment of vegetation cover itself including the mosaic-of-tiles type (Avissar and Pielke, 1989; Koster and Suarez, 1992). For most SVATS, land cover is fixed, with seasonally-varying prescriptions of parameters such as reflectance, leaf area index or rooting depth. Some SVATS incorporate satellite data to characterize more realistically the seasonal dynamics in vegetation function (e.g., Sellers et al., 1994) and several simulate ecological processes such as primary productivity and plant respiration (Sellers et al., 1992; Bonan, 1994).
    >
    > Biogeography, or potential vegetation, models (PVMs) comprise a suite of schemes that focus on modeling distributions of vegetation as a function of climate (e.g., Holdridge, 1947; Prentice, 1990) without influences of anthropogenic or natural disturbance. Several include more sophisticated approaches to account for competition and varying combinations of plant functional types, as well as physiological and ecological constraints on vegetation distributions (e.g. Woodward, 1987; Prentice et al., 1992). Although not merged directly with GCMs, these models have been used to simulate vegetation distributions for the present climate as well as for paleo- and future climates (Prentice, 1990; Prentice and Fung, 1990; Foley, 1995b).
    >
    > Finally, terrestrial biogeochemistry models (TBMs), developed from scaling up local ecological models, are process-based models that simulate dynamics of energy, water, and carbon and nitrogen exchange among biospheric pools and the atmosphere. They typically model primary productivity/photosynthesis as a function of prescribed vegetation, soil, and climate parameters, characterize composition and turnover times of vegetation and soil components, and rely on simple parameterizations of the surface energy and water balance (e.g., Potter et al., 1993; Parton et al., 1993; Schimel et al., 1990). Because these models either rely on, or predict, static land-cover distributions, they are not applicable to transient climate change experiments.
    >
    > A few of the 40 existing, free-standing, global TBMs incorporate vegetation dynamics (J. Foley, I. Woodward, A. Friend, C. Prentice). These models are designed to represent more realistically the dynamic exchanges of water, energy and carbon between the land surface and the atmosphere including seasonal-to-interannual as well as decadal-to-centennial interactions. For example, modeled processes that simulate vegetation responses on seasonal time scales (e.g., phenology of leaf area index and photosynthesis) control the cumulative response of vegetation to a multi-year drought as well as large-scale changes in vegetation distributions in response to transient climate change. Currently, dynamic vegetation models (DVMs) include components of all the schemes summarized above although their strengths lie primarily in the representation of physiological and biological processes while radiative and soil components remain quite simple.
    >
    > Several model intercomparisons have focused on evaluating SVATS and terrestrial biosphere models. The Project for Intercomparison of Land-surface Parameterization Schemes was initiated to evaluate an array of land-surface schemes existing in GCMs (Pitman et al., 1993; Henderson-Sellers et al., 1993; Shao and Henderson-Sellers, 1996). About 30 terrestrial biosphere models, including DVMs, were assessed in two workshops run by the Potsdam Institute for Climate Impact Research (PIK) (Lurin, 1994). PILPS concentrated on assessing the adequacy of modeled energy and water balances at the land surface, while the PIK meetings concentrated on integrative processes like net primary productivity and biomass accumulation.
    >
    > One feature absent from most land surface schemes in GCMs is anthropogenic alterations to land cover. This influence is included, albeit in a simple way, in several schemes that rely on prescribed land-cover distributions as well as those that rely on remote sensing to describe the land surface.
    >
    > The strong coupling of exchanges of water, energy and carbon between the land surface and the atmosphere, along with development of models with varying representations of physical, physiological, and biogeochemical processes, has led to similar efforts among GCM groups to merge strengths of the more sophisticated SVATS (physical and some physiological processes) with those of the dynamic vegetation models (physiological and biological processes). The workshop at GISS was designed to bring together researchers working on various components of land-surface schemes, including snow, to discuss status and improvements of land-surface representations in GCMs.

    * Source: <https://www.giss.nasa.gov/meetings/landsurface1998/section3.html>

    * [LDAS](https://ldas.gsfc.nasa.gov/) Motivations:
        * The land-surface component of the water cycle affects atmospheric and climatic processes while also controlling the distribution of freshwater, which is vital to life on land.
        * Proper characterization of spatial and temporal variations in water and energy states (e.g., soil moisture and temperature) and fluxes (e.g., evaporation and runoff) and is critical to many scientific and practical applications, including weather prediction, agricultural forecasting, drought and flood risk assessments, and improving understanding of land-atmosphere interactions and climate change impacts.
        * Numerous ground and space based observations are relevant to the water and energy cycles, but often they include errors or spatial and temporal gaps, or they may not contain exactly the information we need (for example, snow cover observations do not tell us how much water the snow contains).
    * LSMs in LIS (and why):
        * CABLE Land Surface Model
        * Community Land Model (v2.0)
        * Catchment Land Surface Model
        * WRSI Model
        * HySSIB Land Surface Model
        * Mosaic Land Surface Model
        * Noah Land Surface Model
        * VIC Land Surface Model (v4.1.1 and v 4.1.2.l)

        *See the [LIS Reference Manual](https://lis.gsfc.nasa.gov/documentation/lis) for more information about included LSMs.*
* **MPI/layout**
* **Model physics**
* **netCDF**
    * NetCDF (Network Common Data Form) is a set of software libraries and machine-independent data formats that support the creation, access, and sharing of array-oriented scientific data. It is also a community standard for sharing scientific data.
    * Official website: <https://www.unidata.ucar.edu/software/netcdf/>
* **NU-WRF**
    * The **N**ASA-**U**nified **W**eather **R**esearch and **F**orecasting ([NU-WRF](https://nuwrf.gsfc.nasa.gov/)) model is an observation-driven regional earth system modeling and assimilation system at satellite-resolvable scale. NU-WRF is one of three major earth system modeling systems funded by NASA’s Modeling Analysis and Prediction (MAP) program.

        NU-WRF is designed to study following areas:

        * Impacts of land-surface initialization and hydrological data assimilation on mesoscale weather and regional climate.
        * Feedbacks and coupling between the land surface and planetary boundary layer.
        * High-impact phenomena, such as hurricane, squall line, blizzard, and drought/flood, dust storms, wildfire, heavy pollution events.
        Aerosol-cloud-precipitation interactions.
        * Mesoscale processes controlling the variability of aerosols and trace gases.
        * Effects of aerosols and trace gases on regional climate and air quality.
        * Improve representation of regional water cycle through assimilating precipitation-affected microwave radiance from satellites.
        * Semi-operational high-resolution weather forecasting to support NASA’s field campaigns.
        * Downscaling NASA’s global modeling and reanalysis for regional climate.
        * Supporting current and future satellite missions via satellite simulator.
        * Improving CO2 flux and transport process representation via high-resolution simulation of the surface state and weather.
* **Off-line**
* **Open loop**
* **OPT/UE**
    * <https://esto.nasa.gov/news/news_LIS_8_2012.html>
* **Parameters**
    * (what it means and list of requirements)
* **Perturbations**
    * **State perturbation**
    * **Forcing perturbation**
    * **Observation perturbation**
    * **Perturbation algorithm**
    * **Perturbation frequency**

* **Private modules**
    >The Linux modules package is used to manage users' environment variables to allow users to easily access different versions of commonly used software.
    >
    >Multiple versions of compilers from different vendors and other support applications are available for users on the Discover cluster. These applications are loaded into your environment through the use of modules.
    * More information: <https://www.nccs.nasa.gov/nccs-users/instructional/using-discover/miscellaneous/using-modules>
    * *Module files to set up an environment for LIS work are available from the LIS team!*
* **Queue**
* **Restart**
* **Routing models**
* **Scaling (computational)**
* **Skill scores**
    * How they matter for model performance (e.g., KGE, NSE, Bias, Correlation, etc.)
* **Spin-up**
    * Definition: The time required for a given model to “forget” initial and boundary conditions and reach a state of statistical equilibrium.
    * >Spin up time is simply the time taken for the computer model to approach its own climatology after being started from other initial conditions. If a model were perfectly accurate and the initial conditions from which it is started were also perfect, then there would be no spin up time. However, in practice, computer models of the atmosphere and ocean are imperfect and will drift from a given initial state towards their own preferred state.
        * Additional answers: <https://www.researchgate.net/post/What_does_one_mean_by_Model_Spin_Up_Time>
* **States**
* **Tile**
* **Time-averaged vs. Instantaneous Output**
* **Water balance**
