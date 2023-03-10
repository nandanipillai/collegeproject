HTML code:
<html>
  <head>
     <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>
    <title>bikeypedia</title>
    
    <link rel = "icon" href = "../crome browser/21f0a1db48ca4a5b8e9edfd8549f25a5.png"  type = "image/x-icon">
     </head>
    <body>
<div id = "clock-rim">
  <div id = "clock-face">
    <div id = "notch-container"></div>
    <div id = "brand">HTML<br/>clock</div>
    <div id = "hour"></div>
    <div id = "minute"></div>
    <div id = "second"></div>
    <div id = "center"></div>
  </div>
</div>
        </body>
   </html>

css code:
div { border: 1px solid transparent; }
/* -- The outside rim of the clock with gradient -- */
#clock-rim {
  display: block;
  position: relative;
  width: 300px;
  height: 300px;
  background: linear-gradient(gray, black);
  border-radius: 150px;
}
/* -- Clock's face -- */
#clock-face {
  display: block;
  position: relative;  
  width: 260px;
  height: 260px;
  margin: 19px auto;
  border-radius: 150px;
  background: white;
}
/* -- Second hand -- */
#second { position: absolute; top: 123px; left: 128px; width: 100px; height: 2px; border: 1px solid transparent; border-radius: 5px; transform: unset; background: green; transform-origin: 1px 1px; }
/* -- Minute hand -- */
#minute { position: absolute; top: 123px; left: 128px; width: 100px; height: 4px; border: 1px solid transparent; border-radius: 5px; transform: unset; background: red; transform-origin: 2px 2px; }
/* -- Hour hand -- */
#hour { position: absolute; top: 123px; left: 128px; width: 50px; height: 4px; border: 1px solid transparent; border-radius: 5px; transform: unset; background: blue; transform-origin: 2px 2px; }
/* -- Center of the clock -- */
#center { position: absolute; top: 118px; left: 120px; width: 16px; height: 16px; background: white; border: 1px solid gray; border-radius: 16px; }
/* -- "HTML clock"  in a box -- */
#brand { position: absolute; top: 110px; left: 165px; border: 1px solid gray; border-radius: 5px; width: 50px; height: 40px; font-family: Verdana; font-size: 11px; text-align: center; line-height: 20px; }
/* -- Notches container -- */
#notch-container { width: 260px; height: 260px }
/* -- Hourly notch (thicker, but sparse) -- */
.notch { position: absolute; width: 10px; height: 2px; background: black; border-radius: 5px 5px 5px 5px; }
/* -- Minute notch (thin) -- */
.thin { position: absolute; width: 10px; height: 1px; border-top: 1px solid gray; }

javascript:
/* -- Convert degrees to radians (actually unused here :-) -- */
function deg2rad(d) { return (2 * d / 360) * Math.PI; }
/* -- Progress the clock's hands every once in a while -- */
setInterval(() => {
  let minute = document.getElementById("minute");
  let hour = document.getElementById("hour");
  let second = document.getElementById("second");
  /* -- note retracing by 90 degrees, this is just the way I calculated the hands, the "0" angle is at 3PM, not 12PM, -- */
  let S = new Date().getSeconds() * 6 - 90;
  let M = new Date().getMinutes() * 6 - 90;
  let H = new Date().getHours() * 30 - 90;
  
  second.style.transform = 'rotate(' + S + 'deg)';
  minute.style.transform = 'rotate(' + M + 'deg)';
  hour.style.transform = 'rotate(' + H + 'deg)';
  
}, 10); /* -- update the clock fast enough -- */
/* -- Helps calculate the angle of each hand on the clock -- */
function vec2ang(x, y) {
 angleInRadians = Math.atan2(y, x);
 angleInDegrees = (angleInRadians / Math.PI) * 180.0;
 return angleInDegrees;
}
/* -- Let's calculate position and angle of clock's notches-- */
let nc = document.getElementById("notch-container");
let angle = 0;
let rotate_x = 120;
let rotate_y = 0;
/* -- Calculate the 60 notches for seconds and minutes -- */
for (let i = 0; i < 60; i++) {
  let thin = document.createElement("div");
  let x = rotate_x * Math.cos(angle) - rotate_y * Math.cos(angle);
  let y = rotate_y * Math.cos(angle) + rotate_x * Math.sin(angle);
  let r = vec2ang(x, y);
  thin.className = "thin";
  thin.style.left = 122 + x + "px";
  thin.style.top = 127 + y + "px";
  thin.style.transform = "rotate(" + r + "deg)";
  nc.appendChild(thin);
  angle +=  (Math.PI / 300) * 10;
}
// reset angle parameters
angle = 0; rotate_x = 120; rotate_y = 0;
/* -- Calculate the thicker notches for hour hand -- */
for (let i = 0; i < 12; i++) {
  let notch = document.createElement("div");
  let x = rotate_x * Math.cos(angle) - rotate_y * Math.cos(angle);
  let y = rotate_y * Math.cos(angle) + rotate_x * Math.sin(angle);
  let r = vec2ang(x, y);
  notch.className = "notch";
  notch.style.left = 122 + x + "px";
  notch.style.top = 127 + y + "px";
  notch.style.transform = "rotate(" + r + "deg)";
  nc.appendChild(notch);
  angle +=  (Math.PI / 60) * 10;
}
