---
layout: default
---


<h1 class="text-center">licensekit</h1>

<div class="container align-items-center justify-content-center">
    <form id="form">
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
    <p class="text-center">(Please right-click and "Save As" on each image)</p>
    <div id="results-images"></div>
</div>

<div id="dev-area">

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
    $("#dev-area").hide();
    return false;
  }
  
  $("#results-area").show();
  $("#results-images").empty();

  $("#dev-area").show();

  var scale = $("#scale").val();

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

  $("#dev-area").hide();
}

$("form").submit(function(){
  mainFunction(false);

  return false;
});

$(document).ready(function() {
  mainFunction(true);
});
</script>

<script type="text/javascript" src="static/script.js"></script>
