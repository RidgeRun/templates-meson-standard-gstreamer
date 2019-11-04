# Define source code
gst_plugin_sources = [
  'gstTODO.c',
  'gstTODOplugin.c',
]

gst_plugin_headers = [
  'gstTODO.h',
]

# Build plugin library
gst_plugin = library('gstTODOplugin',
  gst_plugin_sources,
  c_args : c_args,
  include_directories : configinc,
  install : true,
  install_dir : plugins_install_dir,
  dependencies : [plugin_deps, gst_internal_dep] ,
)
