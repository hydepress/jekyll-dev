---
title: Custom Markdown Processors
---

## Custom Markdown Processors

If you're interested in creating a custom markdown processor, you're in luck! Create a new class in the `Jekyll::Converters::Markdown` namespace:

{% highlight ruby %}
class Jekyll::Converters::Markdown::MyCustomProcessor
  def initialize(config)
    require 'funky_markdown'
    @config = config
  rescue LoadError
    STDERR.puts 'You are missing a library required for Markdown. Please run:'
    STDERR.puts '  $ [sudo] gem install funky_markdown'
    raise FatalException.new("Missing dependency: funky_markdown")
  end

  def convert(content)
    ::FunkyMarkdown.new(content).convert
  end
end
{% endhighlight %}

Once you've created your class and have it properly set up either as a plugin
in the `_plugins` folder or as a gem, specify it in your `_config.yml`:

{% highlight yaml %}
markdown: MyCustomProcessor
{% endhighlight %}
