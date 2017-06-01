# Cesium Particle Systems

*This tutorial will walk you through Cesium's particle system API and show you how you can add more realism and interesting special effects to your Cesium app.*

## What is a particle system?

Particle systems are a graphical technique that let you simulate complex physically based effects.  At their core, particle systems are a collection of small images that when viewed together form a more complex "fuzzy" object, such as fire, smoke, clouds, or fireworks.  You'll see examples of particle systems in movies and video games.  Imagine you want to represent damage to a plane.  You can use Cesium's particle system to display an explosion on the plane's engine, then render a different particle system representing a smoke trail from the plane as it crashes.

## Quick Start

Let's get a simple particle system up and running with Cesium.  Open the [Hello World]("https://cesiumjs.org/Cesium/Apps/Sandcastle/index.html?src=Hello%20World.html") Sandcastle example.  Add this code to the example:

```
var entity = viewer.entities.add({
    model : {
        uri : '../../SampleData/models/CesiumAir/Cesium_Air.gltf',
    },

    // Create a particle system
    particleSystem : {
        image : '../../SampleData/fire.png'
    },

    position : Cesium.Cartesian3.fromDegrees(-112.110693, 36.0994841, 1000)
});
viewer.trackedEntity = entity;
```

Cesium's particle systems work with the entity framework.  This code will load a 3D model of an airplane, and then attach a simple fire particle system to it.


## The life of a particle

Particles are born, react to forces such as gravity, wind, or attractive forces, and then die.  Cesium's particle system is modular and configurable, allowing you interactively generate effects.

## Emitters

Different types of emitters.


