---
layout: default
---

<div class="container align-items-center justify-content-center">

    <h1 class="text-center">licensekit</h1>
    
    <div class="text-center">
        <a class="btn btn-secondary" role="button"
            href="https://github.com/createdelic/licensekit">
<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-github" viewBox="0 0 16 16">
  <path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.012 8.012 0 0 0 16 8c0-4.42-3.58-8-8-8z"/>
</svg>
          GitHub
        </a>
        <a class="btn btn-secondary" role="button"
            href="mailto:contact@createdelic.com">
<svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-envelope-fill" viewBox="0 0 16 16">
  <path d="M.05 3.555A2 2 0 0 1 2 2h12a2 2 0 0 1 1.95 1.555L8 8.414.05 3.555ZM0 4.697v7.104l5.803-3.558L0 4.697ZM6.761 8.83l-6.57 4.027A2 2 0 0 0 2 14h12a2 2 0 0 0 1.808-1.144l-6.57-4.027L8 9.586l-1.239-.757Zm3.436-.586L16 11.801V4.697l-5.803 3.546Z"/>
</svg>
        Contact
        </a>
    </div>
    
    <p class="text-center mt-2">
    License data last updated on {{ site.time | date: "%Y-%m-%d %H:%M:%S" }}
    </p>

    <form id="form" class="mt-2">
        {% capture w %}{{site.defaultvalues.width}}{% endcapture %}
        {% include form_row.html name='Width' id='w' value=w units="px" %}

        {% capture h %}{{site.defaultvalues.height}}{% endcapture %}
        {% include form_row.html name='Height' id='h' value=h units="px" %}
        
        {% capture margin %}{{site.defaultvalues.margin}}{% endcapture %}
        {% include form_row.html name='Margin' id='margin' value=margin units="px" %}
        
        {% capture pages %}{{site.defaultvalues.pages}}{% endcapture %}
        {% include form_row.html name='Pages' id='pages' value=pages %}
        
        {% capture columns %}{{site.defaultvalues.columns}}{% endcapture %}
        {% include form_row.html name='Columns' id='columns' value=columns %}
        
        {% capture scale %}{{site.defaultvalues.scale}}{% endcapture %}
        {% include form_row.html name='Scale' id='scale' value=scale %}

        <div class="d-grid gap-2">
            <button type="submit" class="btn btn-primary btn-block">Create images</button>
        </div>
    </form>
</div>

<div id="results-area" class="m-1">
    <hr>
    <h2 class="text-center">Generated Images</h2>
<h5 class="text-center">
Resolution: <span id="res-w">1</span>x<span id="res-h">2</span>
</h5>
    <p class="text-center">(Please right-click and "Save As" on each image)</p>
    <div id="results-images"></div>
</div>

<div id="dev-area" class="mt-5">

    <h2 class="text-center">Canvas (ignore this area)</h2>

<div id="capture" class="licenses">

{% for license in site.data.licenses %}
<h2>
{{ license.name }}
</h2>
<pre>
{% capture x %}{% include licenses/{{ license.license }} %}{% endcapture %}
{{ x | escape }}
</pre>
{% if forloop.last == false %}
<hr>
{% endif %}
{% endfor %}

{% if site.data.sounds.size > 0 %}
<hr>
<h2>Sound Effect Attributions</h2>
<pre>
Below is a list of attributions for sounds which were used or referenced in the production of this software.
{% for sfx in site.data.sounds %}
{{sfx.name}}
- Author: {{sfx.author}}
- URL: {{sfx.url}}
- License: {{sfx.license}}{% if sfx.license-url != "" and sfx.license-url != nil %}
- License URL: {{sfx.license-url}}{% endif %}
{% endfor %}
</pre>
{% endif %}

{% if site.data.othermedia.size > 0 %}
<hr>
<h2>Other Media Attributions</h2>
<pre>{% for item in site.data.othermedia %}
{{item.name}}
- URL: {{item.url}}
- Author: {{item.author}}
- License: {{item.license}}{% if item.license-url != "" and item.license-url != nil %}
- License URL: {{item.license-url}}{% endif %}
{% endfor %}
</pre>
{% endif %}

{% if site.data.disclaimers.size > 0 %}
<hr>
{% for disclaimer in site.data.disclaimers %}
<pre>
{{disclaimer}}
</pre>
{% endfor %}
{% endif %}

</div>
</div>

<script
    src="https://code.jquery.com/jquery-3.6.0.min.js"
    integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4="
    crossorigin="anonymous"></script>
<script type="text/javascript" src="https://unpkg.com/html2canvas@1.0.0-rc.5/dist/html2canvas.js"></script>
<script>
function mainFunction(setuponly) {
  var w = $("#w").val();
  var h = $("#h").val();
  var pages = $("#pages").val();
  var columns = $("#columns").val();
  var margin = $("#margin").val();

  w -= margin * 2;
  h -= margin * 2;

  $("#capture").width(w*pages).height(h);
  $("#capture").css("column-count", pages*columns);

  if (setuponly) {
    $("#results-area").hide();
    return false;
  }
  
  $("#results-area").show();
  $("#results-images").empty();

  var scale = $("#scale").val();

  $("#res-w").text((w*scale) + '');
  $("#res-h").text((h*scale) + '');

  for (var i = 0; i < pages; i++) {
    var canvas = document.createElement('canvas');
    canvas.width = w*scale;
    canvas.height = h*scale;
    canvas.style.width = w + 'px';
    canvas.style.height = h + 'px';
    var context = canvas.getContext('2d');
    context.scale(scale,scale);

    var params = {
      canvas: canvas,
      scale: 1,
      x: i*w,
      backgroundColor: '#ffffff'
    };

    html2canvas(document.querySelector("#capture"), params).then(canvas => {
        $("#results-images").append(canvas);
    });
  }
}

$("form").submit(function(){
  mainFunction(false);

  return false;
});

$(document).ready(function() {
  mainFunction(true);
});
</script>
