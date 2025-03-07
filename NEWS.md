# innsight 0.2.0

This is a minor release but does contain a range of substantial new features 
as well as visual changes, along with some bug fixes. This is accompanied 
by internal breaking changes in the R6 classes `Converter` and 
`ConvertedModel` enabling non-sequential models with multiple input or 
output layers. For users, however, nothing changes that is not set by 
default as in the previous version or made aware by warnings.

### Breaking changes  

There are no user-facing changes that are not handled with default values 
or noted by throwing warnings.

* When converting a model to a list, two necessary entries are added, 
containing the indices of the layers from the sublist `layers` indicating 
the input (`input_nodes`) and output (`output_nodes`) layers of the passed 
model. If one of these values is not set, a warning is raised and it is 
assumed that the model is sequential, i.e. the first layer is the only 
input layer and the last layer is the only output layer.

* Similarly, for each layer in the sublist `layers` the entries 
`input_layers` and `output_layers` are added containing the indices of the 
input and output layers for this layer. If these values are not set, a warning
is raised and it is assumed that the model is sequential, i.e. the 
previous entry is the only preceding and the next entry is the only 
succeeding layer. The values `0` and `-1` indicate the input and output 
layers of the model, respectively.

* The functions `plot` and `boxplot` for the interpretability methods no 
longer return instances of **ggplot2** or **plotly**, but instances of the new
S4 classes `innsight_ggplot2` or `innsight_plotly`. However, these objects can 
be treated like ordinary objects of **ggplot2** or **plotly** to some extent
and also create the usual visualizations by default (see [this section](https://bips-hb.github.io/innsight/articles/detailed_overview.html#advanced-plotting) in the
in-depth explanation for details). Since the results of models with multiple 
input and output layers are very complex, the suggested packages 
**gtable**, **grid** and **gridExtra** are needed only in these cases.

* Add **cli** dependency:  

  * Error messages, warnings, messages, and progress bars have been 
  revised and unified, and now use the package **cli**.
  
  * Overwrite the default `print()` function for the R6 classes `Converter` and 
  `InterpretingMethod`, which in particular is inherited by all interpretability
  methods.

### New features

* The `Converter` class now supports more models and layers:

  * Now models created by `keras::keras_model` are accepted. In addition, we
  add support for the following layers of the **keras** package:
  `layer_input`, `layer_concatenate`, 
  `layer_add`, `layer_activation*`, `layer_zero_padding_1d`, 
  `layer_zero_padding_2d`, `layer_batch_normalization`, 
  `layer_global_average_pooling_1d`, `layer_global_average_pooling_2d`,
  `layer_global_max_pooling_1d` and `layer_global_max_pooling_2d`
  
  * For models created by the **torch** package, we add support for 
  `nn_batch_norm1d` and `nn_batch_norm2d`
  
  * For models defined as a named list, we add the entries described in the 
  [breaking changes](#breaking-changes) and the following layer types 
  (see the [in-depth explanation](https://bips-hb.github.io/innsight/articles/detailed_overview.html#model-as-named-list) for details):
  
    * `type = "BatchNorm"` for batch normalization layers
    * `type = "GlobalPooling"` for all kinds of global pooling layers, i.e.
    maximum or average global pooling
    * `type = "Padding"` for padding layers
    * `type = "Concatenate"` for concatenation layer
    * `type = "Add"` for an adding layer

* Extend the arguments `output_idx` (in all interpretability methods and the 
corresponding plot and boxplot methods), `input_dim`, `input_names`, 
`output_dim`, `output_names` (in `Converter`), which now allow lists of these
arguments to define them for multiple input or output layers.

* Overwrite the default `print()` function for the R6 classes `Converter` and 
`InterpretingMethod`, which in particular is inherited by all interpretability 
methods.

* Add the S3 function `get_result()` for instances of the R6 class 
`InterpretingMethod` (i.e. also for all inherited methods) that forwards to the
corresponding class method `$get_result()`.

* In the method `LRP` it is now possible to set the rule and the parameter 
individually for each layer type. In addition, for batch normalization layers 
the rule `"pass"` is added, which skips this type of layer in the backward pass.

* Add the logical argument `winner_takes_all` to the methods `DeepLift` and 
`LRP` to treat maximum pooling layers as an average pooling layer in the 
backward pass.

* Add the logical argument `verbose` to all implemented methods to show or disable
the progress bar.

### Documentation and vignettes

* Revise the documentation and use roxygen templates (`@template`) for almost 
all fields and arguments. These are stored in the folder `man-roxygen`.

* Revise the introduction vignette `innsight` (`vignette("innsight")`).

* Add vignette "Example 1: Iris Dataset with `torch`" describing the basic
usage of the package with tabular data and only numeric features.

* Add vignette "Example 2: Penguin Dataset with `torch` and `luz`" describing
a more advanced usage with tabular data containing numerical and categorical
features.

* Add article "Example 3: ImageNet with `keras`" describing the usage of the
package with predefined models in `keras` on the ImageNet dataset.

* Add the vignette "In-depth Explanation" explaining all methods, arguments 
and possibilities of the package in great detail. This vignette also includes 
the depreciated vignette "Custom Model Definition".

* The vignette "Custom Model Definition" is deprecated.

### Minor improvements and bug fixes

* Small speed improvements by using more **torch** functions, e.g. 
`torch_clip(x, min = 0)` instead of `(x > 0) * x`

* Some smaller bug fixes


# innsight 0.1.1

* Fix problem with old HTML version of the manual by re-generating the
`.Rd` files using the current CRAN version of roxygen2.

# innsight 0.1.0

* Added a `NEWS.md` file to track changes to the package.
