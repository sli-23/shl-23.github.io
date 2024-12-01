---
layout: about
title: ABOUT
permalink: /
subtitle:

profile:
  align: right
  image:
  image_circular: false # crops the image to make it circular
  more_info:

news: false # includes a list of news items
selected_papers: false # includes a list of papers marked as "selected={true}"
social: false # includes social icons at the bottom of the page
---

<!-- Add p5.js library -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>

<!-- Create a container that takes full width -->
<div id="sketch-holder" style="width: 100%; margin: 20px 0;"></div>

<!-- Your eyes sketch -->
<script>
let symbols = ['/', '&', '*', '%', '#', '@', '~', '+', '-', '='];
let fallingSymbols = [];

function setup() {
  let parentWidth = document.getElementById('sketch-holder').offsetWidth;
  let canvas = createCanvas(parentWidth, 400);
  canvas.parent('sketch-holder');
  colorMode(HSB);
  angleMode(DEGREES);
  describe('Two eyes that follow the cursor with falling symbols.');
  
  // Reduced number of symbols from 20 to 8
  for (let i = 0; i < 8; i++) {
    fallingSymbols.push({
      x: random(width),
      y: random(-100, 0),
      speed: random(0.8, 2),
      symbol: random(symbols),
      size: random(15, 30)
    });
  }
}

// Add windowResized function to handle container width changes
function windowResized() {
  let parentWidth = document.getElementById('sketch-holder').offsetWidth;
  resizeCanvas(parentWidth, 400);
}

function draw() {
  background(15);
  stroke(15);

  // Update and draw falling symbols
  for (let i = 0; i < fallingSymbols.length; i++) {
    let s = fallingSymbols[i];
    
    // Draw symbol with orange color
    noStroke();
    fill('#f39304');
    textSize(s.size);
    textAlign(CENTER, CENTER);
    text(s.symbol, s.x, s.y);
    
    // Update position
    s.y += s.speed;
    
    // Reset if symbol goes off screen
    if (s.y > height) {
      s.y = random(-100, 0);
      s.x = random(width);
      s.symbol = random(symbols);
      s.speed = random(0.8, 2);
    }
  }

  // Center the eyes in the canvas
  let spacing = 100;  // Space between eyes
  let centerX = width/2;
  
  // Draw left eye
  let leftX = centerX - spacing;
  let leftY = 200;
  let leftAngle = atan2(mouseY - leftY, mouseX - leftX);

  push();
  translate(leftX, leftY);
  fill('#F5F5F5');
  ellipse(0, 0, 90, 90);
  rotate(leftAngle);
  fill(15);
  ellipse(12.5, 0, 30, 30);
  pop();

  // Draw right eye
  let rightX = centerX + spacing;
  let rightY = 200;
  let rightAngle = atan2(mouseY - rightY, mouseX - rightX);

  push();
  translate(rightX, rightY);
  fill('#F5F5F5');
  ellipse(0, 0, 90, 90);
  rotate(rightAngle);
  fill(15);
  ellipse(12.5, 0, 30, 30);
  pop();

  // Add line below eyes
  strokeWeight(30);
  stroke('#F5F5F5');
  line(centerX - spacing - 25, leftY + 80, centerX + spacing + 25, rightY + 80);

  strokeWeight(30);
  stroke('#F5F5F5');
  line(centerX - spacing - 25, leftY + 95, centerX + spacing + 25, rightY + 95);

  strokeWeight(5);
  stroke(15);
  line(centerX - spacing - 50, leftY + 88, centerX + spacing + 50, rightY + 88);
}
</script>

_<span style="color: orange; font-weight: bold;"> > code ref:</span> https://p5js.org/examples/angles-and-motion-aim/_

<div class="row justify-content-sm-center">
  <div class="col-sm-4 mt-3 mt-md-0">
    <style>
      .grayscale-effect {
        filter: grayscale(100%);
        transition: filter 0.3s ease;
      }
      .grayscale-effect:hover {
        filter: grayscale(0%);
      }
    </style>
    {% include figure.liquid path="assets/img/ai-generated-figure.png" title="ai-generated figure" class="img-fluid grayscale-effect" %}
    
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    <!-- Add container for the sketch -->
    <div id="square-sketch-holder" style="width: 100%; margin: 20px 0;"></div>

    <!-- Square animation sketch -->
    <script>
      let squareSketch = function(p) {
        let squareSize, rNoise;
        let segments = 50;
        let txt = "hello world!!";
        let textColor;

        p.setup = function() {
          let container = document.getElementById('square-sketch-holder');
          let canvas = p.createCanvas(container.offsetWidth, container.offsetWidth);
          canvas.parent('square-sketch-holder');

          squareSize = container.offsetWidth * 0.8;
          rNoise = squareSize * 0.067;

          p.textFont('Helvetica');
          p.textSize(squareSize * 0.167);
          textColor = p.color('#f39304');
        };

        p.draw = function() {
          p.background('#F5F5F5');
          p.fill('#262626');
          p.noStroke();

          let centreX = p.width / 2;
          let centreY = p.height / 2;

          let halfSize = squareSize / 2;
          let corners = [
            { x: centreX - halfSize, y: centreY - halfSize },
            { x: centreX + halfSize, y: centreY - halfSize },
            { x: centreX + halfSize, y: centreY + halfSize },
            { x: centreX - halfSize, y: centreY + halfSize }
          ];

          p.beginShape();
          for (let i = 0; i < 4; i++) {
            let next = (i + 1) % 4;
            let segmentLength = p.dist(corners[i].x, corners[i].y, corners[next].x, corners[next].y);

            for (let j = 0; j <= segments; j++) {
              let t = j / segments;
              let x = p.lerp(corners[i].x, corners[next].x, t);
              let y = p.lerp(corners[i].y, corners[next].y, t);

              let noiseVal = p.noise(x / 100, y / 100, p.frameCount / 100);
              let nx = x + p.map(noiseVal, 0, 1, -rNoise, rNoise);
              let ny = y + p.map(noiseVal, 0, 1, -rNoise, rNoise);
              p.vertex(nx, ny);
            }
          }
          p.endShape(p.CLOSE);

          let txtPos = 0;
          let totalPerimeter = squareSize * 4;

          for (let j = 0; j < txt.length; j++) {
            let charWidth = p.textWidth(txt.charAt(j));
            let segmentStart = txtPos / totalPerimeter * 4;

            let side = Math.floor(segmentStart);
            let t = (segmentStart - side) + charWidth / totalPerimeter;

            let start = corners[side];
            let end = corners[(side + 1) % 4];

            let x = p.lerp(start.x, end.x, t);
            let y = p.lerp(start.y, end.y, t);

            let noiseVal = p.noise(x / 100, y / 100, p.frameCount / 100);
            let nx = x + p.map(noiseVal, 0, 1, -rNoise, rNoise);
            let ny = y + p.map(noiseVal, 0, 1, -rNoise, rNoise);

            let angle = p.atan2(end.y - start.y, end.x - start.x);

            p.push();
            p.translate(nx, ny);
            p.rotate(angle);
            p.fill(textColor);
            p.text(txt.charAt(j), -charWidth / 2, 0);
            p.pop();

            txtPos += charWidth;
          }
        };

        p.windowResized = function() {
          let container = document.getElementById('square-sketch-holder');
          p.resizeCanvas(container.offsetWidth, container.offsetWidth);
          squareSize = container.offsetWidth * 0.8;
          rNoise = squareSize * 0.067;
          p.textSize(squareSize * 0.167);
        };
      };

      new p5(squareSketch);
    </script>

  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    <div style="text-align: left; margin-top: -10px;" class="responsive-text">
      <span style="font-size: 3em;" class="text-1">人类生而</span>
      <span style="color: #f39304; display: block; margin-top: -40px; font-size: 4.5em; font-weight: bold;" class="text-2">自由 #!!</span>
      <span style="font-size: 1.2em; display: block; margin-top: -10px; margin-left: 10px;" class="text-3">
        <i class="fa-solid fa-barcode" style="font-size: 1.5em; vertical-align: middle;"></i>
        <i class="fa-solid fa-barcode" style="font-size: 1.5em; vertical-align: middle; transform: scaleX(-1);"></i>
        <span style="vertical-align: middle;">人类有權享有生命</span>
      </span>
      <span style="font-size: 3.8em;display: block; margin-top: -8px; position: relative;" class="text-4">
        生命燃烧<button style="color: #f39304;font-size: 0.3em; position: absolute; background: transparent; border: none; cursor: pointer; letter-spacing: 2px; font-weight: bold; font-family: 'Roboto Mono', monospace; transition: background 0.3s ease; border-radius: 4px; padding: 1px 1px;" onmouseover="this.style.background='rgba(243, 147, 4, 0.2)'" onmouseout="this.style.background='transparent'" onclick="window.open('https://zh.moegirl.org.cn/zh-hans/%E6%88%91%E5%B7%B2%E7%BB%8F%E7%87%83%E7%83%A7%E6%AE%86%E5%B0%BD%E4%BA%86%EF%BC%8C%E5%8F%AA%E5%89%A9%E4%B8%8B%E9%9B%AA%E7%99%BD%E7%9A%84%E7%81%B0', '_blank')">[1]</button>
      </span>
      <span style="color: #f39304; font-size: 4.5em; display: block; margin-left: 20px; margin-top: -45px; font-weight: bold; position: relative; z-index: 2;" class="text-5">殆尽_。</span>
    </div>

    <style>
      @media screen and (min-width: 768px) and (max-width: 870px) {
        .responsive-text .text-1 { font-size: 2.4em !important; }
        .responsive-text .text-2 {
          font-size: 3.6em !important;
          margin-top: -32px !important;
        }
        .responsive-text .text-3 { font-size: 1em !important; }
        .responsive-text .text-4 {
          font-size: 3em !important;
          margin-top: -6px !important;
        }
        .responsive-text .text-5 {
          font-size: 3.5em !important;
          margin-top: -35px !important;
        }
      }
    </style>

  </div>
</div>
