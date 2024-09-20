---
layout: post
title: Javascript Itunes API
description: Play some songs!
permalink: /itunesapi/
---
<!-- Dropdown Menu -->
<div class="dropdown">
    <button class="dropbtn">Go to other playground pages!</button>
    <div class="dropdown-content">
        <a href="{{site.baseurl}}/cookieclicker/">Cookie Clicker</a>
        <a href="{{site.baseurl}}/calculator/">Calculator</a>
        <a href="{{site.baseurl}}/snakegame/">Snake Game</a>
        <a href="{{site.baseurl}}/jsprojectplayground/">JS Playground Home</a>
    </div>
</div>

<style>
/* Container for the dropdown */
.dropdown {
    position: relative;
    display: inline-block;
}

/* Button to open the dropdown */
.dropbtn {
    background-color: #ffb4d2;
    color: white;
    padding: 10px 20px;
    font-size: 16px;
    border: none;
    cursor: pointer;
}

/* Dropdown content (hidden by default) */
.dropdown-content {
    display: none;
    position: absolute;
    background-color: #f1f1f1;
    min-width: 160px;
    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
    z-index: 1;
}

/* Links inside the dropdown */
.dropdown-content a {
    color: black;
    padding: 12px 16px;
    text-decoration: none;
    display: block;
}

/* Change color of dropdown links on hover */
.dropdown-content a:hover {
    background-color: #ddd;
}

/* Show the dropdown content on hover */
.dropdown:hover .dropdown-content {
    display: block;
}

/* Change the background color of the button when hovering */
.dropdown:hover .dropbtn {
    background-color: #c9184a;
}
</style>

<!-- Input box and button for filter -->
<div>
  <input type="text" id="filterInput" placeholder="Enter iTunes filter">
  <button onclick="fetchData()">Search</button>
</div>

<!-- HTML table fragment for page -->
<table>
  <thead>
    <tr>
      <th>Artist</th>
      <th>Track</th>
      <th>Images</th>
      <th>Preview</th>
    </tr>
  </thead>
  <tbody id="result">
    <!-- generated rows -->
  </tbody>
</table>

<!-- Script is laid out in a sequence (no function) and will execute when the page is loaded -->
<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");

  // function to fetch data based on user input
  function fetchData() {
    // clear previous results
    resultContainer.innerHTML = "";

    // get user input
    const filterInput = document.getElementById("filterInput");
    const filter = filterInput.value;

    // prepare fetch options
    const url = "https://itunes.apple.com/search?term=" + encodeURIComponent(filter);
    const headers = {
      method: 'GET',
      mode: 'cors',
      cache: 'default',
      credentials: 'omit',
      headers: {
        'Content-Type': 'application/json'
      },
    };

    // fetch the API
    fetch(url, headers)
      .then(response => {
        // check for response errors
        if (response.status !== 200) {
          const errorMsg = 'Database response error: ' + response.status;
          console.log(errorMsg);
          const tr = document.createElement("tr");
          const td = document.createElement("td");
          td.innerHTML = errorMsg;
          tr.appendChild(td);
          resultContainer.appendChild(tr);
          return;
        }
        // valid response will have JSON data
        response.json().then(data => {
          console.log(data);

          // Music data
        for (const row of data.results) {
            console.log(row);

            // tr for each row
            const tr = document.createElement("tr");
            // td for each column
            const artist = document.createElement("td");
            const track = document.createElement("td");
            const image = document.createElement("td");
            const preview = document.createElement("td");

            // data is specific to the API
            artist.innerHTML = row.artistName;
            track.innerHTML = row.trackName; 
            // create preview image
            const img = document.createElement("img");
            img.src = row.artworkUrl100;
            image.appendChild(img);
            // create preview player
            const audio = document.createElement("audio");
            audio.controls = true;
            const source = document.createElement("source");
            source.src = row.previewUrl;
            source.type = "audio/mp4";
            audio.appendChild(source);
            preview.appendChild(audio);

            // this builds td's into tr
            tr.appendChild(artist);
            tr.appendChild(track);
            tr.appendChild(image);
            tr.appendChild(preview);

            // add HTML to container
            resultContainer.appendChild(tr);
          }
        })
      })
      .catch(err => {
        console.error(err);
        const tr = document.createElement("tr");
        const td = document.createElement("td");
        td.innerHTML = err;
        tr.appendChild(td);
        resultContainer.appendChild(tr);
      });
  }
</script>
