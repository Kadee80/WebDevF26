# Assignment: Geolocation in the Browser

**Course:** Intro to Front-End Web Development
**Topic:** Using the Browser's Geolocation API

---

## Overview

In this assignment you'll learn how to use the browser's built-in **Geolocation API** to access a user's physical location with JavaScript. You'll start by reading through the core concepts, then build a working project that plots your location on an interactive map.

**What you'll need:**
- A text editor (VS Code, Sublime, or similar)
- A modern web browser (Chrome or Firefox recommended)
- A basic understanding of HTML, CSS, and JavaScript (variables, functions, `document.getElementById()`)

**What you'll submit:**
- A completed `my-map.html` file (see Part 5)

---

## Part 1: What Is Geolocation?

Your web browser knows things about the device it's running on. One of those things is **where you are in the world**. The **Geolocation API** is a built-in browser feature that lets a website ask for your physical location.

### How Does the Browser Know Where You Are?

The browser uses a combination of signals depending on the device:

| Signal | How It Works | Accuracy |
|---|---|---|
| **GPS** | Satellite signal (phones, tablets) | Very precise (~3 meters) |
| **Wi-Fi** | Matches nearby Wi-Fi networks to known locations | Good (~20-50 meters) |
| **Cell towers** | Triangulates from nearby cell towers | Moderate (~100-300 meters) |
| **IP address** | Looks up your internet connection's rough location | Low (~city level) |

On a laptop without GPS, you'll typically get Wi-Fi or IP-based location. On a phone, you'll get GPS-level precision.

### Privacy First

This is important: **the browser always asks the user for permission before sharing their location.** A website cannot silently track where you are. When your code requests geolocation, the user sees a popup like:

> "example.com wants to know your location. [Allow] [Block]"

If the user clicks **Block**, your code receives an error — and that's something you'll need to handle.

---

## Part 2: The Geolocation API

### Checking If Geolocation Is Available

Not every browser or device supports geolocation (though most modern ones do). You should always check first:

```javascript
if ("geolocation" in navigator) {
  console.log("Geolocation is available!");
} else {
  console.log("Geolocation is not supported by this browser.");
}
```

**What's `navigator`?** It's a built-in JavaScript object that contains information about the browser. Think of it as the browser's "about me" page. The geolocation feature lives inside it.

### Requesting the User's Position

```javascript
navigator.geolocation.getCurrentPosition(success, error);
```

This function takes **two arguments**, both of which are functions:

1. **`success`** — called if the user allows location access and the browser finds their position
2. **`error`** — called if something goes wrong (user denied permission, GPS unavailable, etc.)

### The Success Function

When geolocation works, the browser passes a `position` object to your success function. That object contains coordinates:

```javascript
function success(position) {
  var latitude = position.coords.latitude;
  var longitude = position.coords.longitude;

  console.log("Latitude:", latitude);
  console.log("Longitude:", longitude);
}
```

**What are latitude and longitude?** They're numbers that pinpoint a location on Earth. Latitude measures north/south (-90 to 90). Longitude measures east/west (-180 to 180). For example, New York City is roughly latitude 40.7, longitude -74.0.

### The Error Function

Things can go wrong. The user might deny permission, or the device might not be able to determine its location. You should always handle this:

```javascript
function error(err) {
  console.log("Something went wrong:", err.message);
}
```

Common error messages you might see:
- `"User denied Geolocation"` — the user clicked Block
- `"Position unavailable"` — the device couldn't determine location
- `"Timeout"` — it took too long to get a position

### The Complete Pattern

Here's everything together. Read through this carefully — you'll be using this same pattern when you build your project.

```javascript
if ("geolocation" in navigator) {
  navigator.geolocation.getCurrentPosition(success, error);
} else {
  console.log("Geolocation is not supported.");
}

function success(position) {
  var latitude = position.coords.latitude;
  var longitude = position.coords.longitude;
  console.log("Latitude:", latitude);
  console.log("Longitude:", longitude);
}

function error(err) {
  console.log("Error:", err.message);
}
```

---

## Part 3: Try It Yourself — Display Coordinates on a Page

Before jumping into the main project, try this quick exercise to make sure geolocation is working on your machine.

1. Create a new file called `geolocation-test.html`
2. Copy in the code from the demo file provided with this assignment (`geolocation-demo.html`)
3. Open it in your browser by double-clicking the file or dragging it into Chrome
4. Click the **"Find My Location"** button
5. When the browser asks for permission, click **Allow**

You should see your latitude and longitude appear on the page. If you see an error message instead, make sure you're using Chrome or Firefox and that location services are enabled on your device.

**Now try this:** refresh the page, click the button again, and this time **deny** the permission. Notice how the error message appears instead. This is why we always write an error handler.

---

## Part 4: What Else Is in the Position Object?

The `position.coords` object has more than just latitude and longitude:

| Property | What It Is | Notes |
|---|---|---|
| `latitude` | North/south position | Always available |
| `longitude` | East/west position | Always available |
| `accuracy` | Accuracy in meters | Always available |
| `altitude` | Height above sea level | May be `null` on laptops |
| `altitudeAccuracy` | Accuracy of altitude | May be `null` |
| `heading` | Direction of travel (degrees) | May be `null` |
| `speed` | Speed in meters/second | May be `null` |

On a laptop, you'll typically only get `latitude`, `longitude`, and `accuracy`. On a phone with GPS, you may get all of them.

Open the browser console (`Cmd + Option + J` on Mac, `Ctrl + Shift + J` on Windows) and try adding these lines to your success function to see what your device reports:

```javascript
console.log("Accuracy:", position.coords.accuracy, "meters");
console.log("Altitude:", position.coords.altitude);
```

---

## Part 5: Your Assignment — Plot Your Location on a Map

Now you're going to build a page that finds the user's location and shows it on an interactive map using **Leaflet**, a free and beginner-friendly JavaScript mapping library.

### What Is Leaflet?

Leaflet is an open-source JavaScript library for interactive maps. You don't need to install anything — you'll load it directly from a CDN (a link in your HTML file).

### Instructions

Create a new file called `my-map.html`. Build it step by step following the instructions below.

**Step 1 — Set up the HTML skeleton with Leaflet loaded from a CDN:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Location Map</title>

  <!-- Leaflet CSS — this styles the map -->
  <link rel="stylesheet"
    href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />

  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 20px;
      text-align: center;
    }

    #map {
      height: 400px;
      width: 100%;
      max-width: 800px;
      margin: 20px auto;
      border-radius: 8px;
      border: 2px solid #d1d5db;
    }

    #status {
      margin: 16px 0;
      font-size: 18px;
      color: #374151;
    }
  </style>
</head>
<body>

  <h1>My Location on a Map</h1>
  <div id="status">Finding your location...</div>
  <div id="map"></div>

  <!-- Leaflet JavaScript — this gives us the L.map(), L.marker(), etc. functions -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <script>
    // YOUR CODE WILL GO HERE (Steps 2-4)
  </script>

</body>
</html>
```

**Step 2 — Request the user's location.**

Replace `// YOUR CODE WILL GO HERE` with code that:
- Gets a reference to the `#status` element
- Checks if geolocation is available
- If it is, calls `getCurrentPosition` with a success function called `showOnMap` and an error function called `handleError`
- If geolocation isn't available, updates the status text to say so

**Step 3 — Write the `showOnMap` function.**

This is the core of the assignment. Your function should:
1. Pull `latitude` and `longitude` from the position object
2. Update the `#status` text to display the coordinates
3. Create a Leaflet map inside the `#map` div, centered on the user's coordinates (use zoom level `15`)
4. Add a tile layer so the map has actual map imagery (use OpenStreetMap tiles)
5. Add a marker at the user's coordinates with a popup that says "You are here!"

Here are the Leaflet functions you'll need:

```javascript
// Create a map inside an element, centered on [lat, lon] at zoom level 15
var map = L.map("map").setView([lat, lon], 15);

// Add OpenStreetMap tiles to the map
L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
  attribution: "&copy; OpenStreetMap contributors"
}).addTo(map);

// Add a marker with a popup
L.marker([lat, lon]).addTo(map).bindPopup("You are here!").openPopup();
```

**Step 4 — Write the `handleError` function.**

If something goes wrong, update the `#status` text to show what the error was.

### Testing Your Work

1. Open `my-map.html` in Chrome or Firefox
2. Allow the location permission when prompted
3. You should see an interactive map centered on your location with a marker and popup
4. Try zooming in and out, dragging the map around
5. Refresh and try denying the permission — your error message should appear

### Stretch Challenges (Optional, for extra credit)

If you finish early, try adding these features:

1. **Show accuracy** — Add a translucent circle around the marker showing how accurate the location reading is.
   *Hint:* `L.circle([lat, lon], { radius: position.coords.accuracy }).addTo(map);`

2. **Custom popup** — Change the popup text to display the actual latitude and longitude values instead of "You are here!"

3. **Experiment with zoom** — The number `15` in `setView` controls the zoom level. Try `10` (zoomed way out) or `18` (zoomed way in). Pick the level that feels most useful and leave a comment in your code explaining why you chose it.

---

## What to Submit

- Your completed `my-map.html` file
- Make sure it works when opened directly in a browser (no build tools, no npm, just the HTML file)

---

## Key Concepts to Remember

| Concept | What You Learned |
|---|---|
| **Geolocation API** | A built-in browser feature for accessing the user's physical location |
| **Permission model** | The browser always asks the user before sharing location — privacy by default |
| **`navigator.geolocation.getCurrentPosition()`** | The core function — takes a success callback and an error callback |
| **`position.coords`** | Contains `latitude`, `longitude`, `accuracy`, and more |
| **Error handling** | Always handle the case where location is denied or unavailable |
| **Leaflet** | A free JavaScript library for showing interactive maps on a web page |

---

## Further Reading

- [MDN: Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API) — the official reference documentation
- [Leaflet Quick Start Guide](https://leafletjs.com/examples/quick-start/) — more things you can do with maps
- [Can I Use: Geolocation](https://caniuse.com/geolocation) — browser compatibility table
