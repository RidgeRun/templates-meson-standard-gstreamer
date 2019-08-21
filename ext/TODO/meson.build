# Define source code
gst_plugin_sources = [
  'gstTODO.c',
  'gstTODOplugin.c',
]

gst_plugin_headers = [
  'gstTODO.h',
]

# Build plugin library
gst_plugin = library('gstTODOplugin-@0@'.format(api_version),
  gst_plugin_sources,
  c_args : c_args,
  version : api_version,
  include_directories : configinc,
  install : true,
  install_dir : plugins_install_dir,
  dependencies : [plugin_deps, gst_internal_dep] ,
)

pkgconfig.generate(gst_plugin, install_dir : plugins_pkgconfig_install_dir)
