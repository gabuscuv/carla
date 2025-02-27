# Town 12

![town_12](../img/catalogue/maps/town12/town12.webp)

Town 12 is a Large Map with dimensions of 10x10 km<sup>2</sup>. It is divided into 36 tiles, most with dimensions of 2x2 km<sup>2</sup> (some edge tiles are smaller). The road layout is partially inspired by the road layout of the city of [Amarillo in Texas, USA](https://www.google.com/maps/place/Amarillo,+TX,+USA/@35.2018863,-101.9450251,11z/data=!3m1!4b1!4m5!3m4!1s0x870148d4b245cf03:0xd0f3d11c6836d2af!8m2!3d35.2219971!4d-101.8312969). There are numerous contrasting regions to the city including urban, residential and rural areas, along with a large highway system surrounding the city with a ringroad. The architectural styles reflect those of many medium to large cities across North America.  

## Navigator

The navigator interactive map can be used to browse the town and derive coordinates to use in the CARLA simulator.

__Using the navigator__:

* `left mouse button` - click and hold, drag left, right, up or down to move the map
* `scroll mouse wheel` - scroll down to zoom out, scroll up to zoom in on the location under the mouse pointer
* `double click` - double click on a point on the map to record the coordinates, you will find the coordinates in the text and the code block just below the map

__Zone color reference__:

* <span style="color:#595d5e; background-color:#595d5e;">&nbsp</span>   [Skyscraper](#high-rise-downtown)
* <span style="color:#d2dddc; background-color:#d2dddc;">&nbsp</span>   [High density residential](#high-density-residential)
* <span style="color:#838c8b; background-color:#838c8b;">&nbsp</span>   [Community buildings](#community-buildings)
* <span style="color:#17d894; background-color:#17d894;">&nbsp</span>   [Low density residential](#low-density-residential)
* <span style="color:#df6a19; background-color:#df6a19;">&nbsp</span>   [Parks](#parks)
* <span style="color:#839317; background-color:#839317;">&nbsp</span>   [Rural farmland](#rural-and-farmland)
* <span style="color:#265568; background-color:#265568;">&nbsp</span>   [Water](#water)



![town12_aerial](../img/catalogue/maps/town12/town12roadrunner.webp#map)


__CARLA coordinates__: 

* __X__:  <span id="carlacoord_x" style="animation: fadeMe 2s;">--</span>
* __Y__:  <span id="carlacoord_y" style="animation: fadeMe 2s;">--</span>


After double clicking on a point of interest, the navigator will display the corresponding CARLA coordinates and update them in the following code block. Copy and paste the code into a notebook or Python terminal to translate the spectator to the desired location. You will need first to [connect the client and set up the world object](tuto_first_steps.md#launching-carla-and-connecting-the-client):

```py
# CARLA coordinates: X 0.0, Y 0.0
spectator = world.get_spectator()
loc = carla.Location(0.0, 0.0, 500.0)
rot = carla.Rotation(pitch=-90, yaw=0.0, roll=0.0)
spectator.set_transform(carla.Transform(loc, rot))
```
## Town 12 zones

#### High-rise downtown:

Town 12's downtown area is a large span of high-rise skyscrapers arranged into blocks on a consistent grid of roads, resembling downtown areas in many large American and European cities.

![high_rise](../img/catalogue/maps/town12/high_rise.webp)

#### High density residential:

The high density residential areas of Town 12 have many 2-10 storey apartment buildings with commercial properties like cafes and retail stores at street level.

![high_dens_res](../img/catalogue/maps/town12/hi_dens_res.webp)


#### Community buildings:

The community buildings are a set of 2-4 storey apartment buildings in a colorful bohemian style with cafes and boutiques on the ground floors, located next to the downtown area of the city.

![community](../img/catalogue/maps/town12/community.webp)

#### Low density residential:

The low density residential regions of Town 12 reflect the classic suburbs of many American cities, with one and two story homes surrounded by fenced gardens and garages.

![low_dens_res](../img/catalogue/maps/town12/low_dens_res.webp)

#### Parks:

The dense residential and downtown areas are broken up by small islands of green communal space, juxtaposing green foliage against urban architecture.

![parks](../img/catalogue/maps/town12/parks.webp)

#### Highways and intersections:

Town 12 has an extensive highway system, including 3-4 lane highways interspersed with impressive roundabout junctions and intersections.

![highway](../img/catalogue/maps/town12/highway.webp)

#### Rural and farmland:

Town 12 also has rural regions with characteristic farmland buildings like wooden barns and farmhouses, windmills, grain silos, corn fields, hay bails and rural fencing. These areas have unmarked country dirt roads and single lane interurban roads for inter-city traffic.

![rural](../img/catalogue/maps/town12/rural.webp)

#### Water:

There are several bodies of water in town 12 including 2 large lakes and several ponds. With some large water features located next to the city, these can produce inverted reflections of the skyline, creating challenges for autonomous driving agents. 

![water](../img/catalogue/maps/town12/water.webp)

<style>
@keyframes fadeMe {
  from {
    color: #77aaff;
  }
  to {
    color: #000000;
  }
}

</style>
<script>
window.addEventListener('load', function () {

    var text_coord_x = document.getElementById("carlacoord_x")
    var text_coord_y = document.getElementById("carlacoord_y")
    const code_coords = document.getElementsByClassName("hljs-number")
    const code_comment = document.getElementsByClassName("hljs-comment")
  
    const image = document.querySelector('[src$="map"]');
    const canv = document.createElement('canvas');

    canv.setAttribute('height', image.height)
    canv.setAttribute('width', image.width)
    image.parentNode.replaceChild(canv, image)

    var state = {mDown: false, button: 0, lastX: 0, lastY:0, canvX: 0, canvY: 0, zoom: 1.0, mdownX: 0, mdownY: 0, pX: 0.5, pY: 0.5, dblClick: false, listObj: false, touch: false}

    ctx = canv.getContext('2d')
    ctx.drawImage(image, 0, 0, canv.width, canv.height)

    canv.addEventListener('mousemove', (event) => {
        dX = event.clientX - state.lastX
        dY = event.clientY - state.lastY
        state.lastX = event.clientX
        state.lastY = event.clientY

        if(state.mDown && state.button == 0) {
            state.canvX += dX
            state.canvY += dY
            ctx.clearRect(0, 0, canv.width, canv.height)
            ctx.drawImage(image,  state.canvX, state.canvY, canv.width * state.zoom, canv.height * state.zoom)
            state.touch = true;
        }
    })

    canv.addEventListener('mousedown', (event) => {

        state.button = event.button;
        state.mDown = true;
        state.touch = true;

        var rect = canv.getBoundingClientRect();
            
        state.mdownX = event.clientX - rect.left;
        state.mdownY = event.clientY - rect.top;

        state.pX = (state.mdownX - state.canvX) / (canv.width * state.zoom);
        state.pY = (state.mdownY - state.canvY) / (canv.height * state.zoom);
    })

    canv.addEventListener('mouseup', (event) => {
        state.mDown = false;
    })

    canv.addEventListener('wheel', (event) => {
        
        state.mDown = false;

        var rect = canv.getBoundingClientRect();

        dX = event.clientX - rect.left;
        dY = event.clientY - rect.top;

        state.pX = (dX - state.canvX) / (canv.width * state.zoom);
        state.pY = (dY - state.canvY) / (canv.height * state.zoom);

        if(state.touch){
            event.preventDefault();
            if(event.wheelDelta > 0){
                state.zoom *= 1.15 
            } else {
               state.zoom *= 0.85
            }

            if(state.zoom < 1.0){state.zoom = 1.0;}
            if(state.zoom > 30.0){state.zoom = 30.0}

            ctx.clearRect(0, 0, canv.width, canv.height)

            state.canvX = - canv.width * state.zoom * state.pX + dX;
            state.canvY = - canv.height * state.zoom * state.pY + dY;

            ctx.drawImage(image,  state.canvX, state.canvY, canv.width * state.zoom, canv.height * state.zoom);
        }
        
    })

    canv.addEventListener('dblclick', (event) => {
        
        text_coord_x = document.getElementById("carlacoord_x")
        text_coord_y = document.getElementById("carlacoord_y")

        const carlaX = 10482.4274 * state.pX + -5.39801455 * state.pY - 5673.07949;
        const carlaY = 5.39801455 * state.pX + 10482.4274 * state.pY - 2885.15738;

        code_coords[0].textContent = carlaX.toFixed(1)
        code_coords[1].textContent = carlaY.toFixed(1)
        code_comment[0].textContent = "# CARLA coordinates - X: " + carlaX.toFixed(1) + " Y: " + carlaY.toFixed(1)

        var newX = text_coord_x.cloneNode(true)
        var newY = text_coord_y.cloneNode(true)

        newX.textContent = carlaX.toFixed(1)
        newY.textContent = carlaY.toFixed(1)

        var parentX = text_coord_x.parentNode
        var parentY = text_coord_y.parentNode

        parentX.replaceChild(newX, text_coord_x);
        parentY.replaceChild(newY, text_coord_y);

    })

})
</script>

