---
title: Tags 'n' Filters
---

## Tags 'n' Filters

If you’d like to include custom liquid tags in your site, you can do so by
hooking into the tagging system. Built-in examples added by Jekyll include the
`highlight` and `include` tags. Below is an example of a custom liquid tag that
will output the time the page was rendered:

{% highlight ruby %}
module Jekyll
  class RenderTimeTag < Liquid::Tag

    def initialize(tag_name, text, tokens)
      super
      @text = text
    end

    def render(context)
      "#{@text} #{Time.now}"
    end
  end
end

Liquid::Template.register_tag('render_time', Jekyll::RenderTimeTag)
{% endhighlight %}

At a minimum, liquid tags must implement:

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Method</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <p><code>render</code></p>
      </td>
      <td>
        <p>Outputs the content of the tag.</p>
      </td>
    </tr>
  </tbody>
</table>
</div>

You must also register the custom tag with the Liquid template engine as
follows:

{% highlight ruby %}
Liquid::Template.register_tag('render_time', Jekyll::RenderTimeTag)
{% endhighlight %}

In the example above, we can place the following tag anywhere in one of our
pages:

{% highlight ruby %}
{% raw %}
<p>{% render_time page rendered at: %}</p>
{% endraw %}
{% endhighlight %}

And we would get something like this on the page:

{% highlight html %}
<p>page rendered at: Tue June 22 23:38:47 –0500 2010</p>
{% endhighlight %}

### Liquid filters

You can add your own filters to the Liquid template system much like you can
add tags above. Filters are simply modules that export their methods to liquid.
All methods will have to take at least one parameter which represents the input
of the filter. The return value will be the output of the filter.

{% highlight ruby %}
module Jekyll
  module AssetFilter
    def asset_url(input)
      "http://www.example.com/#{input}?#{Time.now.to_i}"
    end
  end
end

Liquid::Template.register_filter(Jekyll::AssetFilter)
{% endhighlight %}

<div class="note">
  <h5>ProTip™: Access the site object using Liquid</h5>
  <p>
    Jekyll lets you access the <code>site</code> object through the
    <code>context.registers</code> feature of Liquid at <code>context.registers[:site]</code>. For example, you can
    access the global configuration file <code>_config.yml</code> using
    <code>context.registers[:site].config</code>.
  </p>
</div>

