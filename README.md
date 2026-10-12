# Letterbox Count

A single-page web app that counts how many mailboxes (letterboxes) fall inside each of your delivery zones. It's handy for planning letterbox drops, flyer runs and order-form deliveries.

**Use it:** https://tymondomanko.github.io/Letterbox-Counter/

## What it does

- **Zones from a KML or KMZ file.** Draw your zones as shapes in Google My Maps or Google Earth, export them, and load the file. Each shape becomes a zone, named after its placemark.
- **Count per zone and in total.** Mailboxes that sit in two overlapping zones show up in both zone rows but are only counted once in the total.
- **Filter by type.** Houses, apartments/units and businesses can each be switched on or off. Apartments are off by default.
- **Street map with zoom and pan.** Your zones and every address point are drawn over streets, parks, water and rail. Use the scroll wheel or a pinch to zoom, and drag to pan.
- **Export.** Copy the table, or download it as CSV.

## How to use it

1. Load your zones (`.kml` or `.kmz`).
2. Click **Download addresses for all zones**. A tab opens with the data, which comes from OpenStreetMap through the Overpass API. Select all, copy, and paste it into the box in the app. You can also save it as a `.json` file and load that.
   - If the download times out, use **Get it in smaller parts** and load each part.
   - If a server is busy, choose another one from the **Data server** list.
3. Tick which mailbox types to count. The table and map update straight away.

You can also load a CSV with latitude/longitude columns, such as a G-NAF extract. Rows with a value in a `unit`, `flat` or `flat_number` column count as apartments.

## How mailboxes are classified

| Type | Rule |
|---|---|
| Apartment | The address is inside a building tagged `apartments`/`flats`, or has a unit number (`addr:unit`, or `3/12` style numbering). If the building's number of flats is known, each flat counts as one mailbox. |
| Business | The address has a shop, office, amenity, craft, healthcare or tourism tag, or sits in a commercial-type building. |
| House | Every other address. |

## Accuracy

OpenStreetMap address coverage varies from suburb to suburb. Check the map for streets with no points before you rely on a count. For a guaranteed-complete count, use a G-NAF extract as the address source.

## Running it locally

It's a single HTML file with no build step. Open `index.html` in a browser, or serve the folder with any static web server.
