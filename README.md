# Med-CORDEX model documentation

This is currently a test site to document the coupled regional climate models used in the [Med-CORDEX](https://medcordex.eu) initiative (Phase 3) contributing to [CORDEX-CMIP6](https://www.cordex.org).
Models are documented in individual [Markdown](https://www.markdownguide.org/getting-started/) files with a [YAML](https://en.wikipedia.org/wiki/YAML) preamble which facilitates machine-readability of the model metadata.
These metadata can then be processed to create summary tables of the Med-CORDEX ensemble, as those shown in our [model documentation website](https://med-cordex.github.io/model-documentation), which was created using [Jekyll](https://jekyllrb.com).
The files documenting each model can be found under the [_models/](_models/) folder.

## How to contribute

You can contribute to this effort in several ways:

* Contribute to the ongoing discussion on the model documentation.
  We are still developing the framework, so any insight is welcome.
  Check the [open issues](https://github.com/Med-CORDEX/model-documentation/issues) or create a new one.

* Report any error or inconsistency you might find by opening an [issue](https://github.com/Med-CORDEX/model-documentation/issues) o proposing a PR to fix it.

* If you are a model developer, make sure your model is well documented.
  You can start creating the entry for your model by using any of the other models in [_models/](_models/) as a template.
  Fork the repository and make a pull request when you are ready to see the result.
  > [!IMPORTANT]
  > The YAML format is extremely sensitive to indentation.
  > Double-check the different indentation levels in your file.
