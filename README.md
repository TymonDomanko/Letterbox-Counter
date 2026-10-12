# Letterbox Count

A single-page web app that counts how many mailboxes (letterboxes) fall inside each of your delivery zones. It's handy for planning letterbox drops, flyer runs and order-form deliveries.

**Use it:** https://tymondomanko.github.io/Letterbox-Counter/

## What it does

- **Zones from a KML or KMZ file.** Draw your zones as shapes in Google My Maps or Google Earth, export them, and load the file. Each shape becomes a zone, named after its placemark.
- **Fetches addresses automatically.** As soon as zones are loaded, the addresses inside them are fetched. Nothing to download or paste. In NSW they come from the official NSW address register (the source behind G-NAF, updated daily); everywhere else from OpenStreetMap.
- **Shows how current the data is.** A **Last changed** column gives the typical year each zone's addresses were last updated in the NSW register, and each address's popup shows its own date.
- **Count per zone and in total.** Mailboxes that sit in two overlapping zones show up in both zone rows but are only counted once in the total.
- **Filter by type.** Houses, apartments/units and businesses can each be switched on or off. Apartments are off by default.
- **Map.** Zones and every address are drawn on a street map. Click a dot to see its address and how it was counted.
- **Remembers your work.** Your zones and filter choices are kept in your browser. Fetched address data is cached (NSW for a week, OpenStreetMap for two weeks), so the page opens instantly next time. Use **Refresh address data** to fetch it fresh.
- **Export.** Copy the table, or download it as CSV.

## How to use it

1. Load your zones (`.kml` or `.kmz`).
2. Wait a few seconds while the addresses are fetched. A progress bar shows how it's going.
3. Tick which mailbox types to count. The table and map update straight away.

You can also use your own address data, under **Use your own address file instead**. It accepts a CSV with latitude/longitude columns (such as a G-NAF extract), GeoJSON, or Overpass JSON. Rows with a value in a `unit`, `flat` or `flat_number` column count as apartments.

## Where the addresses come from

### Inside NSW: the NSW address register

When your zones are in NSW, addresses come from the [NSW Geocoded Addressing Theme](https://www.data.nsw.gov.au/data/dataset/71c1c645-910e-4e16-8841-a646b47315a3), the state's official address database run by NSW Spatial Services. It is open data, updated daily, and it is where G-NAF gets its NSW addresses. The app queries it directly, 2,000 addresses at a time.

| Type | Rule |
|---|---|
| Apartment / unit | A unit address (`8/24-26 Boundary Road`, or starting with Unit, Flat, Apartment) on a site with three or more units. The building's own address isn't counted separately, because each unit is. |
| House | Any other residential address. A site with only one or two unit addresses (a duplex or granny flat) counts its units as houses. |
| Business | An address starting with Shop, Suite, Office, Kiosk, Factory, Warehouse, Stall, Showroom or Level, or one OpenStreetMap marks as a business. |

Retired addresses and duplicate records are left out. If the register can't be reached, the app falls back to OpenStreetMap and says so.

### Elsewhere: OpenStreetMap

Outside NSW, addresses come from OpenStreetMap through the [Overpass API](https://wiki.openstreetmap.org/wiki/Overpass_API). If a server is busy or fails, the app tries the next one in this list:

1. overpass.private.coffee
2. overpass-api.de
3. maps.mail.ru
4. overpass.kumi.systems

If the area is too big for one request (or wider than about 30 km²), it is split into roughly 2 × 2 km parts automatically and merged without double counting.

| Type | Rule |
|---|---|
| Apartment | The address is inside a building tagged `apartments`/`flats`, or has a unit number (`addr:unit`, or `3/12` style numbering). If the building's number of flats is known, each flat counts as one mailbox. |
| Business | The address has a shop, office, amenity, craft, healthcare or tourism tag, or sits in a commercial-type building. |
| House | Every other address. |

## Accuracy

In NSW the counts come from the official register, so coverage is complete as of the day you fetch. Its **Last changed** year of 2008 means an address hasn't changed since the register was set up, which is normal for established houses. Outside NSW, OpenStreetMap coverage varies from suburb to suburb, so check the map for streets with no dots before you rely on a count.

## Running it locally

It's a single HTML file with no build step. Serve the folder with any static web server (for example `python3 -m http.server`) and open it in a browser. Map tiles come from OpenStreetMap (no API key needed).

## Licence

MIT. See [LICENSE](LICENSE).
