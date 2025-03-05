<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="utf-8">
    <title>หน่วยงานส่วนภูมิภาคในสังกัดสำนักวิจัยและพัฒนาการป่าไม้</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <script src="https://unpkg.com/leaflet-control-geocoder/dist/Control.Geocoder.js"></script>
    <style>
        body {
            margin: 0;
            display: flex;
            height: 100vh;
            flex-direction: row;
        }

        #map {
            flex-grow: 1;
            position: relative;
        }

        #sidebar {
            width: 25%;
            background: linear-gradient(135deg, #f9f9f9, #e3f2fd);
            overflow-y: auto;
            padding: 10px;
            box-shadow: -2px 0 8px rgba(0, 0, 0, 0.2);
            font-size: 14px;
            font-family: Arial, sans-serif;
        }

        h3 {
            margin-top: 0;
            font-size: 16px;
            color: #1e88e5;
            text-align: center;
            font-weight: bold;
        }

        .checkbox-item {
            margin: 10px 0;
            background: #ffffff;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            display: flex;
            align-items: center;
        }

        .checkbox-text {
            color: #555;
            cursor: pointer;
            flex-grow: 1;
        }

        .tree-info-container {
            display: none;
            background-color: #dff0d8;
            padding: 10px;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            margin-top: 5px;
        }
    </style>
</head>
<body>
    <div id="map"></div>
    <div id="sidebar">
        <h3>หน่วยงานส่วนภูมิภาคในสังกัดสำนักวิจัยและพัฒนาการป่าไม้</h3>
        <form id="checkbox-list"></form>
    </div>

    <script>
        var map = L.map('map').setView([14.0, 100.0], 5.5);
        var osm = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            maxZoom: 18,
            attribution: '© OpenStreetMap'
        }).addTo(map);

        var satellite = L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}', {
            maxZoom: 18,
            attribution: '© ESRI, Maxar, Earthstar Geographics'
        });

        var baseMaps = {
            "แผนที่ถนน (OSM)": osm,
            "แผนที่ดาวเทียม": satellite
        };

        L.control.layers(baseMaps).addTo(map);

        var geojsonFeatures = {
            "type": "FeatureCollection",
            "features": [
                {
                    "type": "Feature",
                    "properties": {
                        "id": "chk1",
                        "name": "เชียงใหม่",
                        "treeInfo": [
                            {"name": "ไม้จัน", "coordinates": [99.3986862, 18.3170581], "info": "ไม้สัก (Tectona grandis)<br>ชื่อหัวหน้า : นายอภิรัตน์ พิรุฬห์<br>เบอร์ติดต่อ : 081-3215965"},
                            {"name": "ไม้มะค่า", "coordinates": [101.0628557, 17.3536414], "info": "ไม้ยาง (Hevea brasiliensis)"}
                        ]
                    }
                },
                {
                    "type": "Feature",
                    "properties": {
                        "id": "chk2",
                        "name": "ลำปาง",
                        "treeInfo": [
                            {"name": "ไม้สัก", "coordinates": [102.3986862, 15.3170581], "info": "ไม้สัก (Tectona grandis)"},
                            {"name": "ไม้ยาง", "coordinates": [105.0628557, 16.3536414], "info": "ไม้ยาง (Hevea brasiliensis)"}
                        ]
                    }
                }
            ]
        };

        var treeMarkers = {}; 

        function addCheckbox(feature) {
            var checkboxForm = document.getElementById("checkbox-list");
            var div = document.createElement("div");
            div.className = "checkbox-item";
            var spanText = document.createElement("span");
            spanText.className = "checkbox-text";
            spanText.innerHTML = feature.properties.name;
            spanText.addEventListener("click", function () {
                var treeInfoContainer = document.getElementById('tree-info-container-' + feature.properties.id);
                treeInfoContainer.style.display = treeInfoContainer.style.display === "block" ? "none" : "block";
            });
            div.appendChild(spanText);
            checkboxForm.appendChild(div);

            var treeInfoDiv = document.createElement("div");
            treeInfoDiv.id = "tree-info-container-" + feature.properties.id;
            treeInfoDiv.className = "tree-info-container";
            treeInfoDiv.style.display = "none";

            feature.properties.treeInfo.forEach(function (info) {
                var treeBox = document.createElement("div");
                treeBox.style.background = "#e3f2fd";
                treeBox.style.padding = "10px";
                treeBox.style.borderRadius = "5px";
                treeBox.style.marginBottom = "5px";
                treeBox.style.cursor = "pointer";

                var checkbox = document.createElement("input");
                checkbox.type = "checkbox";
                checkbox.addEventListener("change", function () {
                    if (checkbox.checked) {
                        var marker = L.marker([info.coordinates[1], info.coordinates[0]])
                            .addTo(map)
                            .bindPopup(info.info)
                            .openPopup();
                        treeMarkers[info.name] = marker;
                    } else {
                        if (treeMarkers[info.name]) {
                            map.removeLayer(treeMarkers[info.name]);
                            delete treeMarkers[info.name];
                        }
                    }
                });

                var treeLabel = document.createElement("label");
                treeLabel.innerHTML = info.name + " - " + info.info;
                treeLabel.style.cursor = "pointer";
                
                // ซูมแผนที่เมื่อคลิกที่ชื่อ ไม่ใช่ checkbox
                treeLabel.addEventListener("click", function (event) {
                    if (event.target.tagName !== "INPUT") { // ไม่ซูมหากคลิกที่ checkbox
                        map.setView([info.coordinates[1], info.coordinates[0]], 15);
                    }
                });

                treeBox.appendChild(checkbox);
                treeBox.appendChild(treeLabel);
                treeInfoDiv.appendChild(treeBox);
            });

            checkboxForm.appendChild(treeInfoDiv);
        }

        geojsonFeatures.features.forEach(addCheckbox);
    </script>
</body>
</html>
