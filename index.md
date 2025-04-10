---
layout: base
title: Abby Home  
description: Home Page
image: /images/mario_animation.png
hide: true
---

<!-- Liquid:  statements -->

<!-- Include submenu from _includes to top of pages -->
{% include nav/home.html %}
<!--- Concatenation of site URL to frontmatter image  --->
{% assign sprite_file = site.baseurl | append: page.image %}
<!--- Has is a list variable containing mario metadata for sprite --->
{% assign hash = site.data.mario_metadata %}  
<!--- Size width/height of Sprit images --->
{% assign pixels = 256 %} 

<!--- HTML for page contains <p> tag named "Mario" and class properties for a "sprite"  -->

<p id="mario" class="sprite"></p>
  
<!--- Embedded Cascading Style Sheet (CSS) rules, 
        define how HTML elements look 
--->
<style>

  /*CSS style rules for the id and class of the sprite...
  */
  .sprite {
    height: {{pixels}}px;
    width: {{pixels}}px;
    background-image: url('{{sprite_file}}');
    background-repeat: no-repeat;
  }

  /*background position of sprite element
  */
  #mario {
    background-position: calc({{animations[0].col}} * {{pixels}} * -1px) calc({{animations[0].row}} * {{pixels}}* -1px);
  }
</style>

<!--- Embedded executable code--->
<script>
  ////////// convert YML hash to javascript key:value objects /////////

  var mario_metadata = {}; //key, value object
  {% for key in hash %}  
  
  var key = "{{key | first}}"  //key
  var values = {} //values object
  values["row"] = {{key.row}}
  values["col"] = {{key.col}}
  values["frames"] = {{key.frames}}
  mario_metadata[key] = values; //key with values added

  {% endfor %}

  ////////// game object for player /////////

  class Mario {
    constructor(meta_data) {
      this.tID = null;  //capture setInterval() task ID
      this.positionX = 0;  // current position of sprite in X direction
      this.currentSpeed = 0;
      this.marioElement = document.getElementById("mario"); //HTML element of sprite
      this.pixels = {{pixels}}; //pixel offset of images in the sprite, set by liquid constant
      this.interval = 100; //animation time interval
      this.obj = meta_data;
      this.marioElement.style.position = "absolute";
    }

    animate(obj, speed) {
      let frame = 0;
      const row = obj.row * this.pixels;
      this.currentSpeed = speed;

      this.tID = setInterval(() => {
        const col = (frame + obj.col) * this.pixels;
        this.marioElement.style.backgroundPosition = `-${col}px -${row}px`;
        this.marioElement.style.left = `${this.positionX}px`;

        this.positionX += speed;
        frame = (frame + 1) % obj.frames;

        const viewportWidth = window.innerWidth;
        if (this.positionX > viewportWidth - this.pixels) {
          document.documentElement.scrollLeft = this.positionX - viewportWidth + this.pixels;
        }
      }, this.interval);
    }

    startWalking() {
      this.stopAnimate();
      this.animate(this.obj["Walk"], 3);
    }
    startWalkingL() { // added
       this.stopAnimate();
        this.animate(this.obj["WalkL"]-3);
    }
    startRunning() {
      this.stopAnimate();
      this.animate(this.obj["Run1"], 6);
    }
    startRunningL() { //added
      this.stopAnimate();
      this.animate(this.obj["Run1L"], -6);
    }
    startPuffing() {
      this.stopAnimate();
      this.animate(this.obj["Puff"], 0);
    }

    startCheering() {
      this.stopAnimate();
      this.animate(this.obj["Cheer"], 0);
    }

    startFlipping() {
      this.stopAnimate();
      this.animate(this.obj["Flip"], 0);
    }

    startResting() {
      this.stopAnimate();
      this.animate(this.obj["Rest"], 0);
    }

    stopAnimate() {
      clearInterval(this.tID);
    }
  }

  const mario = new Mario(mario_metadata);

  ////////// event control /////////

  window.addEventListener("keydown", (event) => {
  if (event.key === "ArrowRight") {
    event.preventDefault();
    if (event.repeat) {
      mario.startCheering();
    } else {
      if (mario.currentSpeed === 0) {
        mario.startWalking();
      } else if (mario.currentSpeed === 3) {
        mario.startRunning();
      }
    }
  } else if (event.key === "ArrowDown") { // changed from arrowleft to arrowdown
    event.preventDefault();
    if (event.repeat) {
      mario.stopAnimate();
    } else {
      mario.startPuffing();
    }
  } else if (event.key === "ArrowLeft") { // created new actions for arrow left for walking/running L
    event.preventDefault();
    if (event.repeat) {
      mario.startWalkingL(); // Example action for holding down the key
    } else {
      mario.startRunningL(); // Replace this with the actual function to walk left
    }
  }
});

  //touch events that enable animations
  window.addEventListener("touchstart", (event) => {
    event.preventDefault(); // prevent default browser action
    if (event.touches[0].clientX > window.innerWidth / 2) {
      // move right
      if (currentSpeed === 0) { // if at rest, go to walking
        mario.startWalking();
      } else if (currentSpeed === 3) { // if walking, go to running
        mario.startRunning();
      }
    } else {
      // move left
      mario.startPuffing();
    }
  });

  //stop animation on window blur
  window.addEventListener("blur", () => {
    mario.stopAnimate();
  });

  //start animation on window focus
  window.addEventListener("focus", () => {
     mario.startFlipping();
  });

  //start animation on page load or page refresh
  document.addEventListener("DOMContentLoaded", () => {
    // adjust sprite size for high pixel density devices
    const scale = window.devicePixelRatio;
    const sprite = document.querySelector(".sprite");
    sprite.style.transform = `scale(${0.2 * scale})`;
    mario.startResting();
  });

</script>
<div style="text-align: center; color: #512e5f; font-size: 250%">
Welcome to my Github Pages! 
</div>

<!-- Navigation buttons section -->
<div style="display: flex; justify-content: center; gap: 30px; padding: 40px; border-radius: 20px; box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.2);">

<div style="text-align: center;">
   <a href="tri2_blogs" style="text-decoration: none;">
      <button style="background-color: #d7bde2; color: white; border: none; padding: 20px 50px; font-size: 18px; border-radius: 50px; cursor: pointer; transition: background-color 0.3s, transform 0.3s;">
         Trimester 2 Blogs
      </button>
   </a>
</div>

<div style="text-align: center;">
   <a href="cpt_project" style="text-decoration: none;">
      <button style="background-color: #d7bde2; color: white; border: none; padding: 20px 50px; font-size: 18px; border-radius: 50px; cursor: pointer; transition: background-color 0.3s, transform 0.3s;">
         Create Performance Task 
      </button>
   </a>
</div>


<div style="text-align: center; font-size: 150%">
   <a href="{{site.baseurl}}/jsprojectplayground/" style="text-decoration: none;">
      <button style="background-color: #d7bde2; color: white; border: none; padding: 20px 50px; font-size: 18px; border-radius: 50px; cursor: pointer; transition: background-color 0.3s, transform 0.3s;">
         Javascript Project Playground
      </button>
   </a>
</div>
</div>


<div style="text-align: center; font-size: 150%">
Click on the buttons above to see some of my work! 😀

</div>

<div style="text-align: center;">
<img src="images/carouselindex/coppeliapic.jpg" alt="Coppelia" width="400" height="300">
</div>

<!-- Swap Links Button -->
<div class="button-container">
    <button id="swapButton" class="swap-button">Switch!</button>
</div>
<div class="button-container">
    <a id="button1" class="button" href="https://open.spotify.com/">Spotify</a>
    <a id="button2" class="button" href="https://music.apple.com/us/new">Apple Music</a>
</div>

<style>
.button-container {
    display: flex;
    justify-content: center;
    margin: 20px;
}

.button {
    padding: 10px 20px;
    background-color: #dad7cd;
    color: white;
    border: none;
    cursor: pointer;
    margin: 0 10px;
    text-decoration: none; /* Remove underline from links */
    display: inline-block; /* Make links behave like buttons */
}

.button:hover {
    background-color: #a3b18a;
}

.swap-button {
    background-color: #588157; /* Different color for the swap button */
    color: white;
    padding: 10px 20px;
    border: none;
    cursor: pointer;
    margin: 0 10px;
}

.swap-button:hover {
    background-color: #3a5a40; /* Darker shade on hover */
}
</style>

<script>
document.getElementById('swapButton').onclick = function() {
    const button1 = document.getElementById('button1');
    const button2 = document.getElementById('button2');
    
    // Swap the text and href of the buttons
    const tempText = button1.innerHTML;
    const tempHref = button1.href;
    
    button1.innerHTML = button2.innerHTML;
    button1.href = button2.href;

    button2.innerHTML = tempText;
    button2.href = tempHref;
};
</script>



<script src="https://utteranc.es/client.js"
        repo="abbymanalo/abby2025"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>