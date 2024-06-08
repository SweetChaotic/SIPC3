<!DOCTYPE html>
<html>
   <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">

        <title>Sistem Informasi Pertanahan</title>
       
        <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />

        <style>
            body {
                background-image: url('E:\OTH\BCVB.png');
                background-size: cover;
                background-repeat: no-repeat;
                background-attachment: fixed;
                background-image: linear-gradient(45deg, #ececec, #ffffff);
            }
            h1 {
                font-family: Helvetica, sans-serif;
                text-align: left;
             }
            #header {
                background: linear-gradient(to right, #00c3ff, #007ffd);
                color: rgb(255, 255, 255);
                padding: 10px;
                display: flex;
                justify-content: space-between;
                align-items: center;
                font-family: Helvetica, sans-serif;
                border-top-left-radius: 15px;
                border-top-right-radius: 15px;
                margin-left: 0,5%;
                margin-right: 0,5%;
            }
            #logo {
                width: auto ;
                height: 50px ;
                margin-left: 20px;
            }
            #menu {
                margin-right: 20px;
                font-family: Helvetica, sans-serif;
                color: rgb(255, 255, 255);
                width: 30px ;
                height: auto;
            }
            #map {
                height: 500px;
                width: 80%;
                margin: 0 auto; 
                margin-left: 0,5%; 
                margin-right: 0,5%;
            }
            #map-container {
                display: flex;
                justify-content: space-between;
            }

            #info-panel {
                width: 20%;
                background-color: rgba(255, 255, 255, 0.8);
                padding: 20px;
                border-radius: 10px;
                margin-left: 0px;
                font-family: helvetica, sans-serif;
            }
            
        </style>
        <script src="libs/jquery-3.7.1/dist/jquery.min.js"></script>
   </head> 
   <body>
    <div id="header">
        <img id="logo" src="D:\SMSTR 6\SELO6 ALT\DTB AST\aksara.png" alt="Logo">
        <h1>Sistem Informasi Pertanahan</h1>
        <div id="menu">
            <!-- <a href="#">Menu</a> -->
            <img id="menu" src="C:\xampp\htdocs\img\menutomb.png" alt="Logo">
        </div>
    </div>
    <!-- <h1>Sistem Informasi Pertanahan</h1> -->
    
    <div id="map-container">
        <div id="info-panel">
            <h2>Sistem Informasi Pertanahan</h2>
            <p>Peta Bidang Tanah WP Kawasan Perkotaan Selomerto</p>
        </div>
        <div id="map"></div>
    </div>    

    <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
    <script>
        var map = L.map('map').setView([-7.4193478,109.8999921], 13);

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);

        // data dari geoserver
        var layer1 = L.tileLayer.wms('http://localhost:8082/geoserver/wms', {
            layers: 'SIP_batas_desa',
            format: 'image/png',
            transparent: true,
            attribution: 'Kelompok C2'
        }).addTo(map);

        var layer2 = L.tileLayer.wms('http://localhost:8082/geoserver/wms', {
            layers: 'SIP_batas_WP',
            format: 'image/png',
            transparent: true,
            attribution: 'Kelompok C2'
        }).addTo(map);

        var layer3 = L.tileLayer.wms('http://localhost:8082/geoserver/wms', {
            layers: 'SIP_jalan',
            format: 'image/png',
            transparent: true,
            attribution: 'Kelompok C2'
        }).addTo(map);

        var layer4 = L.tileLayer.wms('http://localhost:8082/geoserver/wms', {
            layers: 'SIP_sungai',
            format: 'image/png',
            transparent: true,
            attribution: 'Kelompok C2'
        }).addTo(map);

        var layer5 = L.tileLayer.wms('http://localhost:8082/geoserver/wms', {
            layers: 'SIP_bidang_tanah',
            format: 'image/png',
            transparent: true,
            attribution: 'Kelompok C2'
        }).addTo(map);

        var layer6 = L.tileLayer.wms('http://localhost:8082/geoserver/wms', {
            layers: 'SIP_toponim',
            format: 'image/png',
            transparent: true,
            attribution: 'Kelompok C2'
        }).addTo(map);

        // layer control
        var overlayMaps = {
            "batas desa": layer1,
            "batas WP": layer2,
            "bidang tanah": layer5,
            "sungai": layer4,
            "jalan": layer3,
            "toponim": layer6,            
        };

        L.control.layers(null, overlayMaps).addTo(map);

        //object info
        map.on('click', function(e) {
            var pos = e.latlng;
            var pt = map.latLngToContainerPoint(pos);
            var w = map.getSize().x;
            var h = map.getSize().y;
            var bnd = map.getBounds();
            var west = bnd.getWest();
            var east = bnd.getEast();
            var north = bnd.getNorth();
            var south = bnd.getSouth();

        $.ajax({
            url: "http://localhost:8082/geoserver/wms",
            data: {
                service: "WMS",
                version: "1.1.0",
                request: "GetFeatureInfo",
                info_format: "application/json",
                layers: "SIP_batas_desa",
                query_layers: "SIP_batas_desa",
                width: w,
                height: h,
                x: parseInt(pt.x),
                y: parseInt(pt.y),
                bbox: west+','+south+','+east+','+north
            },
            dataType: 'json',
            success: function(ajaxresult){
                var data = ajaxresult.features[0].properties;
                var content = data.name;
                L.popup().setLatLng(pos).setContent(content).openOn(map);
            },
            error: function(jqXHR, textStatus, errorThrown){
                alert("error");
            }
        });
         })
    </script>
   </body>
</html>
