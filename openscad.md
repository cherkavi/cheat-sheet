# OpenSCAD
## [installation](https://openscad.org/downloads.html)
```sh
sudo apt install openscad
```

## start
```sh
openscad &
```

## [documentation](https://openscad.org/documentation.html)

## libraries
* [genuine](https://openscad.org/libraries.html)
* 

## examples
### original examples
```sh
find /usr/share/openscad/examples
```
### [how to use libraries ](https://en.wikibooks.org/wiki/OpenSCAD_User_Manual/Libraries)

## web viewer
### [web viewer for stl files](https://www.viewstl.com/plugin/)
```html
<html>
<head>
    <title>My OpenSCAD Model</title>
    <script src="stl_viewer.min.js"></script>
</head>
<body >

    <div id="stl_viewer_container" style="width:1600px; height:900px;"></div>

    <script>
        // 3. Initialize the viewer
        var my_viewer = new StlViewer(
            document.getElementById("stl_viewer_container"),
            {                 
                bg_color: "#808080",                
                allow_interact: true,
                // Set the camera position on load
                camera_state: {
                    position: { x: 150, y: -150, z: 150 }, // Adjust these values to zoom out further
                    target: { x: 0, y: 0, z: 0 }
                },
                models: [ 
                    {
                        id: 0, 
                        filename: "temp.stl",  // your filenam
                        color: "#ff5500"
                    } ]
            }
        );
    </script>

</body>
</html>
```

### web viewer for glb
1. [convert stl to glb](https://convert3d.org/stl-to-glb/app)
2. show on the page
```html
<html>
<head>
    <title>My OpenSCAD Model</title>
    <script type="module" src="https://ajax.googleapis.com/ajax/libs/model-viewer/3.3.0/model-viewer.min.js"></script>
</head>
<body >
<model-viewer 
    src="temp.glb"  // your file 
    camera-controls 
    auto-rotate 
    shadow-intensity="1" 
    style="width: 100%; height: 100%;">
</model-viewer>
</body>
</html>
```
