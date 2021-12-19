# Taking Cesium for Unreal engine to Mars

[Cesium](https://cesium.com) is a powerful **3D Geospatial platform** that recently added integration with the [Unreal game engine](https://www.unrealengine.com/en-US/) enabling developers to build very large games with streaming data.

This will walk you through making an extremly simple game from **NASA JPL‘s open mars data set** you can see here: https://nasa-ammos.github.io/3DTilesRendererJS/example/bundle/mars.html. 

 
The data is **streaming and geo referenced** (sort of for this one but data on earth works great), which means in the correct coordinate system rather than a flat surface, similar to **google earth**. The blurier parts are recorded by Satellites, the sharper parts by their rover.

[[Finished zipped unreal project in this repro]](./mars.zip)

<img src="https://user-images.githubusercontent.com/74843139/146665986-92995517-1a09-405b-a1a1-e772a8aa9a24.png" width=400> <img src="https://user-images.githubusercontent.com/74843139/146668235-ea7620d4-e1fa-4d41-8e95-f80ca338b24f.png" width=290>

Result and **source data set**

There is no need to download any data for the environment, we use it directly from **NASA's github**. 

I also added NASA's **Mars Perseverance Rover**, 3D Model here https://mars.nasa.gov/resources/25042/mars-perseverance-rover-3d-model/.
Since the gltf plugin in Unreal did not quite gave me the result i wanted I added a [Perseverance1.obj](./Perseverance1.obj) in this repro.

There are already some **great beginner tutorials** for Cesium in Unreal but lets see how far we can take it...

- [Taking Cesium for Unreal engine to Mars](#taking-cesium-for-unreal-engine-to-mars)
  * [Unreal Project Setup](#unreal-project-setup)
  * [Add the mars terrain tile set](#add-the-mars-terrain-tile-set)
  * [Add a car/rover to mars](#add-a-car-rover-to-mars)
  * [Finishing up](#finishing-up)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>



## Unreal Project Setup

Install **Unreal engine**. I am using Unreal engine version 4.27.2 here.

Make a **new blank Unreal project** (or any other you like).
Delete all project default content in the **WorldOutliner** tab on the right hand side of the gui. 

You can add **a new level** to the project and call it "Mars", you may want to set it as default in **project settings**.

Enable the **Cesium Unreal plugin** available at Unreal [Marketplace](https://cesium.com/learn/unreal/unreal-quickstart). 
If enabled it opens a **Cesium tab** when you hit the **Cesium icon** pictured on the top right (green and blue) of the toolbar. 
Getting a **cesium ion token** is optional for this project since we are using our own assets and no terrain or maps.

<img src="https://user-images.githubusercontent.com/74843139/146661875-0fb51be6-275a-42f2-b33e-27b9f492eecc.png" width=600>

The **Unreal cesium panel** shows on the left, Cesium tool bar icon top right


## Add the mars terrain tile set

Make a **“Blank 3d Tiles Tileset”** actor in the **Cesium panel** on the left by hitting the **plus sign** next to it.

Select the new tile set actor in the **WorldOutliner** and find the **“Cesium”** section in **Details tab** below the WorldOutliner. 
Set the **“source” attribute** to “url” and copy the **link** below as shown in the next picture (left).
https://raw.githubusercontent.com/NASA-AMMOS/3DTilesSampleData/master/msl-dingo-gap/0528_0260184_to_s64o256_colorize/0528_0260184_to_s64o256_colorize/0528_0260184_to_s64o256_colorize_tileset.json

<img src="https://user-images.githubusercontent.com/74843139/146661882-47df2114-278d-45cf-8c10-bc4efd81d55d.png" width=400> <img src="https://user-images.githubusercontent.com/74843139/146661885-ce606870-7b09-404d-bc09-e4df8d5e5ea1.png" width=400>

Setting url for the tile set actor, **Level of Detail** settings for the tile set actor


Next set the **level of detail** attributes for the tile set (shown in the picture right).
A smaller **screen space error** give you more detail in the tile set (0 is the best). Also disable **Tile set culling** to prevent tiles from being unloaded.
Hit the **"refresh tile set"** button to see the result.

Select and frame (hit the F key) the **tile set actor** you just edited in WorldOutliner. You might see it from **below** as pictured here in the top view.


![image](https://user-images.githubusercontent.com/74843139/146671752-c694084d-76bd-4384-a02a-5b51ed0cd550.png)

The **mars tile set** seen from below in vieport  (the grid are the tiles)	and in WorldOutliner


A **Cesium Georeference actor** has automatically been generated in the WorldOutliner. It is a way of dealing with a large coordinate system.
The **minimum distance** from Earth to Mars is about **33.9 million miles (54.6 million kilometers)**. That is a quite large game, even for what Cesium has build. 
If you are familiar with **floating point precision errors** it is easy to guess that we need to think of a solution here.

Select your **Cesium Georeference actor** in the WorldOutliner. In its **Details tab**, click the **"Place Georeference origin here" button** while framing the just generated tileset in view as shown below.

Change to the **top view** to see your mars tile better. Set the **geo reference** in the tile sets "Cesium" Detail settings to your georeference actor as shown above.
Hit the **"Refresh tile set" button** above the georeference field.


## Add a car/rover to mars

Place a **standard cube actor** (from the **"Place actors" tab** on the right hand side of the gui) in top view on the terrain tile. You can do that by just dragging it in. This one is scaled up already.
We use it as a **start location for the car** later as the tile set may not be fully loaded initially causing the car to fall down.


<img src="https://user-images.githubusercontent.com/74843139/146661894-683ae114-2e54-4e95-8801-62346e186e67.png" width=400>

The tile set show in **top view** and a standard cube actor placed on it


Frame the cube in viewort (with F key) and in the **detail panel** hit the Georeference's **"Place Georeference Origin here"** button again.

You may need to scale up the **"Place actors" tab** of the cube in the **Details tab** so the whole car fits on it.
Add a **Cesium Globe anchor component** to the cube (Use the green Add Componengt button below the Outliner for this). In the details tab find **Cesium** section and set georeference field to your georeference.

Add the **“Advanced vehicle blueprint”** to your projects from **Content browser** (shown below, use the green Add/Import button to select it). 

Drag the **Blueprint swatch** from content browser in the top view to your tile set. Place it above the cube actor on the tileset as shown below.

<img src="https://user-images.githubusercontent.com/74843139/146669631-d6656ce0-30d1-45e8-91af-dd681c85bebf.png" width=400> <img src="https://user-images.githubusercontent.com/74843139/146661901-de463e19-f540-4c1f-a4ec-702a2f1494c5.png" width=400> 

The "advanced vehicle blueprint" and rover model


In the vehicles **Details tab** find the "Pawn" category. There set **Auto Posesses Player settings** to “Player0” to make the car your default pawn (player).

<img src="https://user-images.githubusercontent.com/74843139/146669780-3dde6f66-9ece-44d4-a33a-201a8362becf.png" width=400>  <img src="https://user-images.githubusercontent.com/74843139/146669621-b982a978-7578-4255-a63c-06b901a3fed8.png" width=400>

**Auto Posesses** Player settings to make it active by default


If you also want to place the **rover** model on mars import it in the **content browser** and drag it to the to view like you just did with the car, add a hidden cube below.


Last step: In the Details tab of your cube set **"rendering"** to off to hide the cube in the game (shown below).

<img src="https://user-images.githubusercontent.com/74843139/146671634-0949c883-f10b-4cb6-9b29-63be70d5aff0.png width=400>
 
Set rendering to **off** to hide the cube


## Finishing up
Drag a **light actor** like **“Directional light”** from the **"Place actors" tab** on top of the tile set in the viewport. I could not get good result from Cesium's **sun and sky** lights for this but it works great on arth. Build it (in the toolbar).

Hit **play** in the toolbar and use **WASD keys** to drive the car around mars.

![Animation1](https://user-images.githubusercontent.com/74843139/146667178-19284cb2-8eee-4337-b1a7-a83ed956f4ce.gif)

It would be awsome to add a bit more **fake detail** to the ground (noise for example).
My understanding is that it is not possible with tile sets yet but i am convinced we can soon do that.

* More details on tile sets https://github.com/CesiumGS/3d-tiles
* https://cesium.com/learn/unreal/unreal-quickstart/
