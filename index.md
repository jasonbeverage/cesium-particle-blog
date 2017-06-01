# Cesium Particle Systems

*This tutorial will walk you through Cesium's particle system API and show you how you can add more realism and interesting special effects to your Cesium app.*

## What is a particle system?

Particle systems are a graphical technique that let you simulate complex physically based effects.  At their core, particle systems are a collection of small images that when viewed together form a more complex "fuzzy" object, such as fire, smoke, clouds, or fireworks.  You'll see examples of particle systems in movies and video games.  Imagine you want to represent damage to a plane.  You can use Cesium's particle system to display an explosion on the plane's engine, then render a different particle system representing a smoke trail from the plane as it crashes.

## Quick Start

Let's get a simple particle system up and running with Cesium.  Open the [Hello World](https://cesiumjs.org/Cesium/Apps/Sandcastle/index.html?src=Hello%20World.html) Sandcastle example.  Add this code to the example:

```
var entity = viewer.entities.add({
    model : {
        uri : '../../SampleData/models/CesiumAir/Cesium_Air.gltf',
    },

    // Create a particle system
    particleSystem : {
        image : '../../SampleData/fire.png',
        rate: 50.0
    },
    position : Cesium.Cartesian3.fromDegrees(-112.110693, 36.0994841, 1000)
});
viewer.trackedEntity = entity;
```

Cesium's particle systems work with the entity framework.  This code will load a 3D model of an airplane, and then attach a simple fire particle system to it.
We're also setting the rate parameter so that the particle system will emit 50 particles every second.


## Emitters

You'll see in the example that the particles start at the center of the model and shoot up.  When a particle is born, it's initial position and velocity vector are controlled by the ParticleEmitter that is attached to the particleSystem.  If no emitter is specified, a CircleEmitter will be created by default.

Cesium has various ParticleEmitter's that you can use out of the box.

**BoxEmitter**

The BoxEmitter class initializes particles at random positions within a box and directs them outwards from the center of the box.

```
 particleSystem : {
        image : '../../SampleData/fire.png',

        rate: 50.0,

        emitter: new Cesium.BoxEmitter({
            width: 5.0,
            height: 5.0,
            depth: 5.0
        })
    }
```

**CircleEmitter**

The CircleEmitter class initializes particles at random positions within a circle and directs them up.

```
 particleSystem : {
        image : '../../SampleData/fire.png',

        rate: 50.0,

        emitter: new Cesium.CircleEmitter({
            radius: 5.0
        })
    }
```

**ConeEmitter**

The ConeEmitter class initializes particles at the tip of a cone and directs them at random angles out of the cone.

```
 particleSystem : {
        image : '../../SampleData/fire.png',

        rate: 50.0,

        emitter: new Cesium.ConeEmitter({
            radius: 5.0,
            angle: Cesium.Math.toRadians(30.0)
        })
    }
```

**SphereEmitter**

The SphereEmitter class initializes particles at random positions within a sphere and directs them outwards from the center of the sphere.

```
 particleSystem : {
        image : '../../SampleData/fire.png',

        rate: 50.0,

        emitter: new Cesium.SphereEmitter({
            radius: 5.0
        })
    }
```

## Particle Lifetime

Particles are born, react to forces such as gravity, wind, or attractive forces, and then die.

## Styling particles
bursts, colors, etc.

## Forces


