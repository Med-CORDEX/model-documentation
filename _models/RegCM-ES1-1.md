---
name: RegCM-ES1-1
family: RegCM-ES
dynamic_components:
  - aerosol
  - atmosphere
  - land_surface
  - ocean
  - ocean_biogeochemistry
  - sea_ice
prescribed_components:
omitted_components:
  - atmospheric_chemistry
  - land_ice
description: >
  A brief, free-text scientific overview of the top-level model.  o The
  description should include a brief mention of all the components listed in the
  7.1 component CV, whether dynamically simulated, prescribed, or omitted.  o
  Dynamically simulated components (i.e. those listed by the dynamic_components
  property) are described elsewhere, as discussed in section 3. Model components,
  so only a very short, high-level overview is required in this description. For
  instance “The model includes a global process- based model of the land surface
  and the terrestrial biosphere that calculates water, energy, and carbon fluxes
  between the surface and the atmosphere”, and this description should also
  include the component’s name, if appropriate.  o Prescribed components (i.e.
  those listed by the prescribed_components property) are not described
  elsewhere, so information on how they are treated should be included the
  description. For instance “The model requires that a monthly mean, zonally
  averaged ozone field be prescribed because the model does not include an
  interactive atmospheric chemistry component”.  o Components that are wholly
  omitted (i.e. those listed by the omitted_components property) must be noted as
  such. For instance, “The model has no treatment of atmospheric chemistry”).
calendar: inherited # inherited|standard|365_day|360_day
release_year: 2025
references:
  - citation: Sevault (2024)
    doi: https://doi.org/10.5281/zenodo.11066601
  - citation: Darmaraki et al. (2019)
    doi: https://doi.org/10.1029/2019GL082933

# non-EMD (some of these could be retrieved from model registration info)
label: RegCM-ES1-1
label_extended: The Regional Earth System Model RegCM-ES version 1.1 based on RegCM v5.0, MITgcm v6.9d and CHyM
parent:
  name: CNRM-RCSM6
  description: CNRM-RCSM6B differs from its parent (CNRM-RCSM6) mainly in ...
  references:
    - citation: Sevault (2024)
      doi: https://doi.org/10.5281/zenodo.11066601
flux_correction: no

atmosphere:
  component: atmosphere
  name: RegCM v5.0
  family: RegCM
  description: 
  references: 
    - citation: Nabat et al. (2020)
      doi: https://doi.org/10.5194/acp-20-8315-2020
  code_base:
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
    n_z: 91
    top_of_model: 1
    vertical_units: Pa
  # non-EMD
  time_step_s: &atmosphere-tstep 450 
  nudging: LBC # ( LBC | spectral | grid )
  hydrostatic: yes
  physics:
    radiation_shortwave:
      name: FMR
      description: FMR 6 bands
      references: 
    radiation_longwave:
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
    shallow_convection:
      name: PCMT
      description: Prognostic Condensates Microphysics and Transport, a mass-flux scheme ...
      references:
        - citation: Piriou et al. (2007)
          doi: https://doi.org/
    microphysics:
      name: 
      description: 
      references:
        - citation: 
          doi: 
    boundary_layer:
      name: Cuxart
      description: 
      references:
        - citation: Cuxart et al. (2000)
          doi: https://doi.org/
  lateral_boundary_conditions:
    update_frequency: 6h
    nudging: LBCs with spectral nudging
    nesting: direct into GCM
    relaxation_scheme: according to Davies 1979

land_surface:
  component: land_surface
  name: NoahMP v?
  family: NoahMP
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
  code_base:
  coupled_with:
    - atmosphere
  native_horizontal_grid:
    description: 
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
    top_layer_thickness: 
    bottom_layer_thickness: 
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
      name: CHyM vx.x
      family: CHyM
      description: >
        CTRIP is a river routing model used to convert the daily runoff simulated
        by ISBA into river discharge on the global river channel network at
        1/12-degree resolution.
      references:
        - citation: Decharme et al. (2019) 
          doi:  
        - citation: Munier and Decharme (2022)
          doi: https://doi.org/10.5194/essd-14-2239-2022
      native_horizontal_grid:
        description:
        grid: regular_latitude_longitude
        grid_mapping: latitude_longitude
        region: limited_area
        temporal_refinement: static
        arrangement: arakawa_a # check
        resolution_x: 1/12
        resolution_y: 1/12
        horizontal_units: degree
        n_cells: 1116x648
      time_step_s: 1800
      floodplains_parameterization: true
      groundwater_scheme: true
      carbon_to_ocean: false

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
  native_vertical_grid: *atmosphere-vgrid
  # non-EMD
  transport: specific transport scheme (semi-lagrangian)
  concentrations:
  optical_radiative_properties:
  dataset: None

ocean:
  component: ocean
  name: MITgcm v6.9d
  family: MITgcm
  description: Regional version on the Mediterranean Sea of NEMOv3.6
  references:
    - citation: Beuvier et al. (2012)
      doi:
    - citation: Hamon et al. (2016)
      doi:
    - citation: Waldman et al. (2018)
      doi:
    - citation: Sevault (2024)
      doi:
  code_base:
  native_horizontal_grid: &ocean-hgrid
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
  feature_representation:
    - name: red_sea
      representation: none
    - name: black_sea
      representation: prescribed
      input: e_p_r_budget
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
    equation_of_seawater: TEOS10 (IOC et al., 2010)
    advection: TVD for tracers, EEN for momentum
    lateral_physics:
    vertical_physics:
  tidal_forcing: "False, enhancement of the bottom friction (Beuvier et al., 2012)"
  chlorophyll_concentration: "3D climatology (Zhang et al. 2024, accepted)"
  wave_representation: false
  tide_representation: false

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
  time_step_s: *atmosphere-tstep
  dynamics:
  thermodynamics:
  dataset: None

ocean_biogeochemistry:
  component: ocean_biogeochemistry
  name: PISCES vX.Y
  family: PISCES
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

wave:
  component: wave
  name: WaveWatch 3
  family: WaveWatch
  description: >
    The BFM-OGSTM coupled software is essentially a numerical solver for the
    transport reaction equations where the reaction terms are defined by the
    Biogeochemical Flux Model (BFM) framework.
  references:
    - citation: Lazzari and Bolzon (2023)
      doi: https://www.bfm-community.eu/files/bfm-ogstm-manual_r1.0_202306.pdf
  code_base: https://github.com/inogs/ogstm
  coupled_with:
    - atmosphere
    - ocean
  native_horizontal_grid: *ocean-hgrid
  native_vertical_grid:

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

---

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
{% include resolution-summary.html component="atmosphere" %}

{{ page.atmosphere.description }}

### Physics

{% include physics.html component="atmosphere" %}

#### Radiation

The radiative forcings include non-interactive greenhouse gases, tropospheric and stratospheric aerosols, ozone and solar. Greenhouse gases are CO2, N2O, CH4, CFC12 and a CFC11-equivalent species that includes the effects of all the other GHGs of the original data set (39 species). The solar constant uses the CMIP6 advised value (Matthes et al., 2017) including a 11-year cycle.

{% include further-info-footer.html component="atmosphere"%}

## Land surface
{% include resolution-summary.html component="land_surface" %}

{{ page.land_surface.description }}

{% include further-info-footer.html component="land_surface" %}

### Physics

{% include physics.html component="land_surface" %}

The soil hydrology uses the mixed form of the Richards equation to describe the water‐mass transfer within the soil via Darcy's law.
The one‐dimensional Fourier law is used to simulate temperature diffusion.
ISBA is sub-divided in 12 patches in order to account for the variety of soil and vegetation behaviors within a grid point.
Energy and water balances are calculated separately on each patch."

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

For the atmosphere, a machine-learning based approach called "history matching", adaptation to RCM of the approach described in [Hourdin et al. (2021)](https://doi.org/10.1029/2020MS002225) and [Couvreux et al. (2021)](https://doi.org/10.1029/2020MS002217).
No tuning for the river routing model.
