# Taking Cesium for Unreal engine to Mars


Cesium is a powerful 3D Geospatial platform that recently added integration with the Unreal game engine enabling developers to build very large games with streaming data. The data is streaming and geo referenced (sort of for this one), which means in the correct coordinate system rather than a flat surface, similar to google earth.

[[Finished zipped unreal project in this repro]](./mars.zip)

<img src="https://user-images.githubusercontent.com/74843139/146665986-92995517-1a09-405b-a1a1-e772a8aa9a24.png" width=400>


This will walk you through making a game from **NASA JPL‘s open mars data set** you can see here: https://nasa-ammos.github.io/3DTilesRendererJS/example/bundle/mars.html. No need to download any data, we use it fro their server. There are already some great beginner tutorials for Cesium in Unreal but lets see how far we can take it.
I also added the **Mars Perseverance Rover**, 3D Model here https://mars.nasa.gov/resources/25042/mars-perseverance-rover-3d-model/.
An [[Perseverance1.obj]](./Perseverance1.obj) version in this repro since the gltf plugin in Unreal didnt quite gacve me what i wanted.

The minimum distance from Earth to Mars is about 33.9 million miles (54.6 million kilometers). That is a quite large game, even for what Cesium has build. If you are familiar with floating point precision errors it is easy to guess that we need to think of a solution here.


## Unreal Project Setup

Make a **new blank Unreal project** (or any other you like).
I am using Unreal version 4.27.2 here. Delete all project default content. 

**Add a level** and call it "Mars", you may want to set it as default in project settings.

Enable the **Cesium Unreal plugin** available at Unreal [[Marketplace]](https://cesium.com/learn/unreal/unreal-quickstart)  it will show up in ui, you see a green and blue icon and a panel on the right when you click that icon. Getting a cesium ion token is optional for this project since we are using our own assets and no terrain or maps.

![image](https://user-images.githubusercontent.com/74843139/146661875-0fb51be6-275a-42f2-b33e-27b9f492eecc.png" width=400>

The Unreal cesium panel	on the left,  It opens if you hit the cesium icon pictured on the top right above and find the “Cesium” section in Details tab. S

## Add the mars terrain tile set

Make a **“Blank 3d Tiles Tileset”** actor in the cesium panel by hitting the plus sign.et the “source” attribute to “url” and add the link below as shown in the next picture (left).

https://raw.githubusercontent.com/NASA-AMMOS/3DTilesSampleData/master/msl-dingo-gap/0528_0260184_to_s64o256_colorize/0528_0260184_to_s64o256_colorize/0528_0260184_to_s64o256_colorize_tileset.json

Next set the level of detail attributes for the tile set (shown in the next picture right).
Hit the refresh tile set button.
<img src="https://user-images.githubusercontent.com/74843139/146661882-47df2114-278d-45cf-8c10-bc4efd81d55d.png" width=400> <img src="https://user-images.githubusercontent.com/74843139/146661885-ce606870-7b09-404d-bc09-e4df8d5e5ea1.png" width=400>

	
Setting url for the tile set actor	Level of Detail settings for the tile set actor

Select and frame the tile set actor you just edited. You might see it from below as pictured here in the top view. A Cesium Georeference actor has automatically been generated, select it. In its Details tab, click the Place Georeference Origin Here button while framing the just generated tileset in view as shown below.

Change to top view to see your mars tile better. Add the geo reference in the tile sets Cesium Detail settings as shown below.
<img src="https://user-images.githubusercontent.com/74843139/146661890-439198ac-3d7b-433f-9743-9fe58044efac.png" width=400>

Tile set seen form below (the grid are tiles)	

## Add a car/rover to mars

Place a **standard cube actor **in top view on the terrain tile, this one is scaled up already. We use it as a start location for the car later as the tile set may not be fully loaded initially.

<img src="https://user-images.githubusercontent.com/74843139/146661894-683ae114-2e54-4e95-8801-62346e186e67.png" width=400>
The tile setin top view and a cube

Frame the cube in viewort and hit Georeference again.
Add a **Cesium Globe anchor component** to the cube. Set the https://cesium.com/learn/unreal/unreal-quickstart/in details to your georefernec.
In the Details tab set "rendering: to off to hide the cube (shown below).

<img src="https://user-images.githubusercontent.com/74843139/146661901-de463e19-f540-4c1f-a4ec-702a2f1494c5.png" width=400> <img src="https://user-images.githubusercontent.com/74843139/146655459-fdb3a169-a140-420a-aad6-4f53bb156d8a.png" width=400>
Set rendering to **off** to hide the cube

Add the **“advanced vehicle blueprint”** to your projects from **Content browser** (shown below, use the green Add/Import button to select it). Drag the blueprint from content browser in the top view of your tile set

In the cars Details tab set **Auto Posesses Player settings** to “Player0” to make the car your default pawn (player).

<img src="https://user-images.githubusercontent.com/74843139/146655459-fdb3a169-a140-420a-aad6-4f53bb156d8a.png" width=400>
Auto Posesses Player settings



## Finishing up
Add a light actor like **“Directional light” **to the scene, I couldnt get good result from Physical lighting for this. Build it (in the toolbar).

Hit **play**  and use **WASD keys** to drive the car around mars.

![Animation1](https://user-images.githubusercontent.com/74843139/146667178-19284cb2-8eee-4337-b1a7-a83ed956f4ce.gif)


* More details on tile sets https://github.com/CesiumGS/3d-tiles
* https://cesium.com/learn/unreal/unreal-quickstart/
