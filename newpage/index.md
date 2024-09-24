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
    .then(response => {
      console.log("First run");
      return response.text();
      })
    .then(data => {
      console.log("Second part");
      console.log("Data is ", data);
      document.getElementById('green-content').innerHTML = data;
    })
    .catch(error => console.error('Error loading green_map.html:', error));
</script>