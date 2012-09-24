Template
====================

from django.template import Template, Context:

you can `rander()` Context to Template.

There many kinds of template class:

+ Template
+ Context

RequestContext: difference with Context

1. the first parameter is pass in request
2. It automatically  populates a few context, according to `TEMPLATE_CONTEXT_PROCESSORS` setting

so, we need to avoid using default variables

Also, we can threw in a list of additional processors,for example

    p_address_processor(request):
        return {'ip_address': request.META['REMOTE_ADDR']}

    def some_view(request):
        c = RequestContext(request, {
            'foo': 'bar',
        }, [ip_address_processor])
        return HttpResponse(t.render(c))

Loading template
--------------------

### TEMPLATE_DIRS (in setting.py)

setting you template director in `setting.py` , for example:

    TEMPLATE_DIRS = (
        "/home/html/templates/lawrence.com",
        "/home/html/templates/default",
    )

**Note: these paths should be Unix-style forward slashes**

The python API
--------------------

    django.template.loader.get_template(template_name)

return a compiled template

    django.template.loader.selec_template(template_list_name)

Action like `get_template` but you can pass a list of template

Using subdirector
--------------------

It's possible! It's good for module and seperate your templates in folder.

    get_template('news/story_detail.html')

The render_to_string shortcut
--------------------

    django.template.loader.render_to_string(template_name, dictionary=None, context_instance=None)

Django provide a shortcut function, with

1. load a template
2. render it
3. return a  string

for example

    from django.template.loader import render_to_string
    rendered = render_to_string('my_template.html', { 'foo': 'bar' })

.
