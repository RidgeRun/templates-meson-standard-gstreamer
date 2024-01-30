build_hotdoc = false

if meson.is_cross_build()
    if docs_enabled
        error('Documentation enabled but building the doc while cross building is not supported yet.')
    endif

    message('Documentation not built as building it while cross building is not supported yet.')
    subdir_done()
endif


required_hotdoc_extensions = ['gst-extension']

plugins_cache = join_paths(meson.current_source_dir(), 'plugins/gst_plugins_cache.json')
plugins_doc_dep = []
plugins_doc = []

if get_option('gst-doc-generate-gst-doc-cache').enabled()
    gst_c_args = ['-DHAVE_CONFIG_H']

    hotdoc_plugin_scanner = executable('gst-hotdoc-plugins-scanner',
    'gst-hotdoc-plugins-scanner.c',
    c_args : gst_c_args,  
    include_directories : [configinc],
    dependencies : gst_doc_dep,
    install_dir : helpers_install_dir,
    install: true,
    )

    configure_file(
        input: 'gst-plugins-doc-cache-generator.py',
        output: 'gst-plugins-doc-cache-generator',
        install_dir: helpers_install_dir,
        copy: true
    )

    plugins_cache_generator = find_program(join_paths(meson.current_build_dir(), 'gst-plugins-doc-cache-generator'))

    if plugins_cache_generator.found()
        plugins_cache = join_paths(meson.current_build_dir(), 'plugins/gst_plugins_cache.json')
        plugins_doc_dep = custom_target('TODO-plugins-doc-cache',
            command: [plugins_cache_generator, plugins_cache, '@OUTPUT@', '@INPUT@'],
            input: plugins,
            output: 'gst_plugins_cache.json',
            depends: [hotdoc_plugin_scanner],
            build_always: true,
            build_by_default: true
        )
    else
        warning('GStreamer plugin inspector for documentation not found, can\'t update the cache')
    endif
endif

hotdoc_p = find_program('hotdoc', required: docs_enabled)
if not hotdoc_p.found()
    message('Hotdoc not found, not building the documentation')
    subdir_done()
endif

hotdoc_req = '>= 0.11.0'
hotdoc_version = run_command(hotdoc_p, '--version', check: true).stdout()
if not hotdoc_version.version_compare(hotdoc_req)
    if docs_enabled
        error('Hotdoc version @0@ not found, got @1@'.format(hotdoc_req, hotdoc_version))
    else
        message('Hotdoc version @0@ not found, got @1@'.format(hotdoc_req, hotdoc_version))
        subdir_done()
    endif
endif

build_hotdoc = true
hotdoc = import('hotdoc')
docconf = configuration_data()
docconf.set('GST_API_VERSION', api_version)

foreach extension: required_hotdoc_extensions
    if not hotdoc.has_extensions(extension)
        if docs_enabled
            error('Documentation enabled but gi-extension missing')
        endif

        message('@0@ extensions not found, not building documentation requiring it'.format(extension))
        subdir_done()
    endif
endforeach

sitemap = ''
subdir('plugins')

TODO_doc = hotdoc.generate_doc('gstTODO',
    project_version: project_version,
    sitemap: 'sitemap.txt',
    index: 'index.md',
    gi_index: 'index.md',
    install: true,
    dependencies:[plugins_doc_dep, plugins_doc],
    syntax_highlighting_activate: true,
    disable_incremental_build: true,
    gst_list_plugins_page: 'gst-index',
    devhelp_activate: true,
    build_always: true,
    build_by_default: true,
)

