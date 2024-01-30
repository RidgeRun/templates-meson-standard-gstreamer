python3 = import('python').find_installation()
list_plugin_res = run_command(python3, '-c',
'''
import sys
import json

with open("@0@") as f:
    print(':'.join(json.load(f).keys()), end='')
'''.format(join_paths(meson.current_source_dir(), 'gst_plugins_cache.json')),
  check: true)
foreach plugin_name: list_plugin_res.stdout().split(':')
    plugins_doc += [hotdoc.generate_doc(plugin_name,
        project_version: api_version,
        sitemap: 'sitemap.txt',
        index: 'index.md',
        gst_index: 'index.md',
        gst_smart_index: true,
        gst_c_sources: [
          '../../ext/*/*.[cmh]',
          '../../ext/*/*.cpp',
          '../../ext/*/*.hpp',
        ],
        gst_order_generated_subpages: true,
        gst_cache_file: join_paths(meson.current_source_dir(), 'gst_plugins_cache.json'),
        gst_plugin_name: plugin_name,
    )]
    sitemap += '        plugins/@0@-doc.json\n'.format(plugin_name)
endforeach

