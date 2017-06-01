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

Cesium's particle systems work with the entity framework.  This code will load a 3D model of an airplane, and then attach a simple fire particle system to it.  The image property defines what
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

**Positioning Emitters**

Particle systems are attached to it's parent entity and particles will be emitted from the entity's position.  It is also possible to position the emitter
within the coordinate space of the entity.  For example, what if we want the fire particles to emit from the propeller of the plane instead of the center?
We can use the emitterModelMatrix property of the ParticleSystem to accomplish this.  Change the entity creation code to the following:

```
var entity = viewer.entities.add({
    model : {
        uri : '../../SampleData/models/CesiumAir/Cesium_Air.gltf',
    },

    particleSystem : {
        image : '../../SampleData/fire.png',

        rate: 50.0,

        emitter: new Cesium.ConeEmitter({
            radius: 5.0,
            angle: Cesium.Math.toRadians(30.0)
        }),

        // Position the emitter on the propeller.
        emitterModelMatrix: Cesium.Matrix4.fromTranslation(new Cesium.Cartesian3(2.5, 4.0, 1.0)),
    },

    position : Cesium.Cartesian3.fromDegrees(-112.110693, 36.0994841, 1000)
});
```

## Configuring Particle Systems

Cesium has numerous options for fine tuning how particle systems behave.

**Particle emission rate**

The *rate* property controls how many particles are emitted per second.

You can also specify an array of *burst* objects to apply timed bursts of particles.  This is a great way to add some variety or explosions to your particle system.

Add this property to your particleSystem.

```
bursts: [
            {time: 5.0, min: 300, max: 500},
            {time: 10.0, min: 50, max: 100},
            {time: 15.0, min: 200, max: 300}
        ],
```

These bursts will emit between min and max particles at the given times.

**Lifetime**
There are a few properties that control the lifetime of a particle system.  By default, particle systems will run forever.

If you want a particle system to run for a specified amount of time and then stop, you can set the **lifetime** property to the number of seconds you want it to run and set the *loop* property to false.  For example, to run a particle system for 5 seconds and stop, you can do this:

```
var entity = viewer.entities.add({
    model : {
        uri : '../../SampleData/models/CesiumAir/Cesium_Air.gltf',
    },

    particleSystem : {
        image : '../../SampleData/fire.png',

        rate: 50.0,

        emitter: new Cesium.ConeEmitter({
            radius: 5.0,
            angle: Cesium.Math.toRadians(30.0)
        }),

        lifeTime: 5.0,
        loop: false,

        emitterModelMatrix: Cesium.Matrix4.fromTranslation(new Cesium.Cartesian3(2.5, 4.0, 1.0)),
    },

    position : Cesium.Cartesian3.fromDegrees(-112.110693, 36.0994841, 1000)
});
```



## Styling particles
olors, etc.

## Forces


