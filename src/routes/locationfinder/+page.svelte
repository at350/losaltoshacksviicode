<script>
    import Navbar from '../../components/Navbar.svelte';
    import { onMount } from 'svelte'
    import mapboxgl from "mapbox-gl";
    import { initializeApp } from "firebase/app";
    import { doc, getDocs, query, collection, getFirestore } from 'firebase/firestore';
    import { counties } from "../counties.js";

    // replaces spaces in string with plus sign
    function replaceSpaces(str) {
            return str.replace(/\s/g, '+');
    }

    const firebaseConfig = {
        apiKey: "[YOUR API KEY]",
        authDomain: "losaltoshacksvii.firebaseapp.com",
        projectId: "losaltoshacksvii",
        storageBucket: "losaltoshacksvii.appspot.com",
        messagingSenderId: "[YOUR ID]",
        appId: "[YOUR ID]"
    };
    
    let locs = [];
	mapboxgl.accessToken = '[YOUR API KEY]';
    let map;

    onMount(async () => {
        map = new mapboxgl.Map({
            container: "map",
            style: "mapbox://styles/mapbox/dark-v10",
            // center in San Francisco
            center: [-122.03120659644243, 37.41082299046011],
            zoom: 8,
        });

        const db = getFirestore(initializeApp(firebaseConfig));
        const querySnapshot = await getDocs(collection(db, 'locs'));
        querySnapshot.forEach((doc) => {
            const data = doc.data();
            locs.push({
                id: doc.id,
                name: data.name,
                lat: data.loc.latitude,
                long: data.loc.longitude
            });
        })

        // Loop through each location
         /* for (let i = 0; i < locationNames.length; i++) {
            // Get the location name
            const urlQuery = locationNames[i];
            const options = {
                method: 'GET',
                url: `https://nominatim.openstreetmap.org/search?q=${replaceSpaces(urlQuery)}&format=json`
            }
            axios.request(options).then(function (response) {
                console.log(response.data[0].lat)
                console.log(response.data[0].lon)
            })
        } */

        map.on('load', () => {
            const geojson = {
                type: 'FeatureCollection',
                features: counties.map(loc => ({
                    type: 'Feature',
                    properties: {},
                    geometry: {
                    type: 'Point',
                    coordinates: [loc.longitude, loc.latitude]
                    }
                }))
            };

            // Add the GeoJSON data as a source
            map.addSource('counties', {
                type: 'geojson',
                data: geojson
            });

            map.addLayer({
                id: 'countyCircles',
                type: 'circle',
                source: 'counties',
                paint: {
                    'circle-color': [
                        'interpolate',
                        ['linear'],
                        ['heatmap-density'],
                        0,
                        'rgba(0, 255, 0, 1)',
                        0.1,
                        'rgba(255, 0, 0, 1)',
                        0.3,
                        'rgba(255, 255, 0, 1)',
                        0.5,
                        'rgba(0, 255, 0, 1)',
                        1,
                        'rgba(0, 255, 0, 1)'
                    ],
                'circle-radius': {
                    stops: [
                    [0, calculateHeatmapRadius(0, map.getCenter().lat, 15)],
                    [22, calculateHeatmapRadius(22, map.getCenter().lat, 15)],
                    ],
                    base: 2,
                },
                'circle-opacity': 0.3,
                },
            });

            addHeatmapLayer(locs);
        });
    })

    function addHeatmapLayer(locations) {
        // Convert the locations array to GeoJSON format
        const geojson = {
        type: 'FeatureCollection',
        features: locations.map(loc => ({
            type: 'Feature',
            properties: {},
            geometry: {
            type: 'Point',
            coordinates: [loc.long, loc.lat]
            }
        }))
        };

        map.addSource('places', {
            'type': 'geojson',
            'data': {
            'type': 'FeatureCollection',
            'features': locations.map(loc => ({
                type: 'Feature',
                properties: {
                    'description': loc.name
                },
                geometry: {
                    type: 'Point',
                    coordinates: [loc.long, loc.lat]
                }
            }))
        }});

        map.addLayer({
            'id': 'places',
            'type': 'circle',
            'source': 'places',
            'paint': {
            'circle-color': '#4264fb',
            'circle-radius': 6,
            'circle-stroke-width': 2,
            'circle-stroke-color': '#ffffff'
        }
        });
        
        // Create a popup, but don't add it to the map yet.
        const popup = new mapboxgl.Popup({
            closeButton: false,
            closeOnClick: false
        });
        
        map.on('mouseenter', 'places', (e) => {
            // Change the cursor style as a UI indicator.
            map.getCanvas().style.cursor = 'pointer';
            
            // Copy coordinates array.
            const coordinates = e.features[0].geometry.coordinates.slice();
            const description = e.features[0].properties.description;
            
            // Ensure that if the map is zoomed out such that multiple
            // copies of the feature are visible, the popup appears
            // over the copy being pointed to.
            while (Math.abs(e.lngLat.lng - coordinates[0]) > 180) {
                coordinates[0] += e.lngLat.lng > coordinates[0] ? 360 : -360;
            }
        
            // Populate the popup and set its coordinates
            // based on the feature found.
            popup.setLngLat(coordinates).setHTML(description).addTo(map);
        });
            
        map.on('mouseleave', 'places', () => {
            map.getCanvas().style.cursor = '';
            popup.remove();
        });

        // Add the GeoJSON data as a source
        map.addSource('locs', {
            type: 'geojson',
            data: geojson
        });

        map.addLayer({
            id: 'circles',
            type: 'circle',
            source: 'locs',
            paint: {
                'circle-color': [
                    'interpolate',
                    ['linear'],
                    ['heatmap-density'],
                    0,
                    'rgba(255, 255, 100, 1)',
                    0.1,
                    'rgba(255, 0, 0, 1)',
                    0.3,
                    'rgba(255, 255, 0, 1)',
                    0.5,
                    'rgba(0, 255, 0, 1)',
                    1,
                    'rgba(0, 255, 0, 1)'
                ],
            'circle-radius': {
                stops: [
                    [0, calculateHeatmapRadius(0, map.getCenter().lat, 40)],
                [9, calculateHeatmapRadius(9, map.getCenter().lat, 25)],
                // Add additional stops to shrink circles at higher zoom levels
                [12, calculateHeatmapRadius(12, map.getCenter().lat, 3)],
                [15, calculateHeatmapRadius(15, map.getCenter().lat, 1)],
                ],
                base: 2,
            },
            'circle-opacity': 0.15,
            },
        });
    }

    function calculateHeatmapRadius(zoom, latitude, miles) {
        const metersPerMile = 1609.34;
        const tileSize = 512; // Mapbox uses 512x512 tile size
        const circumferenceAtEquator = 40075016.686; // Meters

        const metersPerPixelAtZoom0 = circumferenceAtEquator / tileSize;
        const metersPerPixel = metersPerPixelAtZoom0 * Math.cos((latitude * Math.PI) / 180) / Math.pow(2, zoom);

        return (miles * metersPerMile) / metersPerPixel;
    }
</script>

<svete:head>
    <link href="https://api.mapbox.com/mapbox-gl-js/v2.13.0/mapbox-gl.css" rel="stylesheet">
    <script src="https://api.mapbox.com/mapbox-gl-js/v2.13.0/mapbox-gl.js"></script>
</svete:head>

<Navbar />
<div class="map-key1">
    <div class="circle green"></div>
    <span>Traditionally Disadvantaged Areas</span>
</div>
<div class="map-key2">
    <div class="circle yellow"></div>
    <span>Upcoming/Current Hackathons</span>
</div>

<div id="map"></div>

<style>
    #map { position: absolute; top: 125px; bottom: 0; width: 100%; }

    .mapboxgl-popup {
max-width: 400px;
font: 12px/20px 'Helvetica Neue', Arial, Helvetica, sans-serif;
}

.map-key1 {
    position: absolute;
    bottom: 67px;
    right: 20px;
    display: flex;
    align-items: center;
    background-color: #191a1a;
    border-radius: 5px;
    padding: 10px 15px;
    font-size: 14px;
    z-index: 1000;
}

.map-key2 {
    position: absolute;
    bottom: 20px;
    right: 20px;
    display: flex;
    align-items: center;
    background-color: #191a1a;
    border-radius: 5px;
    padding: 10px 15px;
    font-size: 14px;
    z-index: 1000;
}

.map-key1 span, .map-key2 span {
    color: #fff;
}

.circle {
    display: inline-block;
    width: 15px;
    height: 15px;
    border-radius: 50%;
    margin-right: 10px;
}

.green {
    background-color: #32CD32;
}

.yellow {
    background-color: #FFFF00;
}

</style>
