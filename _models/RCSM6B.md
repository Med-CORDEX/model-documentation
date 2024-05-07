---
source_id: RCSM6B
label: RCSM v6 B
label_extended: Regional Climate System Model, version 6B

atmos:
  label: ALADIN 6.4
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
      reference: Lopez (2002)
      reference-url: https://doi.org/10.1256/00359000260498879

land:
  label: SURFEX 4.7
  levels: 30
  projection: lambert_conformal_conic
  resolution: 25 km
  physics:
    urban:
      label: none
    lake:
      label: none
    glacier:
      label: none

hydrology:
  label: MyLand 7
  projection: lambert_conformal_conic
  resolution: 25 km

ocean:
  label: ORCA 5.2
  resolution: 0.25 degree
  levels: 40
---

Intro on the coupled model. 
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

An example how to add an image. Upload to the images/ folder and voilà!
![image tooltip here]({{ site.baseurl }}/images/model1-example-image.png)
or even simpler if the image is already available online (no need to upload anything):
![CNRM logo](https://www.umr-cnrm.fr/squelettes/img/cnrm_beau25.png)

Further information:

 * [CNRM-ALADIN at umr-cnrm.fr](https://www.umr-cnrm.fr/spip.php?article125&lang=en)
 * [List of Med-CORDEX simulations with {{ page.source_id }}](https://wcrp-cordex.github.io/simulation-status/CMIP6_downscaling_plans.html?search.search=med-12%20{{ page.source_id }})

{% include toc %}

## Atmosphere
{% include resolution-summary.html source-id=page.source_id component="atmos" %}

Describe atmos component. 
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Physics

Overall description of the physics. We can add a summary table built automatically from the front matter metadata:

{% include physics.html source-id=page.source_id component="atmos" %}

## Ocean
{% include resolution-summary.html source-id=page.source_id component="ocean" %}

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Land
{% include resolution-summary.html source-id=page.source_id component="land" %}

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

{% include physics.html source-id=page.source_id component="land" %}

### References

 * Daniel M., Lemonsu A., Déqué M., Somot S., Alias A., Masson V. (2019) [Benefits of explicit urban parameterization in regional climate modeling to study climate and city interactions](doi:10.1007/s00382-018-4289-x). Climate Dynamics, 52(5-6), 2745-2764, doi:10.1007/s00382-018-4289-x
