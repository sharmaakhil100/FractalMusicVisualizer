/*
Akhil Sharma
3 July 2019
Description: Fractal based transformations/animations 
that respond to music.
Music: Bullseye (KDrew)
*/

import codeanticode.syphon.*; // Syphon Server
import processing.sound.*; // 
SoundFile file;
Amplitude amp;
SyphonServer server;
int counter = 0;
float ca = -.4; //+ cos(angle*1.69);
float cb = -.59; //+ sin(angle);
float norm = 0;
//float angle = 1.15; // old method of changing ca & cb
//float angle_const = angle;
float w = 5;
float h = (w * height) / width;
//int seconds = 0;
boolean zoomIn = false;
int initialTime;
int interval = 1000;
int time;

void setup() {
  size(400, 400, P2D);
  server = new SyphonServer(this, "Processing Syphon");
  colorMode(HSB, 100); // 100
  initialTime = millis();
  w -= 7; // initial "zoom" for fractal image
  h -= 7;
  file = new SoundFile(this, "bullseyeKdrew.mp3"); // Dubstep
  file.amp(0.1); // sets max amplitude for song to be .1 
  amp = new Amplitude(this);
  amp.input(file);
  file.play();
}

float huePicker(int numiterations) {
  // selects a hue for HGB mode/ color
  // n is num of iterations before passing escape radius
  // selects a hue with an algorithm depending 
  // on the value of norm before passing the
  // escape radius
  float hue = numiterations - (log (log (sqrt(norm))));
  //print(numiterations);
  //print("    ");
  float iterMapped = map(numiterations, 1, 5, 55, 95);
  if (hue < 3) {
    hue += iterMapped;
  }
  return hue;
}

void draw() {
  float vol = amp.analyze(); // volume/amplitude from 0 to .1
  //delay(10);
  fractal(vol);
  server.sendScreen();
  counter += 1;
}

void fractal(float vol) {
  /* constructs live fractal image that responds
  to music by zooming in/out and transforming shape
  */
  // Creates an image based on the Julia Set fractals
  /* general aglorithm: for every pixel, 
   iterate (where z is a complex number) z_new = z_old² + c 
   on the complex plane (represented by XY plane here)
   until it leaves a 
   circle around the 
   origin with given escapeRadius. 
   The number of iterations 
   it the color of the pixel */
  // c = a + bi where c is a complex number
  
  float zoomFactor = map(vol, 0, 0.1, 1, 100);
  if (millis() - initialTime > interval){
    // increment time as an int
    time = time + 1;
    // reset counter
    initialTime = millis();
  }
  // Uncomment for debugging constant values for fractal
  //float ca = -.4; //+ cos(angle*1.69);
  //float cb = -.59; //+ sin(angle);
  //print("ca value:   ");
  //print(ca);
  //print("        ");
  //print("cb value:   ");
  //print(cb);
  //print("        ");
  //print("The angle is:   ");
  //print(angle);
  //print("      ");
  //print("cos change:   ");
  //print(cos(angle*1.69));
  //print("      ");
  //print("sin change:   ");
  //print(sin(angle));
  //print("      ");
  //print(zoomIn);
  //print("      ");
  if (zoomIn) {
    float factor = .00000025*counter%100;
    // c = a + bi where c is a complex number
    ca -= factor; // represents the "a" constant
    cb -= factor; // represents the "b" constant
    if (ca < -.54) {
      ca = -.4;
      cb = -.59;
    }
    //angle = angle_const + random(-0.005, 0.005); // 0.008
    //angle += random(-0.005, 0.005); // 0.008
    //angle += 0.0016; // 0.008
  }
  else {
    //angle -= 0.0005;//random(-0.001, 0.001);
    //angle -= 0.002;
  }
  
  background(255);
  // Zoom in/out depending on the amplitude from the song
  if (zoomFactor > 99.4) {  // 95 // 98
    zoomIn = true;
  }
  else {
    zoomIn = false;
  }
  
  // If we've zoomed in too much (into the fractal image), zoom back out
  if (w < 2.5) { 
    w += random(0.8,2.0);
  }
  if (h < 2.5) {
    h += random(0.8,2.0);
  }
  
  // If we've zoomed out too much (from the fractal image), zoom back in 
  if (w > 5.0) { 
    w -= random(0.8,2.0);
  }
  if (h > 5.0) {
    h -= random(0.8,2.0);
  }
  // Zoom in/out depending on the amplitude from the song
  if (zoomIn) {
    zoomFactor = map(vol, 0, 0.1, 0.04, 0.1); // 0.04
    w -= zoomFactor;
    h -= zoomFactor;
  }
  else {
    // Zooming out
    zoomFactor = map(vol, 0, 0.1, 0.011, 0.01);
    w += zoomFactor;
    h += zoomFactor;
  }

  // Start at negative half the width and height
  float xmin = -w/2;
  float ymin = -h/2;

  loadPixels();

  // Maximum number of iterations for each point on the complex plane
  int maxiterations = 180; //100

  // x goes from xmin to xmax
  float xmax = xmin + w;
  // y goes from ymin to ymax
  float ymax = ymin + h;

  // Calculate amount we increment x,y for each pixel
  float dx = (xmax - xmin) / (width);
  float dy = (ymax - ymin) / (height);

  // Start y
  float y = ymin;
  // iterate through every pixel
  // iterate z_new = z_old² + c 
  // c is determined by the changing constant factors from above (ca and cb)
  // the complex number z is determined by the pixel's position
  for (int j = 0; j < height; j++) {
    // Start x
    float x = xmin;
    for (int i = 0; i < width; i++) {
      float a = x;
      float b = y;
      int n = 0;
      while (n < maxiterations) {
        float aa = a * a; // aa & bb are used in squaring a complex number
        float bb = b * b;
        if (aa + bb > 4.0) { // 4.0 = escape_radius here
          norm = sqrt(aa*aa + bb*bb); // "norm" of complex number
          break;  // Exit
        }
        float twoab = 2.0 * a * b; 
        a = aa - bb + ca; // real part: squaring a complex number
        b = twoab + cb; // imaginary part: squaring a complex number
        n++;
      }

      if (n == maxiterations) { // if this complex number is in the Julia Set (i.e. it's norm converges)
        pixels[i+j*width] = color(0); 
      } else {
        // if the complex number is NOT in Julia Set (i.e. it's norm "diverged")
        // pick a color based on the num of iterations it took to escape the "escape_radius"
        float hu = huePicker(n);
        pixels[i+j*width] = color(hu, 255, 255);
      }
      x += dx; // increment x
    }
    y += dy; // increment y
  }
  updatePixels();
  reflect(); // create a 2x2 reflected image
}

void reflect() {
  /*
  generates relflected images
  */
  PImage img = get(0,0,200,200);
  image(img,0,0);
  pushMatrix();
  translate(0,height);
  scale(1,-1);
  image(img,0,0);
  popMatrix();
  pushMatrix();
  translate(width,0);
  scale(-1,1);
  image(img,0,0);
  popMatrix();
  img = get(0,height/2,200,200);
  translate(width,height);
  image(img,0,0);
}
