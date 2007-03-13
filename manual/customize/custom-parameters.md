Custom parameters
=================

You can create an unlimited number of custom parameters to be used with [authorization scenarios](/manual/authorization-scenarios), [web templates](#web_template_files) and [mail templates](#template_file_format).

These parameters are defined in each list configuration through the web interface by using the form in *Admin -> Edit list config -> Miscellaneous* page. There, you add a parameter in the **custom parameters (custom\_vars)** section. The **var name** field corresponds to your custom parameter name, the **var value** field corresponds to your custom parameter value.

You can later access this parameter:

  - in scenarios : with the syntax `[custom_vars->your_custom_var_name]`

  - in web or mail templates : with the syntax `custom_vars.your_custom_var_name`

### Example

You define a custom parameter with the following values:

  - var name : `sisterList`

  - var value : `math-teachers`

You can use it as follows:

  - in scenarios : with the syntax `[custom_vars->sisterList]`, which will correspond to "math-teachers"

  - in web or mail templates : with the syntax `custom_vars.sisterList`, which will correspond to "math-teachers"

