```
module ViewHelpers
  def get_relative_path()
    slashCount = request.path.split("/").length - 2
    pathAdd = ""
    for i in 0..slashCount
      pathAdd +="../"
    end
    return pathAdd
  end

  def a_href(href)
    pathAdd = get_relative_path()

    newHref = pathAdd + href

    return newHref
  end

  def image_tag(src, html_options = {})
    pathAdd = get_relative_path()

    src = "#{pathAdd}#{Compass.configuration.images_dir}/#{src}" unless src =~ /^(https?:|\/)/

    tag(:img, html_options.merge({:src => src}))
  end

  def bg_image_path(src)
    pathAdd = get_relative_path()

    src = "#{pathAdd}#{Compass.configuration.images_dir}/#{src}" unless src =~ /^(https?:|\/)/

    return src
  end

  def stylesheet_link_tag(*sources)
    pathAdd = get_relative_path()

    options = extract_options!(sources)

    base_folder = if defined? Compass
                    Compass.configuration.css_dir
                  else
                    'stylesheets'
                  end
    sources.map do |source|
      path = ensure_path(ensure_extension(source, 'css'), base_folder)
      path[0, 1] = ''
      path = pathAdd + path
      tag('link', {
          'rel' => 'stylesheet',
          'type' => 'text/css',
          'media' => 'screen',
          'href' => path
      }.with_indifferent_access.merge(options))
    end.join("\n")
  end


  def javascript_include_tag(*sources)
    path = ""
    pathAdd = get_relative_path()

    options = extract_options!(sources)

    base_folder = if defined? Compass
                    Compass.configuration.javascripts_dir
                  else
                    'javascripts'
                  end
    sources.map do |source|
      path = ensure_path(ensure_extension(source, 'js'), base_folder)
      path[0, 1] = ''
      path = pathAdd + path

      content_tag('script', '', {
          'type' => 'text/javascript',
          'src' => path
      }.with_indifferent_access.merge(options))
    end.join("\n")
  end
end
```
