# How to make nice terrain in Unity

***Requires Unity 2019.1 and later. Last tested in Unity 2021.3.8f1***
(**Using this guide: https://www.youtube.com/watch?v=ddy12WHqt-M**)

## Prerequisites

1. Have some nice assets installed, e.g.: 
[https://assetstore.unity.com/packages/2d/textures-materials/floors/outdoor-ground-textures-12555](https://assetstore.unity.com/packages/2d/textures-materials/floors/outdoor-ground-textures-12555)
[https://assetstore.unity.com/packages/3d/vegetation/trees/conifers-botd-142076](https://assetstore.unity.com/packages/3d/vegetation/trees/conifers-botd-142076)
[https://assetstore.unity.com/packages/2d/textures-materials/nature/grass-and-flowers-pack-1-17100](https://assetstore.unity.com/packages/2d/textures-materials/nature/grass-and-flowers-pack-1-17100)
[https://assetstore.unity.com/packages/3d/environments/flooded-grounds-48529](https://assetstore.unity.com/packages/3d/environments/flooded-grounds-48529)

1.  Import said assets
2.  Right click in the hierarchy, go to 3D Object > Terrain
3.  Adjust parameters as necessary - select > options 
4.  Import ***Terrain Tools*** from the Package Manager

## Sculpting

1.  Select the sculpting tool

3.  Select brushes and sculpt away!

### Tips:

-  Hold **A** and drag mouse to increase brush strength
-  Hold **S** and drag mouse to increase brush size

-  Hold **D** and move mouse to rotate brush


## Texturing

1. Select the brush
2. Select **Paint Texture** from the drop down menu
3. Scroll down until you reach the **Layers** submenu
4. Expand it and hit add layer.

6. Choose a layer and that will now be your base!
7. Edit normal maps and other parameters to your liking!

### Alternatively:

1. Look for "**Create New layer**", name your layer - e.g.: *Grass*
2. Click Create, and choose your texture.

3. Add your normal map
4. Edit parameters to your liking!

5. Add another layer, name it.

7. Choose your texture and apply normal maps
8. Texture away by essentially painting!

### Tips:

- Create a 1x2x1 cube for reference!

- Enable your lighting for the best visuals!


## Adding trees

1.  Select your **terrain** GameObject and go to the **Add Trees** section
2.  Hit **Edit Trees** then **Add Tree**
3.  Choose your tree

4. Click and drag to add trees!

### Tips:

-  Hold **Ctrl** to delete trees

-  Use the **Mass Place Trees** button to.. well.. mass place trees to your whole terrain!
-  Use multiple tree meshes for the best looks
-  Go to your **lighting settings** > Environment > Scroll down and enable **Fog**
-  Go to your **Terrain's settings** and toggle the **Draw** option to quickly hide and show **trees**


## Adding grass

1. Go back to your **Terrain** and head to the **Paint Details** section

3. Hit **edit details**


### 2D Grass - performant, but not as visually pleasing

1. Click **Add Grass Texture**

2. Attach your **detail texture**
3. Hit add
4. Click and drag to add grass!
5. Edit colour tint and size to make it as realistic as possible


### 2D Grass - performant, but not as visually pleasing

1. Click **Add Detail Mesh**

2. Attach your **3D Mesh GameObject**
3. Change colour to make it blend in.
4. Click and drag to add grass!
Copy
