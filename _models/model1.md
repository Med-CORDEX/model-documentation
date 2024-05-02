---
source_id: Model1
label: Model 1
label_extended: My model 1 extended label
atmosphere:
  label: ALADIN 6.5
  resolution: 0.25 degree
  levels: 30
  TOA: 10 hPa
  physics:
    radiation:
      label: Lopez
      description: blah blah
      reference: Lopez (2002)
      reference-url: http://doi.org/XXXX
    convection:
      label: PCMT
      description: blah blah
      reference: Smith et al. (2020)
      reference-url: http://doi.org/XXXX
ocean:
  label: ORCA 5.2
  resolution: 0.25 degree
  levels: 40
land:
  label: MyLand 7
  resolution: 1/12 degree
  levels: 10
---

Intro on the coupled model. 
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

An example how to add an image. Upload to the images/ folder and voilà!
![image tooltip here]({{ site.baseurl }}/images/model1-example-image.png)
or even simpler if the image is already available online (no need to upload anything):
![CNRM logo](https://www.umr-cnrm.fr/squelettes/img/cnrm_beau25.png)

Further information:

 * [CNRM-ALADIN at umr-cnrm.fr](https://www.umr-cnrm.fr/spip.php?article125&lang=en)
 * ...

{% include toc %}

## Atmosphere
{% include resolution-summary.html source-id=page.source_id component="atmosphere" %}

Describe atmos component. 
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### Physics

Overall description of the physics. We can add a summary table built automatically from the front matter metadata:

{% include physics-atm.html source-id=page.source_id %}

## Ocean
{% include resolution-summary.html source-id=page.source_id component="ocean" %}

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Land
{% include resolution-summary.html source-id=page.source_id component="land" %}

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Urban

### References

 * Daniel M., Lemonsu A., Déqué M., Somot S., Alias A., Masson V. (2019) [Benefits of explicit urban parameterization in regional climate modeling to study climate and city interactions](doi:10.1007/s00382-018-4289-x). Climate Dynamics, 52(5-6), 2745-2764, doi:10.1007/s00382-018-4289-x
