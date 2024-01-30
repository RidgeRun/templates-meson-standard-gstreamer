# Define source code
gst_plugin_sources = [
  'gstTODO.c',
  'gstTODOplugin.c',
]

gst_plugin_headers = [
  'gstTODO.h',
]

# Build plugin library
gst_plugin = library('gstTODO',
  gst_plugin_sources,
  c_args : c_args,
  include_directories : configinc,
  install : true,
  install_dir : plugins_install_dir,
  dependencies : [plugin_deps, gst_internal_dep] ,
)
plugins += [gst_plugin]
gst_plugin_dep = declare_dependency(link_with : gst_plugin)
gst_doc_dep += [gst_plugin_dep]
