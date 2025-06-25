---
name: CNRM-RCSM6B
family: CNRM-RCSM
dynamic_components:
  - aerosol
  - atmosphere
  - land_surface
  - ocean
  - sea_ice
#prescribed_components:
omitted_components:
  - atmospheric_chemistry
  - land_ice
  - ocean_biogeochemistry
description: >
  Regional Climate System models allow to reproduce medium scale atmospheric and
  oceanic phenomena on periods going from present time hindcast to scenarios for
  the 21st century. Atmosphere, aerosol, land surface and sea-ice are grouped
  in one executable (ALADIN), river routing (CTRIP), ocean (NEMOMED12); the
  models are coupled with OASIS3-MCT, the outputs are managed by XIOS. Each
  version of CNRM-RCSM6 uses the last versions of ALADIN, CTRIP and NEMOMED12
  available at a given time.
calendar: inherited # inherited|standard|365_day|360_day
release_year: 2023
references:
  - citation: Sevault (2024)
    doi: https://doi.org/10.5281/zenodo.11066601
  - citation: Darmaraki et al. (2019)
    doi: https://doi.org/10.1029/2019GL082933

# non-EMD (some of these could be retrieved from model registration info)
label: CNRM-RCSM version 6B
label_extended: Version 6B of the fully-coupled Regional Climate System Model (RCSM) of the Centre National de Recherches Meteorologiques (CNRM)
parent:
  name: CNRM-RCSM5
  references:
    - citation: Sevault (2024)
      doi: https://doi.org/10.5281/zenodo.11066601

atmosphere:
  component: atmosphere
  name: CNRM-ALADIN65
  family: CNRM-ALADIN
#  description: 
  references: 
    - citation: Nabat et al. (2020)
      doi: https://doi.org/10.5194/acp-20-8315-2020
#  code_base:
  coupled_with:
    - land_surface
    - ocean
    - sea_ice
  native_horizontal_grid: &atmosphere-hgrid
#    description: 
    grid: plane_projection
    grid_mapping: lambert_conformal_conic
    region: limited_area
    temporal_refinement: static
    arrangement: arakawa_c # check
    resolution_x: 12.5 
    resolution_y: 12.5 
    horizontal_units: km
    n_cells: 613x405
    # non-EMD
    n_cells_full: 640x432
    n_cells_buffer: 8
  native-vertical-grid: &atmosphere-vgrid
#    description:
    coordinate: atmosphere_hybrid_height_coordinate
    n_z: 91
    top_of_model: 1
    vertical_units: Pa
  # non-EMD
  time-step: &atmosphere-tstep 450 s 
  nudging: LBC # ( LBC | spectral | grid )
  hydrostatic: yes
  physics:
    radiation-shortwave:
      name: FMR
      description: FMR 6 bands
#      references: 
    radiation-longwave:
      name: RRTM
      description: Rapid radiative transfer model
      references:
        - citation: Mlawer et al. (1997)
          doi: https://doi.org/10.1029/97JD00237
    convection:
      name: PCMT
      description: Prognostic Condensates Microphysics and Transport, a mass-flux scheme ...
      references:
        - citation: Guérémy (2011)
          doi: https://doi.org/
    shallow-convection:
      name: PCMT
      description: Prognostic Condensates Microphysics and Transport, a mass-flux scheme ...
      references:
        - citation: Piriou et al. (2007)
          doi: https://doi.org/
    boundary-layer:
      name: Cuxart
#      description: 
      references:
        - citation: Cuxart et al. (2000)
          doi: https://doi.org/
  lateral-boundary-conditions:
    update-frequency: 6h
    nudging: LBCs with spectral nudging
    nesting: direct into GCM
    relaxation-scheme: according to Davies 1979

land_surface:
  component: land_surface
  name: SURFEX v8.0
  family: SURFEX
  description: >
    SURFEXv8.0 encompasses several submodules for modeling the interactions between
    the atmosphere, the ocean, the lakes and the land surface. The FLAKE model
    simulate surface fluxes over lakes, including both the Caspian and the Aral
    seas, and computes the temporal evolution of the vertical lake temperature
    profile from the surface mixing layer to the bottom (Le Moigne et al. 2016).
    The land surface is represented using the new ISBA-CTRIP coupled system
    (Decharme et al., 2019; Delire et al. 2019). ISBA calculates the time evolution
    of the energy, carbon and water budgets at the land surface while CTRIP
    simulates river discharges (including carbon transport) up to the ocean from
    the total runoff (and total soil carbon leaching) computed by ISBA.
  references:
    - citation: Decharme et al. (2019)
      doi: http://dx.doi.org/10.1029/2018MS001545
#  code_base:
  coupled_with:
    - atmosphere
  native_horizontal_grid:
#    description: 
    grid: plane_projection
    grid_mapping: lambert_conformal_conic
    region: limited_area
    temporal_refinement: static
    arrangement: arakawa_a # check
    resolution_x: 25 
    resolution_y: 25 
    horizontal_units: km
    n_cells: 613x405 # check
  native_vertical_grid:
    coordinate: depth
    n_z: 30
#    top_layer_thickness: 
#    bottom_layer_thickness: 
    vertical_units: m
  # non-EMD
  physics:
    soil:
      name: ISBA
#      land-use-dataset:
      land-use-change: no
    urban:
      name: none
      comment: Cities are represented as rock surfaces
    lake:
      name: FLake vX.X
      description: FLAKE single column model
      references:
        - citation: Mironov (2008)
          doi:
    glacier: # there is a land_ice component in EMD
      name: none
      comment: Simple parameterization to avoid snow accumulation
    river-routing:
      name: ISBA-CTRIP vx.x
      family: TRIP
      description: >
        CTRIP is a river routing model used to convert the daily runoff simulated
        by ISBA into river discharge on the global river channel network at
        1/12-degree resolution.
      references:
        - citation: Decharme et al. (2019) 
          doi:  
        - citation: Munier and Decharme (2022)
          doi: https://doi.org/10.5194/essd-14-2239-2022
      grid:
        resolution: 1/12 degree
        grid: regular
        n_cells: 1116x648
      time-step: 1800 s
      floodplains-parameterization: true
      groundwater-scheme: true
      carbon-to-ocean: false
      spin-up: true

aerosol:
  component: aerosol
  name: TACTIC v
  family: AER # check
  description:
  references:
    - citation: Nabat et al. (2020)
      doi: 
  code_base:
  embedded_in:
    - atmosphere
  native_horizontal_grid: *atmosphere-hgrid
  native-vertical-grid: *atmosphere-vgrid
  # non-EMD
  transport: specific transport scheme (semi-lagrangian)
  concentrations:
  optical-radiative-properties:
  dataset: None

ocean:
  component: ocean
  name: NEMOMED12 v3.6
  family: NEMO
  description: Regional version on the Mediterranean Sea of NEMOv3.6
  references:
    - citation: Beuvier et al., 2012, Hamon et al., 2016, Waldman et al., 2018, Sevault 2024
    - doi:
  code_base:
  native_horizontal_grid:
    description: ORCA tripolar grid
    grid: tripolar
    grid_mapping: latitude_longitude
    region: limited_area
    temporal_refinement: static
    arrangement: arakawa_c # check
    resolution_range_km:  
    mean_resolution_km:  
    horizontal_units: degree
    n_cells: 567x264
  native-vertical-grid:
    description: >
      75 vertical levels using a vertical z-coordinate with partial step bathymetry
      formulation (Barnier et al., 2006), from 1m to 135m
    coordinate: ocean_sigma_z_coordinate # check
    n_z: 75
    vertical_units: m
  # non-EMD
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

sea_ice:
  component: sea_ice
  name: GELATO 1D
  family: GELATO
  description:
  references:
    - citation:
      doi:
  code_base:
  coupled_with:
    - atmosphere
    - ocean
  # non-EMD
  time-step: *atmos-time-step
  dynamics:
  thermodynamics:
  dataset: None
  spin-up: true

# non-EMD
coupler:
  component: coupler
  name: OASIS3-MCT
  family: OASIS
  description:
  references:
    - citation: Craig et al. (2017)
      doi: http://doi.org/10.5194/gmd-10-3297-2017
  code_base:
  coupling_frequency: 1hr
  flux_correction: no

---

Regional Climate System models allow to reproduce medium scale atmospheric and oceanic phenomena on periods going from present time hindcast to scenarios for the 21st century.
Atmosphere, aerosol, land surface and sea-ice are grouped in one executable (ALADIN), river routing (CTRIP), ocean (NEMOMED12); the models are coupled with OASIS3-MCT, the outputs are managed by XIOS.
Each version of CNRM-RCSM6 uses the last versions of ALADIN, CTRIP and NEMOMED12 available at a given time.

{{ page.description }}

An example how to add an image. Upload to the images/ folder and voilà!
![image tooltip here]({{ site.baseurl }}/images/model1-example-image.png)
or even simpler if the image is already available online (no need to upload anything):
![CNRM logo](https://www.umr-cnrm.fr/squelettes/img/cnrm_beau25.png)

Further information:

 * [CNRM-RCSM at umr-cnrm.fr](https://www.umr-cnrm.fr/spip.php?article1098)
 * [List of Med-CORDEX simulations with {{ page.name }}](https://wcrp-cordex.github.io/simulation-status/CMIP6_downscaling_plans.html?search.search=med-12%20{{ page.name }})

{% include toc %}

## Atmosphere
{% include resolution-summary.html name=page.name component="atmos" %}

{{ page.atmosphere.description }}

### Physics

{% include physics.html name=page.name component="atmosphere" %}

#### Radiation

The radiative forcings include non-interactive greenhouse gases, tropospheric and stratospheric aerosols, ozone and solar. Greenhouse gases are CO2, N2O, CH4, CFC12 and a CFC11-equivalent species that includes the effects of all the other GHGs of the original data set (39 species). The solar constant uses the CMIP6 advised value (Matthes et al., 2017) including a 11-year cycle.

## Land surface
{% include resolution-summary.html name=page.name component="land_surface" %}

{{ page.land_surface.description }}

The soil hydrology uses the mixed form of the Richards equation to describe the water‐mass transfer within the soil via Darcy's law.
The one‐dimensional Fourier law is used to simulate temperature diffusion.
ISBA is sub-divided in 12 patches in order to account for the variety of soil and vegetation behaviors within a grid point.
Energy and water balances are calculated separately on each patch."

SURFEX v8.0 encompasses several submodules for modeling the interactions between the atmosphere, the ocean, the lakes and the land surface.
The FLAKE model simulate surface fluxes over lakes, including both the Caspian and the Aral seas, and computes the temporal evolution of the vertical lake temperature profile from the surface mixing layer to the bottom (Le Moigne et al. 2016).
The land surface is represented using the new ISBA-CTRIP coupled system (Decharme et al., 2019; Delire et al. 2019).
ISBA calculates the time evolution of the energy, carbon and water budgets at the land surface while CTRIP simulates river discharges (including carbon transport) up to the ocean from the total runoff (and total soil carbon leaching) computed by ISBA."

{% include physics.html name=page.name component="land_surface" %}

### References

 * Daniel M., Lemonsu A., Déqué M., Somot S., Alias A., Masson V. (2019) [Benefits of explicit urban parameterization in regional climate modeling to study climate and city interactions](doi:10.1007/s00382-018-4289-x). Climate Dynamics, 52(5-6), 2745-2764, doi:10.1007/s00382-018-4289-x

## Aerosol
{% include resolution-summary.html name=page.name component="aerosol" %}

Fully interactive: Sea salt, Desert Dust; Interactive + CMIP6 emission: Black Carbon, Organic Matter, Sulfate, Nitrate, Ammonium; CMIP6 dataset: Evolving Monthly AOD for Volcanic.
Emissions: Natural (sea-salt and dust) emissions are dynamic (fully interactive). Fossil-fuel and biomass burning CMIP6 inventories are used for sulphate precursors, black carbon and organic matter.

## Ocean
{% include resolution-summary.html name=page.name component="ocean" %}

SSTs outside the ocean model are daily interpolated from GCM monthly dataset.

## Model tuning

For the atmosphere, a machine-learning based approach called "history matching", adaptation to RCM of the approach described in [Hourdin et al. (2021)](https://doi.org/10.1029/2020MS002225) and [Couvreux et al. (2021)](https://doi.org/10.1029/2020MS002217) 
