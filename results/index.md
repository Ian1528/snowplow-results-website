# Testing creating a new page

<!-- Include HTML content directly -->
<div>
  <h1>Green Page</h1>
  <p>This is the content of the green.html file.</p>
</div>

<div id="green-content"></div>

<!-- Embed green_map.html as an iframe -->
<iframe src="green_map.html" width="100%" height="500px" style="border:none;"></iframe>

<script>
  fetch('green_map.html')
    .then(response => response.text())
    .then(data => {
      const container = document.getElementById('green-content');
      container.innerHTML = data;
      console.log("EVAL")
      // Extract and evaluate scripts
      const scripts = container.getElementsByTagName('script');
      for (let i = 0; i < scripts.length; i++) {
        const script = document.createElement('script');
        script.text = scripts[i].text;
        eval(scripts[i].text);
        console.log("evaluating");
        document.body.appendChild(script).parentNode.removeChild(script);
      }
    })
    .catch(error => console.error('Error loading green_map.html:', error));
</script>