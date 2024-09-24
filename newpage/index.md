# Testing creating a new page

<div id="green-content"></div>

<script>
  fetch('green_map.html')
    .then(response => response.text())
    .then(data => {
      document.getElementById('green-content').innerHTML = data;
    })
    .catch(error => console.error('Error loading green_map.html:', error));
</script>