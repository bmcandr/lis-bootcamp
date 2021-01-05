# Glossary

<!--Add new items in appropriate place to maintain alphabetic order!-->

The following terms and concepts will likely be encountered as you work with LIS. *This document is still in the information collection phase, edits for clarity and brevity to come...*

## Batch Script

>...a series of commands to be executed by the command-line interpreter, stored in a plain text file. A batch file may contain any command the interpreter accepts interactively and use constructs that enable conditional branching and looping within the batch file, such as IF, FOR, and GOTO labels. The term "batch" is from batch processing, meaning "non-interactive execution", though a batch file may not process a batch of multiple data.

&mdash; [*Source*](https://en.wikipedia.org/wiki/Batch_file)

## Bias

>We consider the term **bias** to broadly include any type of error that is systematic rather than random. In statistics, bias is a property of an estimator which, on average, under- or overestimates some quantity. For example, a model which is consistently cold at some location is biased. The bias may be spatially variable, seasonal, diurnal, or even situation-dependent. If we allow some ﬂexibility with respect to the notion of a model (or an observing system) as an estimator, and with the operative deﬁnition of averaging, then any component of error that is systematic in some well-deﬁned sense can be considered a bias. This is consistent with the usage of human forecasters, who describe, for example, the tendency of a particular model to generate excessive surface lows in certain recurring situations as a bias.

&mdash; Dee, D. P. (2005). Bias and data assimilation. Quarterly Journal of the Royal Meteorological Society: A journal of the atmospheric sciences, applied meteorology and physical oceanography, 131(613), 3323-3343. <https://doi.org/10.1256/qj.05.137>

### Bias Correction

>Climate models exhibit systematic errors (biases) in their output. These errors can be due, among others, to:
>
>* Limited spatial resolution (horizontal and vertical)
>* Simplified physics and thermodynamic processes
>* Numerical schemes
>* Incomplete knowledge of climate system processes
>
>Such errors can and generally should be corrected for, before using climate model data in impact studies. The main assumptions of bias correction methods are:
>
>* Quality of the observations database limits the quality of the correction.
>* It is assumed that the bias behaviour of the model does not change with time.
>* Limitation: Temporal errors of major circulation systems can not be corrected.

&mdash; [*Source*](https://climate4impact.eu/impactportal/documentation/guidanceandusecases.jsp?q=bias_correction)

#### **Bias correction approaches and algorithms**

#### *Bias-blind*

>Data assimilation systems that are designed to correct random, zero-mean errors in a model-generated background estimate based on unbiased observations might be called **bias-blind**. Routine monitoring of observed-minus-background residuals (also knownas innovations, background residuals, or background departures) in bias-blind systems invariably shows evidence of biases in either the model, the observations, or both.

&mdash; *see Dee, 2005 (above)*

* Standard normal deviate-based scaling
* CDF matching

#### Bias-aware

>Similarly, the presence of persistent or repetitive patterns in the analysis increments produced during the assimilation indicates that there are systematic discrepancies between model and observations, and possibly among different components of the observing systemas well. To effectively remove those discrepancies during the data assimilation process requires **bias-aware** assimilation methods, which incorporate speciﬁc assumptions about the source and nature of (some of) the biases in the system, and are speciﬁcally designed to estimate and correct those biases.

&mdash; *see Dee, 2005 (above)*

More information:

&mdash; Kumar, S. V., Reichle, R. H., Harrison, K. W., Peters‐Lidard, C. D., Yatheendradas, S., and Santanello, J. A. ( 2012), A comparison of methods for a priori bias correction in soil moisture data assimilation, Water Resour. Res., 48, W03515, [doi:10.1029/2010WR010261](https://doi.org/10.1029/2010WR010261).

## Calibration and Validation

>**Calibration** the iterative process of comparing the model with real system, revising the model if necessary, comparing again, until a model is accepted (validated).
>
>**Validation** is a process of comparing the model and its behavior to the real system and its behavior.

&mdash; [*Source*](https://www.eg.bucknell.edu/~xmeng/Course/CS6337/Note/master/node70.html)

## Cold start

>A cold start usually occurs when a model is first initialized and needs to be spun up. For example, if a regional model is configured in a new domain, it would need to be started in this manner. A cold start could be from climatology (commonly used to initialize global models), an analysis of data (such as MODAS), a forecast from a different model (like using NOGAPS to initialize (COAMPSTM), or a combination of the above. The model is then run until a statistical equilibrium is achieved.

&mdash; [*Source*](https://www.oc.nps.edu/nom/modeling/initial.html)

## Coupled

>In Earth-system models, the term "coupling" is often used in two ways. The first is "offline" coupling, where output from one model is passed to another model for computation of some variable. The second is "online" coupling, where the feedbacks are allowed to pass between the two models. Online coupling is synonymous with "fully coupled".
>
>A good example is when wildfires are modeled with coupled meteorology-chemistry models. Historically, the aerosol emissions from the fire would be modeled by a separate chemistry model that only used the meteorology model as an input (e.g. offline coupling). However, now you can fully couple (e.g. online coupling) the chemistry with the weather such that the heat and aerosol released from the fire perturbs the meteorological/radiation calculations in the next iterated time step.

&mdash; [*Source*](https://earthscience.stackexchange.com/questions/4792/meaning-of-coupling-in-modelling)

* Overview: <https://www.star.nesdis.noaa.gov/ngmmf/documents/Coupled_Modeling_Tutorial.pdf>

## Data assimilation

>a mathematical discipline that seeks to optimally combine theory (usually in the form of a numerical model) with observations

&mdash; [*Source*](https://en.wikipedia.org/wiki/Data_assimilation)

<!-- TODO: Update above quote with info from <http://robinson.seas.harvard.edu/PAPERS/red_report_62.html>? Additional information (presentation): <https://earth.esa.int/documents/973910/1641786/AO1.pdf> -->

### Algorithms

* Matlab video series: [Understanding Kalman Filters](https://www.youtube.com/watch?v=mwn8xhgNpFY)
* **Ensemble Kalman Filter (EnKF)**
    >The ensemble Kalman flter (EnKF) is an approximate fltering method introduced in the geophysics literature by [Evensen (1994)](https://doi.org/10.1029/94JC00572). In contrast to the standard Kalman flter ([Kalman 1960](https://www.cs.unc.edu/~welch/kalman/media/pdf/Kalman1960.pdf)), which works with the entire distribution of the state explicitly, the EnKF stores, propagates, and updates an ensemble of vectors that approximates the state distribution. This ensemble representation is a form of dimension reduction, in that only a small ensemble is propagated instead of the joint distribution including the full covariance matrix. When new observations become available, the ensemble is updated by a linear “shift” based on the assumption of a linear Gaussian state-space model. Hence, additional approximations are introduced when non-Gaussianity or nonlinearity is involved. However, the EnKF has been highly successful in many extremely high-dimensional, nonlinear, and non-Gaussian data-assimilation applications. It is an embodiment of the principle that an approximate solution to the right problem is worth more than a precise solution to the wrong problem (Tukey 1962). For many realistic, highly complex systems, the EnKF is essentially the only way to do (approximate) inference, while alternative exact inference techniques can only be applied to highly simplifed versions of the problem.

    &mdash; [Source](https://www.math.umd.edu/~slud/RITF17/enkf-tutorial.pdf)

* **Extended Kalman Filter (EKF))**
    >In estimation theory, the extended Kalman filter (EKF) is the nonlinear version of the Kalman filter which linearizes about an estimate of the current mean and covariance.

    &mdash; [*Source*](https://en.wikipedia.org/wiki/Extended_Kalman_filter)

* **EnKS (Ensemble Kalman Smoother)**
    >A Kalman smoother is a direct generalization of the Kalman filter which incorporates observations both before and after the analysis time.

    &mdash; [*Source*](https://ams.confex.com/ams/pdfpapers/28864.pdf)

* **DI (Direct insertion)**

    >Direct Insertion consists of replacing the forecast values by the observed ones, at all data points.

    &mdash; [*Source*](http://robinson.seas.harvard.edu/PAPERS/red_report_62.html)

The above algorithms are discussed in the context of LIS in [Kumar et al. 2008](https://pubag.nal.usda.gov/download/23495/PDF).

* **Analysis increments**

    >Reanalysis data are an important source of information for hydrometeorology applications, which use data assimilation to combine an imperfect atmospheric model with uncertain observations. However, uncertainty estimates are not normally provided with reanalyses. The model “first guess” (6-h forecast) is sometimes saved along with reanalysis estimates, which allows the calculation of the **analysis increment (AI)**, *defined as the analysis minus the model first guess.* Analysis increment statistics could provide a quantitative index for comparing models in regions with sufficient observations.

    &mdash; [*Source*](https://journals.ametsoc.org/jhm/article/9/6/1535/5877/Comparing-Reanalyses-Using-Analysis-Increment)

* **Innovations**

    >The difference between the forecast and the observations...(as it provides new information to the data assimilation process).

    &mdash; [*Source*](https://en.wikipedia.org/wiki/Data_assimilation)

* **Measurement model**

    >Some metrologists and philosophers of science argue that virtually all scientific measurement involves inference: we infer measurement outcomes from instrument indications, with the help of a measurement model that is very often informed by theory (see Mari 2005; Boumans 2006; Tal 2012). *A **measurement model** is a conceptualization of i) the physical interactions that take place during a measuring process—both desired interactions and interfering ones—as well as ii) how the results of those interactions relate to values of the parameter(s) that we seek to measure.* Such a model guides the inference from the rain gauge’s indication to the final estimate of rainfall depth and from the thermometer and barometer indications to the final estimate of relative humidity. In some cases, the inference from instrument indication(s) to measurement outcome is rather trivial—we might have good reason to take a particular thermometer’s indication at face value, for example—but in many other cases it is more complex, involving calculations informed by theory.

    &mdash; [*Source*](https://journals.ametsoc.org/bams/article/97/9/1565/69542)

* **Forward model**

    Prediction of system state based on given initial conditions and a model

    &mdash; Adapted from: <https://www.quora.com/What-is-the-difference-between-forward-and-inversion-modeling-in-geophysics>

    Additional information: <https://open.oregonstate.education/climatechange/chapter/models/>

* **Measurement error standard deviation**

    <!-- TODO -->

## Data compression
<!-- TODO -->
<!-- * (this has been a recent learning experience in the Air Force project) -->
<!-- * Reducing file sizes? -->

## Debugging

>The process of finding and resolving defects or problems within a computer program that prevent correct operation of computer software or a system.

&mdash; [*Source*](https://en.wikipedia.org/wiki/Debugging)

There are a variety of tools used in debugging, including:

* [TotalView](https://totalview.io/products/totalview) - debugging software that provides the specialized tools you need to quickly debug, analyze, and scale high-performance computing (HPC) applications. This includes highly dynamic, parallel, and multicore applications that run on diverse hardware — from desktops to supercomputers.
* [valgrind](https://valgrind.org/) - an instrumentation framework for building dynamic analysis tools. There are Valgrind tools that can automatically detect many memory management and threading bugs, and profile your programs in detail.
* "Old school" print statements - embed print statements in your code to output variables and status messages to the console

## Ensembles
<!-- TODO -->
* **Downscale**

* **Upscale**

* **Number of ensembles**

* **Ensemble mean**

* **Ensemble spread**

## ESMF

>The **E**arth **S**ystem **M**odeling **F**ramework (ESMF) is high-performance, flexible software infrastructure for building and coupling weather, climate, and related Earth science applications. The ESMF defines an architecture for composing complex, coupled modeling systems and includes data structures and utilities for developing individual models.
>
>The basic idea behind ESMF is that complicated applications should be broken up into coherent pieces, or components, with standard calling interfaces. In ESMF, a component may be a physical domain, or a function such as a coupler or I/O system. ESMF also includes toolkits for building components and applications, such as regridding software, calendar management, logging and error handling, and parallel communications.

&mdash; [*Source*](https://www.earthsystemcog.org/projects/esmf/)

## Fluxes

Inflows and outflows (e.g., the exchange of water between land and atmosphere).

## Forcings

* What it means and list of requirements
* From LIS 7.0 User Guide:
    >The boundary conditions describing the (upper) atmospheric fluxes are known as "forcings". LIS makes use of model derived data as well as satellite and ground-based observational data as forcings. The land surface models are typically run using model derived data. The observational data are used to overwrite the model derived data, whenever they are available.
* **Upscaling/downscaling**

    <!-- TODO -->

## Git and GitHub

[**Git**](https://git-scm.com/) (or `git`) is a free and open source distributed version-control system (VCS) for tracking changes in a project. Git is used primarily through the command line, but visual `git` programs and extensions for text editors (e.g., Atom, VS Code) enable the use of `git` through a GUI.

**GitHub** is a commercial cloud-hosting service for `git` repositories that enables collaboration. To learn more about using GitHub, see the [official documentation](https://docs.github.com/en/free-pro-team@latest/github).

* **Pull Requests**

    >Pull Requests let you tell others about changes you've pushed to a branch in a repository on GitHub. Once a pull request is opened, you can discuss and review the potential changes with collaborators and add follow-up commits before your changes are merged into the base branch.
    &mdash; [*Source*](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests)

## GRIB (**GRI**dded **B**inary)

>... a general purpose, bit-oriented data exchange format. It is an efficient vehicle for transmitting large volumes of gridded data to automated centers over high-speed telecommunication lines using modern protocols. By packing information into the GRIB code, messages (or records * the terms are synonymous in this context) can be made more compact than character oriented bulletins, which will produce faster computer-to-computer transmissions. GRIB can equally well serve as a data storage format, generating the same efficiencies relative to information storage and retrieval devices.

&mdash; [*Source*](https://climatedataguide.ucar.edu/climate-data-tools-and-analysis/grib)

## Grid

### Resolution

<!-- TODO: impacts for hydrologic and atmospheric models -->

## HDF5

>The Hierarchical Data Format version 5 (HDF5), is an open source file format that supports large, complex, heterogeneous data. HDF5 uses a "file directory" like structure that allows you to organize data within the file in many different structured ways, as you might do with files on your computer. The HDF5 format also allows for embedding of metadata making it self-describing.

&mdash; [*Source*](https://www.neonscience.org/resources/learning-hub/tutorials/about-hdf5#:~:text=The%20Hierarchical%20Data%20Format%20version,with%20files%20on%20your%20computer.)

* [HDF5 Overview](https://www.neonscience.org/about-hdf5)
* [HDF Group Website](https://www.hdfgroup.org/solutions/hdf5/)
* [Alternate Overview](https://www.earthdatascience.org/courses/use-data-open-source-python/hierarchical-data-formats-hdf/) &mdash; includes guide to working with HDF5 in Python

## Initial conditions
<!-- TODO -->
* <https://www.merriam-webster.com/dictionary/initial%20condition>
* <https://www.public.asu.edu/~kirkwood/sysdyn/SDIntro/ch-9.pdf>

## Interpolation
<!-- TODO -->
* **Bilinear vs. budget bilinear**
* **Nearest-neighbor**

## Land Surface Models (LSMs)

### Science Background

>Global climate and the global carbon cycle are, in a direct sense, controlled by exchanges of water, carbon, and energy between the terrestrial biosphere and atmosphere. Thus models of these processes are essential for the purpose of developing predictive capability for the Earth's climate on all time scales, including seasonal climate prediction as well as natural climate fluctuations and human-induced climate change on decadal time scales.
>
>Early efforts to introduce parameterizations of land processes into global climate models were driven by the understanding that development of the atmospheric planetary boundary layer (including clouds and precipitation) is strongly affected by redistribution of incoming radiative energy at the land surface into sensible and latent heat fluxes. Application of increasingly sophisticated land-surface models has shown that water-carbon-energy exchanges are tightly coupled, that large-scale interannual variations in climate produce substantial variations in atmosphere-biosphere carbon exchanges and that changes in atmospheric concentrations of CO<sub>2</sub> can influence vegetation physiology directly. Most current land-surface models are can be associated with three broad types with respect to representation of vegetation as described by Foley (1995a):
>
>* soil-vegetation-atmosphere transfer schemes (SVATS)
>* potential vegetation models (PVMs)
>* terrestrial biogeochemistry models (TBMs).
>
>Development of more realistic treatment of biospheric processes in land-surface schemes for GCMs has resulted in explicit representation of biospheric carbon exchanges thereby interweaving the carbon cycle with energy and water cycles.
>
>The first generation of soil-vegetation-atmosphere transfer schemes (SVATS) evolved from simple bucket schemes focusing on soil water availability (Manabe, 1969), through the schemes of Deardorff (1978), to the biosphere-atmosphere transfer scheme (BATS) of Dickinson et al. (1986) and the Simple Biosphere (SiB) of Sellers et al. (1986). The latter was the first land-surface scheme that explicitly modeled plant physiology in a GCM (General Circulation Model or Global Climate Model). Aside from differences in which processes were included in these schemes, some schemes focused on more complex methods for the spatial treatment of vegetation cover itself including the mosaic-of-tiles type (Avissar and Pielke, 1989; Koster and Suarez, 1992). For most SVATS, land cover is fixed, with seasonally-varying prescriptions of parameters such as reflectance, leaf area index or rooting depth. Some SVATS incorporate satellite data to characterize more realistically the seasonal dynamics in vegetation function (e.g., Sellers et al., 1994) and several simulate ecological processes such as primary productivity and plant respiration (Sellers et al., 1992; Bonan, 1994).
>
>Biogeography, or potential vegetation, models (PVMs) comprise a suite of schemes that focus on modeling distributions of vegetation as a function of climate (e.g., Holdridge, 1947; Prentice, 1990) without influences of anthropogenic or natural disturbance. Several include more sophisticated approaches to account for competition and varying combinations of plant functional types, as well as physiological and ecological constraints on vegetation distributions (e.g. Woodward, 1987; Prentice et al., 1992). Although not merged directly with GCMs, these models have been used to simulate vegetation distributions for the present climate as well as for paleo- and future climates (Prentice, 1990; Prentice and Fung, 1990; Foley, 1995b).
>
>Finally, terrestrial biogeochemistry models (TBMs), developed from scaling up local ecological models, are process-based models that simulate dynamics of energy, water, and carbon and nitrogen exchange among biospheric pools and the atmosphere. They typically model primary productivity/photosynthesis as a function of prescribed vegetation, soil, and climate parameters, characterize composition and turnover times of vegetation and soil components, and rely on simple parameterizations of the surface energy and water balance (e.g., Potter et al., 1993; Parton et al., 1993; Schimel et al., 1990). Because these models either rely on, or predict, static land-cover distributions, they are not applicable to transient climate change experiments.
>
>A few of the 40 existing, free-standing, global TBMs incorporate vegetation dynamics (J. Foley, I. Woodward, A. Friend, C. Prentice). These models are designed to represent more realistically the dynamic exchanges of water, energy and carbon between the land surface and the atmosphere including seasonal-to-interannual as well as decadal-to-centennial interactions. For example, modeled processes that simulate vegetation responses on seasonal time scales (e.g., phenology of leaf area index and photosynthesis) control the cumulative response of vegetation to a multi-year drought as well as large-scale changes in vegetation distributions in response to transient climate change. Currently, dynamic vegetation models (DVMs) include components of all the schemes summarized above although their strengths lie primarily in the representation of physiological and biological processes while radiative and soil components remain quite simple.
>
>Several model intercomparisons have focused on evaluating SVATS and terrestrial biosphere models. The Project for Intercomparison of Land-surface Parameterization Schemes was initiated to evaluate an array of land-surface schemes existing in GCMs (Pitman et al., 1993; Henderson-Sellers et al., 1993; Shao and Henderson-Sellers, 1996). About 30 terrestrial biosphere models, including DVMs, were assessed in two workshops run by the Potsdam Institute for Climate Impact Research (PIK) (Lurin, 1994). PILPS concentrated on assessing the adequacy of modeled energy and water balances at the land surface, while the PIK meetings concentrated on integrative processes like net primary productivity and biomass accumulation.
>
>One feature absent from most land surface schemes in GCMs is anthropogenic alterations to land cover. This influence is included, albeit in a simple way, in several schemes that rely on prescribed land-cover distributions as well as those that rely on remote sensing to describe the land surface.
>
>The strong coupling of exchanges of water, energy and carbon between the land surface and the atmosphere, along with development of models with varying representations of physical, physiological, and biogeochemical processes, has led to similar efforts among GCM groups to merge strengths of the more sophisticated SVATS (physical and some physiological processes) with those of the dynamic vegetation models (physiological and biological processes). The workshop at GISS was designed to bring together researchers working on various components of land-surface schemes, including snow, to discuss status and improvements of land-surface representations in GCMs.

&mdash; [*Source*](https://www.giss.nasa.gov/meetings/landsurface1998/section3.html)

### Motivations for modeling the land surface (and the LDAS project in particular):

>* The land-surface component of the water cycle affects atmospheric and climatic processes while also controlling the distribution of freshwater, which is vital to life on land.
>* Proper characterization of spatial and temporal variations in water and energy states (e.g., soil moisture and temperature) and fluxes (e.g., evaporation and runoff) and is critical to many scientific and practical applications, including weather prediction, agricultural forecasting, drought and flood risk assessments, and improving understanding of land-atmosphere interactions and climate change impacts.
>* Numerous ground and space based observations are relevant to the water and energy cycles, but often they include errors or spatial and temporal gaps, or they may not contain exactly the information we need (for example, snow cover observations do not tell us how much water the snow contains).

&mdash; [*Source*](https://ldas.gsfc.nasa.gov/ldas/land-data-assimilation-system)

* LSMs in LIS (and why): <!--TODO: update list-->
    * CABLE Land Surface Model
    * Community Land Model (v2.0)
    * Catchment Land Surface Model
    * WRSI Model
    * HySSIB Land Surface Model
    * Mosaic Land Surface Model
    * Noah Land Surface Model
    * VIC Land Surface Model (v4.1.1 and v 4.1.2.l)

    *See the [LIS Users' Guide](https://github.com/NASA-LIS/LISF/tree/master/docs) for an up to date list of LSMs included in LIS.*

## MPI (Message Passing Interface)

>MPI is a communication protocol for programming parallel computers. Both point-to-point and collective communication are supported. MPI "is a message-passing application programmer interface, together with protocol and semantic specifications for how its features must behave in any implementation." MPI's goals are high performance, scalability, and portability. MPI remains the dominant model used in high-performance computing today.

&mdash; [*Source*](https://en.wikipedia.org/wiki/Message_Passing_Interface)

* [MPI Introduction and Tutorial](https://computing.llnl.gov/tutorials/mpi/)

## Model physics

## NetCDF

>NetCDF (Network Common Data Form) is a set of software libraries and machine-independent data formats that support the creation, access, and sharing of array-oriented scientific data. It is also a community standard for sharing scientific data.

&mdash; [*Source*](https://confluence.ecmwf.int/display/CKB/What+are+NetCDF+files+and+how+can+I+read+them#:~:text=NetCDF%20\(Network%20Common%20Data%20Form,store%20and%20distribute%20scientific%20data.)

Official website: <https://www.unidata.ucar.edu/software/netcdf/>

## NU-WRF

>The **N**ASA-**U**nified **W**eather **R**esearch and **F**orecasting ([NU-WRF](https://nuwrf.gsfc.nasa.gov/)) model is an observation-driven regional earth system modeling and assimilation system at satellite-resolvable scale. NU-WRF is one of three major earth system >modeling systems funded by NASA’s Modeling Analysis and Prediction (MAP) program.
>
>NU-WRF is designed to study following areas:
>
>* Impacts of land-surface initialization and hydrological data assimilation on mesoscale weather and regional climate.
>* Feedbacks and coupling between the land surface and planetary boundary layer.
>* High-impact phenomena, such as hurricane, squall line, blizzard, and drought/flood, dust storms, wildfire, heavy pollution events.
>* Aerosol-cloud-precipitation interactions.
>* Mesoscale processes controlling the variability of aerosols and trace gases.
>* Effects of aerosols and trace gases on regional climate and air quality.
>* Improve representation of regional water cycle through assimilating precipitation-affected microwave radiance from satellites.
>* Semi-operational high-resolution weather forecasting to support NASA’s field campaigns.
>* Downscaling NASA’s global modeling and reanalysis for regional climate.
>* Supporting current and future satellite missions via satellite simulator.
>* Improving CO2 flux and transport process representation via high-resolution simulation of the surface state and weather.

&mdash; [*Source*](https://map.nasa.gov/models/NU-WRF.php)

## Off-line

*See [Coupled](#Coupled) definition above.*

## Open Loop

Model runs without data assimilation.

## Optimization and Uncertainty Estimation (OPT/UE)
<!-- TODO -->
<https://esto.nasa.gov/news/news_LIS_8_2012.html>

## Parameters
<!-- TODO what it means and list of requirements -->

## Perturbations
<!-- TODO -->
* **State perturbation**
* **Forcing perturbation**
* **Observation perturbation**
* **Perturbation algorithm**
* **Perturbation frequency**

## Modules

>The Linux `modules` package is used to manage users' environment variables to allow users to easily access different versions of commonly used software.
>
>Multiple versions of compilers from different vendors and other support applications are available for users on the Discover cluster. These applications are loaded into your environment through the use of modules.

&mdash; [*Source*](https://www.nccs.nasa.gov/nccs-users/instructional/using-discover/miscellaneous/using-modules)

* *Module files for using LIS on NCCS' Discover HPC environment are available [here](https://github.com/NASA-LIS/LISF/tree/master/env/discover)*.

## Queues/Queuing

On shared computing platforms, such as *Discover*, compute resources are allocated to users via a resource management practice called queuing (or job scheduling). Users submit jobs to a queue where they wait for the required compute resources to become available. On *Discover*, the program `slurm` is used to manage job queues.

* [NASA NCCS's Slurm Usage Guidance](https://www.nccs.nasa.gov/nccs-users/instructional/using-slurm)
* [Slurm Official Website](https://slurm.schedmd.com/overview.html)

## Restart

>An initial run starts the model from an initial conditions dataset. As the model executes, history datasets, restart datasets and initial condition datasets are written periodically.
>...
>In addition to initial simulations, there are two types of continuation runs: **restart** and branch. **A restart run is an exact continuation of a previous simulation from its point of termination.**

&mdash; [*Source*](http://www.cgd.ucar.edu/cms/ccm3/ccm3lsm_doc/ccm3_doc/UG-18.html)

## Routing Models

>In hydrology, **routing** is a technique used to predict the changes in shape of a hydrograph as water moves through a river channel or a reservoir. In flood forecasting, hydrologists may want to know how a short burst of intense rain in an area upstream of a city will change as it reaches the city. Routing can be used to determine whether the pulse of rain reaches the city as a deluge or a trickle.
>
>Routing also can be used to predict the hydrograph shape (and thus lowland flooding potential) subsequent to multiple rainfall events in different sub-catchments of the watershed. Timing and duration of the rainfall events, as well as factors such as antecedent moisture conditions, overall watershed shape, along with subcatchment-area shapes, land slopes (topography/physiography), geology/hydrogeology (i.e. forests and aquifers can serve as giant sponges that absorb rainfall and slowly release it over subsequent weeks and months), and stream-reach lengths all play a role here. The result can be an additive effect (i.e. a large flood if each subcatchment's respective hydrograph peak arrives at the watershed mouth at the same point in time, thereby effectively causing a "stacking" of the hydrograph peaks), or a more distributed-in-time effect (i.e. a lengthy but relatively modest flood, effectively attenuated in time, as the individual subcatchment peaks arrive at the mouth of the main watershed channel in orderly succession).

Routing models currently supported by LIS:

* [HyMAP](https://doi.org/10.1175/JHM-D-12-021.1)
* HyMAP2
* NLDAS

## Scaling (computational)

<!-- TODO -->

## Skill scores
<!-- TODO -->
* How they matter for model performance (e.g., KGE, NSE, Bias, Correlation, etc.)

## Spin-up

The time required for a given model to “forget” initial and boundary conditions and reach a state of statistical equilibrium.

>Spin up time is simply the time taken for the computer model to approach its own climatology after being started from other initial conditions. If a model were perfectly accurate and the initial conditions from which it is started were also perfect, then there would be no spin up time. However, in practice, computer models of the atmosphere and ocean are imperfect and will drift from a given initial state towards their own preferred state.

&mdash; [*Source*](https://www.researchgate.net/post/What_does_one_mean_by_Model_Spin_Up_Time)

## States
<!-- TODO -->
## Tile
<!-- TODO -->

## Time-averaged vs. Instantaneous Output
<!-- TODO -->

## Water balance
<!-- TODO -->

* <https://www.ipcc.ch/site/assets/uploads/2018/03/ipcc_far_wg_II_chapter_04.pdf> <!--investigate further...-->
