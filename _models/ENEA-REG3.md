---
name: ENEA-REG3
family: ENEA-REG
dynamic_components:
  - atmosphere
  - land_surface
  - ocean
  - ocean_biogeochemistry
prescribed_components:
  - aerosol
omitted_components:
  - atmospheric_chemistry
  - land_ice
  - sea_ice
description: >
calendar: inherited # inherited|standard|365_day|360_day
release_year: 2021
references:
  - citation: Anav et al. (2024)
    doi: https://doi.org/10.1007/s00382-023-07064-3

# non-EMD (some of these could be retrieved from model registration info)
label: ENEA-REG v3.0
label_extended: The ENEA-REG regional Earth System Model version 3.0

atmosphere:
  component: atmosphere
  name: WRF v4.2.2
  family: WRF
  description: >
    The Weather Research and Forecasting (WRF) model ...
  references: 
    - citation: 
      doi: 
  code_base: https://github.com/wrf-model/WRF/tree/v4.2.2
  coupled_with:
    - land_surface
    - ocean
    - sea_ice
  native_horizontal_grid: &atmosphere-hgrid
    description: 
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
    description:
    coordinate: atmosphere_hybrid_height_coordinate
    n_z: 51
    top_of_model: 2000
    vertical_units: Pa
  # non-EMD
  time_step_s: &atmosphere-tstep 450 
  nudging: LBC # ( LBC | spectral | grid )
  hydrostatic: yes
  physics:
    radiation-shortwave:
      name: RRTMG
      description: 
      references: 
        - citation:
          doi:
    radiation-longwave:
      name: RRTMG
      description: Rapid radiative transfer model
      references:
        - citation: Mlawer et al. (1997)
          doi: https://doi.org/10.1029/97JD00237
    convection:
      name: Kain-Fritsch
      description: Prognostic Condensates Microphysics and Transport, a mass-flux scheme ...
      references:
        - citation: Guérémy (2011)
          doi: https://doi.org/
    shallow-convection:
      name: 
      description: 
      references:
        - citation: 
          doi: 
    microphysics:
      name: WSM5
      description: 
      references:
        - citation: 
          doi: 
    boundary-layer:
      name: YSU
      description: 
      references:
        - citation: 
          doi: 
  lateral_boundary_conditions:
    update_frequency: 6h
    nudging: LBCs with spectral nudging
    nesting: direct into GCM
    relaxation_scheme: according to Davies 1979

land_surface:
  component: land_surface
  name: Noah-MP v4.2
  family: Noah-MP
  description: >
  references:
    - citation: 
      doi: 
  code_base: https://github.com/wrf-model/WRF/tree/v4.2.2
  coupled_with:
    - atmosphere
  native_horizontal_grid: *atmosphere-hgrid
  native_vertical_grid:
    coordinate: depth
    n_z: 4
    top_layer_thickness: 0.1
    bottom_layer_thickness: 2
    vertical_units: m
  # non-EMD
  physics:
    soil:
      name: ISBA
      land-use-dataset:
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
    river_routing:
      name: CaMa-Flood v4.10
      family: 
      description: >
      references:
        - citation: 
          doi: 
      grid:
        resolution: 1/12 degree
        grid: regular
        n_cells: 1116x648
      time_step_s: 1800
      floodplains_parameterization: true
      groundwater_scheme: true
      carbon_to_ocean: false

ocean:
  component: ocean
  name: MITgcm z67
  family: MITgcm
  description: 
  references:
    - citation: 
      doi:
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
  time_step_s: 720
  lateral_boundary_condition:
    buffer_zone: false
    open_boundary_condition: true
    dataset: forcing GCM
    unbiased_dataset: "True, with ORAS5"
  physics:
    equation_of_seawater: TEOS10 (IOC et al., 2010)
    advection: TVD for tracers, EEN for momentum
    lateral_physics:
    vertical_physics:
  tidal_forcing: "False, enhancement of the bottom friction (Beuvier et al., 2012)"
  chlorophyll_concentration: "3D climatology (Zhang et al. 2024, accepted)"
  wave_representation: false
  tide_representation: false

ocean_biogeochemistry:
  component: ocean_biogeochemistry
  name: BFM-OGSTM vX.Y
  family: BFM
  description: >
    The BFM-OGSTM coupled software is essentially a numerical solver for the
    transport reaction equations where the reaction terms are defined by the
    Biogeochemical Flux Model (BFM) framework.
  references:
    - citation: Lazzari and Bolzon (2023)
      doi: https://www.bfm-community.eu/files/bfm-ogstm-manual_r1.0_202306.pdf
  code_base: https://github.com/inogs/ogstm
  coupled_with:
    - ocean
  native_horizontal_grid:
  native_vertical_grid:

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

{{ page.description }}

An example how to add an image. Upload to the images/ folder and voilà!
![image tooltip here]({{ site.baseurl }}/images/model1-example-image.png)
or even simpler if the image is already available online (no need to upload anything):
![CNRM logo](https://www.umr-cnrm.fr/squelettes/img/cnrm_beau25.png)

Further information:

 * [List of Med-CORDEX simulations with {{ page.name }}](https://wcrp-cordex.github.io/simulation-status/CMIP6_downscaling_plans.html?search.search=med-12%20{{ page.name }})

{% include toc %}

## Atmosphere
{% include resolution-summary.html component="atmosphere" %}

{{ page.atmosphere.description }}

### Physics

{% include physics.html component="atmosphere" %}

#### Radiation

{% include further-info-footer.html component="atmosphere"%}

## Land surface
{% include resolution-summary.html component="land_surface" %}

{{ page.land_surface.description }}

{% include further-info-footer.html component="land_surface" %}

### Physics

{% include physics.html component="land_surface" %}

## Aerosol
{% include resolution-summary.html component="aerosol" %}

Fully interactive: Sea salt, Desert Dust; Interactive + CMIP6 emission: Black Carbon, Organic Matter, Sulfate, Nitrate, Ammonium; CMIP6 dataset: Evolving Monthly AOD for Volcanic.
Emissions: Natural (sea-salt and dust) emissions are dynamic (fully interactive). Fossil-fuel and biomass burning CMIP6 inventories are used for sulphate precursors, black carbon and organic matter.

## Ocean
{% include resolution-summary.html component="ocean" %}

{{ page.ocean.description }}

SSTs outside the ocean model are daily interpolated from GCM monthly dataset.

{% include further-info-footer.html component="ocean" %}

## Spin-up strategy

## Model tuning


