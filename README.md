# MOVED TO [di2e - Stash](https://stash.di2e.net/projects/USACERMC/repos/MoD/browse)

# MOD v2

###Maps on Demand version 2

All font files will need to placed in your htdocs folder

For Google material fonts place the files noted in the main.css file in htdocs/modv2/fonts  
Download from here http://google.github.io/material-design-icons/

Add this to your Apache httpd.conf   
```
<Location /mod>
ProxyPass  http://localhost:3000
</Location>
<Location /api>
ProxyPass http://localhost:3002
</Location>
```



For IE9 support domify will need to be modified
```
// body support
if (tag == 'body') {
  var el = doc.createElement('html');
  el.innerHTML = html;
  return el.removeChild(el.lastChild);
}
```
Needs to be changed to
```
// body support
  if (tag == 'body') {
    var el = doc.createElement('html');

        // ie9 fix: if we just paste `html` in `el`, IE throws script600
        var bodyEl = doc.createElement('body');
       bodyEl.innerHTML = html;

       el.appendChild(bodyEl);
    return el.removeChild(el.lastChild);
  }

```
Headers need to be added to Jade templates
```
doctype html PUBLIC "-//W3C//DTD HTML 4.01//EN"
<!--[if IE 8]><html class="ie8" lang="en"><![endif]-->
<!--[if IE 9]><html lang="en" class="ie9"><![endif]-->
<!--[if !IE]><!-->
html(lang="en")
  <!--<![endif]-->
meta(http-equiv="X-UA-Compatible", content="IE=edge,chrome=1")
```

The router function will also need to handle IE9 differently

instead of
```
this.router.history.start({ pushState: true });
```
We have to first check for the browser's compatibility to use pushState
```
 if(!(window.history && history.pushState)) {
    this.router.history.start({ pushState: false, silent: true });
    var url = window.location.href;

    var fragment = url.split("/").slice(3).join("/");

      this.router.history.navigate(fragment, { trigger: true });
    }
    else {
      this.router.history.start({ pushState: true });
    }
```
<br/>
#TODO:
1.  [ ] Incorporate new style
2.  [x] Decouple api
3.  [ ] Fix identification tool
4.  [x] Add Apache config info

