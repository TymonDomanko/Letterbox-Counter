# Letterbox Count

A single-page web app that counts how many mailboxes (letterboxes) fall inside each of your delivery zones. It's handy for planning letterbox drops, flyer runs and order-form deliveries.

**Use it:** https://tymondomanko.github.io/Letterbox-Counter/

## What it does

- **Zones from a KML or KMZ file.** Draw your zones as shapes in Google My Maps or Google Earth, export them, and load the file. Each shape becomes a zone, named after its placemark.
- **Fetches addresses automatically.** As soon as zones are loaded, the addresses inside them are fetched from OpenStreetMap. Nothing to download or paste.
- **Count per zone and in total.** Mailboxes that sit in two overlapping zones show up in both zone rows but are only counted once in the total.
- **Filter by type.** Houses, apartments/units and businesses can each be switched on or off. Apartments are off by default.
- **Map.** Zones and every address are drawn on a street map. Click a dot to see its address and how it was counted.
- **Remembers your work.** Your zones and filter choices are kept in your browser. Fetched address data is cached for two weeks, so the page opens instantly next time. Use **Refresh address data** to fetch it fresh.
- **Export.** Copy the table, or download it as CSV.

## How to use it

1. Load your zones (`.kml` or `.kmz`).
2. Wait a few seconds while the addresses are fetched. A progress bar shows how it's going.
3. Tick which mailbox types to count. The table and map update straight away.

You can also use your own address data, under **Use your own address file instead**. It accepts a CSV with latitude/longitude columns (such as a G-NAF extract), GeoJSON, or Overpass JSON. Rows with a value in a `unit`, `flat` or `flat_number` column count as apartments.

## How fetching works

Addresses come from the [Overpass API](https://wiki.openstreetmap.org/wiki/Overpass_API), which serves OpenStreetMap data. The app sends one request covering all your zones. If a server is busy or fails, it tries the next one in this list:

1. overpass.private.coffee
2. overpass-api.de
3. maps.mail.ru
4. overpass.kumi.systems

If the area is too big for one request (or wider than about 30 km²), the app splits it into roughly 2 × 2 km parts automatically, splitting further if needed, and merges the results without double counting.

## How mailboxes are classified

| Type | Rule |
|---|---|
| Apartment | The address is inside a building tagged `apartments`/`flats`, or has a unit number (`addr:unit`, or `3/12` style numbering). If the building's number of flats is known, each flat counts as one mailbox. |
| Business | The address has a shop, office, amenity, craft, healthcare or tourism tag, or sits in a commercial-type building. |
| House | Every other address. |

## Accuracy

OpenStreetMap address coverage varies from suburb to suburb. Check the map for streets with no dots before you rely on a count. For a guaranteed-complete count, use a G-NAF extract as the address source.

## Running it locally

It's a single HTML file with no build step. Serve the folder with any static web server (for example `python3 -m http.server`) and open it in a browser. Map tiles come from CARTO, using OpenStreetMap data.

## Licence

MIT. See [LICENSE](LICENSE).
