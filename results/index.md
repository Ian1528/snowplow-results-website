# Testing creating a new page

<!-- Include HTML content directly -->
<div>
  <h1>Green Page</h1>
  <p>This is the content of the green.html file.</p>
</div>

<div id="green-content"></div>

<script>
  console.log("Running script");
  fetch('green_map.html')
    .then(response => response.text())
    .then(data => {
      const container = document.getElementById('green-content');
      container.innerHTML = data;

      // Extract and evaluate scripts
      const scripts = container.getElementsByTagName('script');
      for (let i = 0; i < scripts.length; i++) {
        const script = document.createElement('script');
        script.text = scripts[i].text;
        document.body.appendChild(script).parentNode.removeChild(script);
      }
    })
    .catch(error => console.error('Error loading green_map.html:', error));
</script>