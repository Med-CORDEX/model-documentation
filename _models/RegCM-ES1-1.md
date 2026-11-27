---
name: RegCM-ES1-1
family: RegCM-ES
dynamic_components:
  - atmosphere
  - land_surface
  - ocean
  - river
prescribed_components:
omitted_components:
  - aerosol
  - sea_ice
  - ocean_biogeochemistry
  - atmospheric_chemistry
  - land_ice
description: >
  The RegCM-ES1-1 is a coupled model built around the RegESM coupler.
  The atmosphere model is the ICTP RegCM5, which is internally coupled with
  the CLM4.5 land surface model. The ocean model is MITgcm, adapted for the
  coupling with the RegESM. The river routing model is a fork adapted for
  coupling of the CHyM-roff model, itself a runoff only input of the CETEMPS
  CHyM model. Anthropogenic aerosol is either coming from MERRA2 reanalysis
  or from the Inpu4MIPS Simple Plume model. The sea ice model, the atmospheric
  chemistry, the ocean biogeoochemistry  and land ice glaier are omitted.
calendar: inherited # inherited|standard|365_day|360_day
release_year: 2025
references:
  - citation: Giuliani et al., [2026]
    doi: in preparation
  - citation: Coppola et al. [2025]
    doi: https://doi.org/10.1007/s00382-025-07913-3
  - citation: Giorgi et al, [2023]
    doi: https://doi.org/10.1029/2022JD038199

# non-EMD (some of these could be retrieved from model registration info)
label: RegCM-ES1-1
label_extended: The Regional Earth System Model RegCM-ES version 1.1 based on RegCM v5.0, MITgcm v6.9d and CHyM
parent:
  name: RegCM-ES1-1
  description: The Unesco ICTP Earth System model
  references:
    - citation: in preparation
      doi: 
flux_correction: no

atmosphere:
  component: atmosphere
  name: RegCM v5.0
  family: RegCM
  description: 
  references: 
    - citation: Giorgi et al, (2023)
      doi: https://doi.org/10.1029/2022JD038199
  code_base:
  coupled_with:
    - land_surface
    - ocean
    - sea_ice
  native_horizontal_grid: &atmosphere-hgrid
    description: 
    grid: plane_projection
    grid_mapping: rotated_latitude_longitude
    region: limited_area
    temporal_refinement: static
    arrangement: arakawa_c # check
    resolution_x: 0.11
    resolution_y: 0.11 
    horizontal_units: degrees
    n_cells: 421x291
    # non-EMD
    n_cells_full: 438x308
    n_cells_buffer: 16
  native_vertical_grid: &atmosphere-vgrid
    description:
    coordinate: atmosphere_hybrid_height_coordinate
    n_z: 30
    top_of_model: 30000
    vertical_units: m
  # non-EMD
  time_step_s: &atmosphere-tstep 120 
  nudging: LBC + spectral # ( LBC | spectral | grid )
  hydrostatic: no
  physics:
    radiation_shortwave:
      name: ccm3
      description: NCAR CCM3 radiation scheme
      references: Kiehl et al. [1996]
    radiation_longwave:
      name: ccm3
      description: NCAR CCM3 radiation scheme
      references:
        - citation: Kiehl et al. [1996]
          doi: https://doi.org/10.5065/D6FF3Q99
    convection:
      name: Tiedtke
      description: Mass-flux scheme with transport represented by a spectrum of entraining and detraining plumes.
      references:
        - citation: Tiedtke [1989]
          doi: https://doi.org/10.1175/1520-0493(1989)117%3C1779:ACMFSF%3E2.0.CO;2
    shallow_convection:
      name: Tiedtke
      description: Mass-flux scheme with transport represented by a spectrum of entraining and detraining plumes.
      references:
        - citation: Tiedtke [1989]
          doi: https://doi.org/10.1175/1520-0493(1989)117%3C1779:ACMFSF%3E2.0.CO;2
    microphysics:
      name: Nogherotto - Tompkins
      description: equations for five water species and advances them with an implicit matrix solver that simultaneously conserves total water and moist static energy
      references:
        - citation: Nogherotto et al. [2016]
          doi: https://doi.org/10.5194/gmd-9-2533-2016
    boundary_layer:
      name: Holtslag
      description: non-local diffusion concept that takes into account counter-gradient fluxes resulting from large-scale eddies in an unstable, well-mixed atmosphere
      references:
        - citation: Holtslag et al. [1990]
          doi: https://doi.org/10.1175/BAMS-D-11-00187.1
  lateral_boundary_conditions:
    update_frequency: Exponential nudging
    nudging: LBCs
    nesting: direct into GCM
    relaxation_scheme: Exponential relaxation
        - citation: Giorgi et al. [1993]
          doi: https://doi.org/10.1175/1520-0493(1993)121%3C2794:DOASGR%3E2.0.CO;2

land_surface:
  component: land_surface
  name: The Community Land Model CLM4.5 (CESM).
  family: CLM
  description: >
    NCAR CESM CLM4.5 adapted in RegCM
  references:
    - citation: Oleson et al. [2013]
      doi: https://doi.org/10.5065/D6RR1W7M
  code_base: https://github.com/ICTP/RegCM/tree/master/Main/clmlib/clm4.5
  coupled_with:
    - atmosphere
  native_horizontal_grid:
    description: Same as atmosphere
    grid: plane_projection
    grid_mapping: rotated_latitude_longitude
    region: limited_area
    temporal_refinement: static
    resolution_x: 0.11 
    resolution_y: 0.11
    horizontal_units: degrees
    n_cells: 438x308
  native_vertical_grid:
    coordinate: depth
    n_z: 10
    top_layer_thickness: 0.0175128179162552
    bottom_layer_thickness: 3.80188191232272
    vertical_units: m
  # non-EMD
  physics:
    soil:
      name: PFT based CLM4.5 + TopModel
      land-use-dataset: CESM CLM4.5
      land-use-change: no
    urban:
      name: CLMU in CLM4.5
      comment: urban model with a single-layer canopy scheme under the Earth system model hierarchy
    lake:
      name: LISSS
      description: one-dimensional lake physics module inside the Community Land Model version 4.5 (CLM4.5)
      references:
        - citation: Subin et al. [2012]
          doi: https://doi.org/10.1029/2011MS000072
    glacier: # there is a land_ice component in EMD
      name: none
      comment: Simple parameterization to avoid snow accumulation
    river_routing:
      name: CHyM-cpl
      family: CHyM
      description: Coupled version of CHyM-roff, the runoff based version of the CHyM CETEMPS model with a river channel network at 1/12-degree resolution.
      references:
        - citation: Di Sante et al. [2021]
          doi: https://doi.org/10.1002/joc.7014
      native_horizontal_grid:
        description: Same as ocean model
        grid: ORCA NEMO tripolar
        grid_mapping: latitude_longitude
        region: limited_area
        temporal_refinement: static
        arrangement: 
        resolution_x: 1/12
        resolution_y: 1/12
        horizontal_units: degree
        n_cells: 616x616
      time_step_s: 300
      floodplains_parameterization: false
      groundwater_scheme: false
      carbon_to_ocean: false
      damming: true

ocean:
  component: ocean
  name: MITgcm v6.9d
  family: MITgcm
  description: Regional MITgcm configuration with ESMF coupler
  references:
    - citation: Marshall et al. [1997]
      doi: https://doi.org/10.1029/96JC02775
  code_base: https://github.com/graziano-giuliani/MITgcm
  native_horizontal_grid: &ocean-hgrid
    description: ORCA tripolar grid
    grid: tripolar
    grid_mapping: latitude_longitude
    region: limited_area
    temporal_refinement: static
    arrangement: arakawa_c
    resolution_range_km: 10
    mean_resolution_km: 10
    horizontal_units: degree
    n_cells: 616x616
  native_vertical_grid:
    description: >
      75 vertical levels using a vertical z-coordinate with partial step bathymetry
      formulation (Barnier et al., 2006), from 1m to 135m
    coordinate: ocean_sigma_z_coordinate
    n_z: 75
    vertical_units: m
  # non-EMD
  time_step_s: 150
  feature_representation:
    - name: red_sea
      representation: none
    - name: black_sea
      representation: coupled
    - name: rivers
      representation: coupled
      description: Coupled river input except for the Nile river
    - name: nile_river
      representation: prescribed
      input: climatological_seasonal_cycle
      description: >
        A climatological seasonal cycle is applied, with a yearly average of 444 m3 s-1
        (Erika Coppola, Med-CORDEX community,
        https://www.medcordex.eu/Med-CORDEX-2 baseline-runs protocol.pdf)
  lateral_boundary_condition:
    buffer_zone: false
    open_boundary_condition: true
    dataset: forcing GCM
    unbiased_dataset: "True, with ORAS5"
  physics:
    equation_of_seawater: MDJWF
    advection: 3rd-order Flux-Limiting Advection Scheme using a DST3 (Direct Space-Time 3rd-order) formulation with a Multi-Dimensional Flux Limiter
    lateral_physics: no_slip sides and bottom
    vertical_physics: GGL90 TKE vertical mixing scheme
  tidal_forcing: false
  chlorophyll_concentration: false
  wave_representation: false
  tide_representation: false

coupler:
  component: coupler
  name: RegCM-ES
  family: ESMF NUOPC
  description: Regional Climate model coupler forked from RegESM
  references:
    - citation: Turuncoglu et al [2017]
      doi: https://doi.org/10.1007/s00382-016-3241-1
  code_base: https://github.com/graziano-giuliani/RegESM
  coupling_frequency: 1hr ATM-OCN, 1d ATM-RTM-OCN

---

{{ page.description }}

An example how to add an image. Upload to the images/ folder and voilà!
![image tooltip here]({{ site.baseurl }}/images/model1-example-image.png)
or even simpler if the image is already available online (no need to upload anything):
![CNRM logo](https://www.umr-cnrm.fr/squelettes/img/cnrm_beau25.png)

Further information:

 * [Experiment environment](https://github.com/graziano-giuliani/MED12-ocean-mit)
 * [List of Med-CORDEX simulations with {{ page.name }}](https://wcrp-cordex.github.io/simulation-status/CMIP6_downscaling_plans.html?search.search=med-12%20{{ page.name }})

{% include toc %}

## Atmosphere
{% include resolution-summary.html component="atmosphere" %}

{{ page.atmosphere.description }}

### Physics

{% include physics.html component="atmosphere" %}

#### Radiation

The radiative forcings include non-interactive greenhouse gases, tropospheric and stratospheric aerosols, ozone and solar. Greenhouse gases are CO2, N2O, CH4, CFC12 and a CFC11-equivalent specie. The solar constant uses the CMIP6 advised value (Matthes et al., 2017) including a 11-year cycle.

{% include further-info-footer.html component="atmosphere"%}

## Land surface
{% include resolution-summary.html component="land_surface" %}

{{ page.land_surface.description }}

{% include further-info-footer.html component="land_surface" %}

### Physics

{% include physics.html component="land_surface" %}

## Aerosol
{% include resolution-summary.html component="aerosol" %}

## Ocean
{% include resolution-summary.html component="ocean" %}

{{ page.ocean.description }}

SSTs outside the ocean model are daily interpolated from GCM monthly dataset.

{% include further-info-footer.html component="ocean" %}

## Spin-up strategy

## Model tuning
