---
source_id: RCSM6B
label: RCSM v6 B
label_extended: Regional Climate System Model, version 6B
parent:
  label: CNRM-RCSM6
  description:
  reference: Sevault (2024)
  reference-url: https://doi.org/10.5281/zenodo.11066601
reference: Sevault F. 2014 and Darmaraki et al. 2019b
reference-url: still the CNRM-RCSM6 web page

atmos:
  label: CNRM-ALADIN65
  description:
  reference: Nabat et al., 2020
  reference-url: https://doi.org/10.5194/acp-20-8315-2020
  projection: lambert_conformal_conic
  resolution: 12.5 km
  computational-grid-size: 480x480
  computational-grid-buffer: 8
  levels: 91
  TOA: 0.01 hPa
  time-step: 450 s 
  LBC-update-frequency: 6 h
  nudging: LBC # ( LBC | spectral | grid )
  hydrostatic: yes
  physics:
    radiation-shortwave:
      label: FMR
      description: FMR 6 bands
      reference: 
      reference-url: 
    radiation-longwave:
      label: RRTM
      description: Rapid radiative transfer model
      reference: Mlawer et al. (1997)
      reference-url: https://doi.org/10.1029/97JD00237
    convection:
      label: PCMT
      description: Prognostic Condensates Microphysics and Transport, a mass-flux scheme ...
      reference: Guérémy (2011)
      reference-url: 
    shallow-convection:
      label: PCMT
      description: Prognostic Condensates Microphysics and Transport, a mass-flux scheme ...
      reference: Piriou et al. (2007)
      reference-url:
    planetary-boundary-layer:
      label: Cuxart
      description: 
      reference: Cuxart et al. (2000)
      reference-url:
  grid: &atmos-grid
    horizontal-resolution: 12.5 km
    grid-type: lambert conformal
    number-of-grid-cells: 613x405
    full-domain: 640x432
    buffer-number-of-grid-cells: 8
    vertical-levels: 91
    model-top-height: 75 km, 0.01 hPa
  lateral-boundary-conditions:
    update-frequency: 6h
    nudging: LBCs with spectral nudging
    nesting: direct into GCM
    relaxation-scheme: according to Davies 1979

land:
  label: SURFEX v8.0
  family: SURFEX
  reference: Decharme et al., 2019
  reference-url: http://dx.doi.org/10.1029/2018MS001545
  levels: 30
  projection: lambert_conformal_conic
  resolution: 25 km
  physics:
    soil:
      label: ISBA
      land-use-dataset:
      land-use-change: no
    urban:
      label: none
      comment: Cities are represented as rock surfaces
    lake:
      label: FLake vX.X
      description: FLAKE single column model
      reference: Mironov (2008)
      reference-url:
    glacier:
      label: none
      comment: Simple parameterization to avoid snow accumulation
    river-routing:
      label: ISBA-CTRIP vx.x
      family: TRIP
      description: CTRIP is a river routing model used to convert the daily runoff simulated by ISBA into river discharge on the global river channel network at 1/12-degree resolution.
      reference: Munier and Decharme, 2022
      reference-url: https://doi.org/10.5194/essd-14-2239-2022
      reference: Decharme et al. (2019) 
      reference-url:  
      grid:
        resolution: 1/12°
        grid-type: regular
        number-of-grid-cells: 1116x648
      time-step: 1800 s
      floodplains-parameterization: true
      groundwater-scheme: true
      carbon-to-ocean: false
      spin-up: true

aerosol:
  label: TACTIC
  description:
  reference: Nabat et al, 2020
  reference-url: 
  family: AER
  grid: *atmos-grid
  transport: specific transport scheme (semi-lagrangian)
  concentrations:
  optical-radiative-properties:
  dataset: None

ocean:
  label: NEMOMED12 v3.6
  family: NEMO
  description: Regional version on the Mediterranean Sea of NEMOv3.6
  reference: Beuvier et al., 2012, Hamon et al., 2016, Waldman et al., 2018, Sevault 2024
  reference-url:
  resolution: 1/12 degrees
  levels: 75
  grid:
    resolution: 1/12 degree
    horizontal-grid-type: ORCA tripolar grid
    number-of-grid-cells: 567x264
    vertical-levels: 75 vertical levels using a vertical z-coordinate with partial step bathymetry formulation (Barnier et al., 2006), from 1m to 135m
  time-step: 720 s
  Red-Sea-representation: false
  Black-Sea-representation:
    model-domain: false
    Black-Sea-input: E-P-R budget
  Lateral-boundary-condition:
    buffer-zone: false
    open-boundary-condition: true
    dataset: forcing GCM
    unbiased-dataset: "True, with ORAS5"
  physics:
    equation-of-seawater: TEOS10 (IOC et al., 2010)
    advection: TVD for tracers, EEN for momentum
    lateral-physics:
    vertical-physics:
  river-input:
    coupled: "True, except for the Nile"
    dataset: "True, for the Nile only"
    Nile-representation: "a climatological seasonal cycle is applied, with a yearly average of 444 m3 .s−1 (Erika Coppola, Med-CORDEX community, https://www.medcordex.eu/Med-CORDEX-2 baseline-runs protocol.pdf)"
  tidal-forcing: "False, enhancement of the bottom friction (Beuvier et al., 2012)"
  chlorophyll-concentration: "3D climatology (Zhang et al. 2024, accepted)"
  wave-representation: false
  tide-representation: false
  tuning: false
  spin-up: true

sea-ice:
  label: GELATO 1D
  family: GELATO
  description:
  reference:
  reference-url:
  grid: 1D
  time-step: *atmos-time-step
  dynamics:
  thermodynamics:
  dataset: None
  spin-up: true

coupler:
  label: OASIS3-MCT
  reference: Craig et al. (2017)
  reference-url: http://doi.org/10.5194/gmd-10-3297-2017
  coupling-frequency: 1hr
  flux-correction: no

---

Regional Climate System models allow to reproduce medium scale atmospheric and oceanic phenomena on periods going from present time hindcast to scenarios for the 21st century.
Atmosphere, aerosol, land surface and sea-ice are grouped in one executable (ALADIN), river routing (CTRIP), ocean (NEMOMED12); the models are coupled with OASIS3-MCT, the outputs are managed by XIOS.
Each version of CNRM-RCSM6 uses the last versions of ALADIN, CTRIP and NEMOMED12 available at a given time.

An example how to add an image. Upload to the images/ folder and voilà!
![image tooltip here]({{ site.baseurl }}/images/model1-example-image.png)
or even simpler if the image is already available online (no need to upload anything):
![CNRM logo](https://www.umr-cnrm.fr/squelettes/img/cnrm_beau25.png)

Further information:

 * [CNRM-RCSM at umr-cnrm.fr](https://www.umr-cnrm.fr/spip.php?article1098)
 * [List of Med-CORDEX simulations with {{ page.source_id }}](https://wcrp-cordex.github.io/simulation-status/CMIP6_downscaling_plans.html?search.search=med-12%20{{ page.source_id }})

{% include toc %}

## Atmosphere
{% include resolution-summary.html source-id=page.source_id component="atmos" %}

Describe atmos component. 
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Physics

{% include physics.html source-id=page.source_id component="atmos" %}

#### Radiation

The radiative forcings include non-interactive greenhouse gases, tropospheric and stratospheric aerosols, ozone and solar. Greenhouse gases are CO2, N2O, CH4, CFC12 and a CFC11-equivalent species that includes the effects of all the other GHGs of the original data set (39 species). The solar constant uses the CMIP6 advised value (Matthes et al., 2017) including a 11-year cycle.

## Land
{% include resolution-summary.html source-id=page.source_id component="land" %}

The soil hydrology uses the mixed form of the Richards equation to describe the water‐mass transfer within the soil via Darcy's law.
The one‐dimensional Fourier law is used to simulate temperature diffusion.
ISBA is sub-divided in 12 patches in order to account for the variety of soil and vegetation behaviors within a grid point.
Energy and water balances are calculated separately on each patch."

SURFEX v8.0 encompasses several submodules for modeling the interactions between the atmosphere, the ocean, the lakes and the land surface.
The FLAKE model simulate surface fluxes over lakes, including both the Caspian and the Aral seas, and computes the temporal evolution of the vertical lake temperature profile from the surface mixing layer to the bottom (Le Moigne et al. 2016).
The land surface is represented using the new ISBA-CTRIP coupled system (Decharme et al., 2019; Delire et al. 2019).
ISBA calculates the time evolution of the energy, carbon and water budgets at the land surface while CTRIP simulates river discharges (including carbon transport) up to the ocean from the total runoff (and total soil carbon leaching) computed by ISBA."

{% include physics.html source-id=page.source_id component="land" %}

### References

 * Daniel M., Lemonsu A., Déqué M., Somot S., Alias A., Masson V. (2019) [Benefits of explicit urban parameterization in regional climate modeling to study climate and city interactions](doi:10.1007/s00382-018-4289-x). Climate Dynamics, 52(5-6), 2745-2764, doi:10.1007/s00382-018-4289-x

## Aerosol
{% include resolution-summary.html source-id=page.source_id component="aerosol" %}

Fully interactive: Sea salt, Desert Dust; Interactive + CMIP6 emission: Black Carbon, Organic Matter, Sulfate, Nitrate, Ammonium; CMIP6 dataset: Evolving Monthly AOD for Volcanic.
Emissions: Natural (sea-salt and dust) emissions are dynamic (fully interactive). Fossil-fuel and biomass burning CMIP6 inventories are used for sulphate precursors, black carbon and organic matter.

## Ocean
{% include resolution-summary.html source-id=page.source_id component="ocean" %}

SSTs outside the ocean model are daily interpolated from GCM monthly dataset.

## Model tuning

For the atmosphere, a machine-learning based approach called "history matching", adaptation to RCM of the approach described in [Hourdin et al. (2021)](https://doi.org/10.1029/2020MS002225) and [Couvreux et al. (2021)](https://doi.org/10.1029/2020MS002217) 
