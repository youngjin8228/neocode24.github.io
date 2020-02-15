---
title: "Java LocalDateTime을 JSON 포멧으로 변경하는 최적의 방법"
date: 2020-02-15 09:00:00
tags: 
  - code
  - LocalDateTime
toc: true
---



Java8에서 새롭게 추가된 강력한 날짜 관련 클래스 LocalDate, LocalDateTime의 등장으로 Date 객체에 쌓여있던 수 많은 단점이 개선되었다. 그러나, 이런 개선은 일부분에서 또다른 불편함을 초래하였는데, 이는 JSON 으로 REST API를 통신함에 있어서 JSON Array Object로 전달 되고 이를 다시 Java에서 처리해야 하는 불편한 과정이 그러한 내용들이다.

Java8 이전에는 Calendar 클래스와 Joda Time 과 같은 또 다른 날짜 함수에 의존하여 다양한 날짜 계산의 오류나 한계를 극복해 왔으나, 이제는 완벽한 수준의 자유자제의 날짜 API의 등장으로 더이상 또 다른 날짜 클래스에 의존 하지 않고도 기능을 구현을 할 수 있게 되었다. 그러나, 이는 Java 시스템 내부에서의 이야기이고, 또 다른 시스템과 시간을 처리 할때는 기존과 동일한 변환이라는 문제점이 남아 있는다. (Remote Call 방식이 아닌, JSON, XML 등 방식일 경우)

물론, 이와 같은 과정은 기존의 Date 클래스를 사용하던 시절에도 존재하였고, 이에 대한 전통적인 방법으로 시간을 문자열로 보내고 받으며 처리자의 언어 방식으로 변환 하는 것이 관례였다.

하지만, 이제는 REST API를 통한 시스템간의 서비스 호출이 손쉽게 이뤄지고 있고, 다양한 Front-End 기술의 발전으로 상호간에 주고받는 방식에 표준날짜 포멧을 선호하는 추세에 따라, 시간 문자열을 ISO 표준[^1] 방식으로 기록하고 이를 Java에서 손쉽게 처리 하는 방식에 대해 Java Code와 함께 손쉬운 방법을 제시 하고자 한다.
[^1]: https://en.wikipedia.org/wiki/ISO_8601



### LocalDateTime 을 JSON 으로 보낼 때 현상 확인

GitHub Flavored Markdown [fenced code blocks](https://help.github.com/articles/creating-and-highlighting-code-blocks/) are supported. To modify styling and highlight colors edit `/_sass/syntax.scss`.

```css
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
```

{% highlight scss %}
.highlight {
  margin: 0;
  padding: 1em;
  font-family: $monospace;
  font-size: $type-size-7;
  line-height: 1.8;
}
{% endhighlight %}

```html
{% raw %}<nav class="pagination" role="navigation">
  {% if page.previous %}
    <a href="{{ site.url }}{{ page.previous.url }}" class="btn" title="{{ page.previous.title }}">Previous article</a>
  {% endif %}
  {% if page.next %}
    <a href="{{ site.url }}{{ page.next.url }}" class="btn" title="{{ page.next.title }}">Next article</a>
  {% endif %}
</nav><!-- /.pagination -->{% endraw %}
```

```ruby
module Jekyll
  class TagIndex < Page
    def initialize(site, base, dir, tag)
      @site = site
      @base = base
      @dir = dir
      @name = 'index.html'
      self.process(@name)
      self.read_yaml(File.join(base, '_layouts'), 'tag_index.html')
      self.data['tag'] = tag
      tag_title_prefix = site.config['tag_title_prefix'] || 'Tagged: '
      tag_title_suffix = site.config['tag_title_suffix'] || '&#8211;'
      self.data['title'] = "#{tag_title_prefix}#{tag}"
      self.data['description'] = "An archive of posts tagged #{tag}."
    end
  end
end
```

### ISO Date Time

Indentation matters. Be sure the indent of the code block aligns with the first non-space character after the list item marker (e.g., `1.`). Usually this will mean indenting 3 spaces instead of 4.

1. Do step 1.
2. Now do this:
  
   ```ruby
   def print_hi(name)
     puts "Hi, #{name}"
   end
   print_hi('Tom')
   #=> prints 'Hi, Tom' to STDOUT.
   ```
   
3. Now you can do this.

### Serializer, Desirializer

An example of a code blocking using Jekyll's [`{% raw %}{% highlight %}{% endraw %}` tag](https://jekyllrb.com/docs/templates/#code-snippet-highlighting).

{% highlight javascript linenos %}
// 'gulp html' -- does nothing
// 'gulp html --prod' -- minifies and gzips HTML files for production
gulp.task('html', () => {
  return gulp.src(paths.siteFolderName + paths.htmlPattern)
    .pipe(when(argv.prod, htmlmin({
      removeComments: true,
      collapseWhitespace: true,
      collapseBooleanAttributes: false,
      removeAttributeQuotes: false,
      removeRedundantAttributes: false,
      minifyJS: true,
      minifyCSS: true
    })))
    .pipe(when(argv.prod, size({title: 'optimized HTML'})))
    .pipe(when(argv.prod, gulp.dest(paths.siteFolderName)))
    .pipe(when(argv.prod, gzip({append: true})))
    .pipe(when(argv.prod, size({
      title: 'gzipped HTML',
      gzip: true
    })))
    .pipe(when(argv.prod, gulp.dest(paths.siteFolderName)))
});
{% endhighlight %}

{% highlight wl linenos %}
Module[{},
  Sqrt[2]
  4
]
{% endhighlight %}

### Test

An example of a Gist embed below.

<script src="https://gist.github.com/mmistakes/77c68fbb07731a456805a7b473f47841.js"></script>



### 결론