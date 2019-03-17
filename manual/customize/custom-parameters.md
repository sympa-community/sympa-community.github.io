---
title: 'Custom list parameters'
up: ../customize.md#customizing-sympa-services
---

Custom list parameters
======================

You can create an unlimited number of custom parameters to be used with [authorization scenarios](basics-scenarios.md) and [mail/web templates](basics-templates.md#mail-and-web-templates).

These parameters are defined in each list configuration through the web interface by using the form in "Admin" -> "Edit list config" -> "Miscellaneous" page. There, you add a parameter in the "[custom parameters (`custom_vars`)](/gpldoc/man/list_config.5.html#custom_vars)" section. The "var name (name)" field corresponds to your custom parameter name, and the "var value (value)" field corresponds to your custom parameter value.

You can later access this parameter:

  - in scenarios: with the variable `[custom_vars->`*your_custom_var_name*`]`
  - in web or mail templates: with the variable `[%custom_vars.`*your_custom_var_name*`%]`

### Example

You define a custom parameter with the following values:

  - var name: `sisterList`
  - var value: `math-teachers`

You can use it as follows:

  - in scenarios: with the variable `[custom_vars->sisterList]`, which will correspond to "math-teachers"
  - in web or mail templates: with the variable `[%custom_vars.sisterList%]`, which will correspond to "math-teachers"

