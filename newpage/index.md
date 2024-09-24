# Testing creating a new page

<!-- Include HTML content directly -->
<div>
  <h1>Green Page</h1>
  <p>This is the content of the green.html file.</p>
</div>

<div id="green-content"></div>

<script>
  console.log("Runing script")
  fetch('green_map.html')
    .then(response => response.text())
    .then(data => {
      document.getElementById('green-content').innerHTML = data;
    })
    .catch(error => console.error('Error loading green_map.html:', error));
  console.log("Script finished")
</script>