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
        ]
```

These bursts will emit between min and max particles at the given times.

**Lifetime**

There are a few properties that control the lifetime of a particle system.  By default, particle systems will run forever.

If you want a particle system to run for a specified amount of time and then stop, you can set the *lifetime* property to the number of seconds you want it to run and set the *loop* property to false.  For example, to run a particle system for 5 seconds and stop, you can do this:

```
particleSystem : {
    lifeTime: 5.0,
    loop: false
}

```

Each particle emitted from the particle system will live for a random number of seconds between the **minLife** and **maxLife** properties of the particle system.  For example, to make a particle system where the particles live between 5 and 10 seconds you can do:
```
particleSystem : {
    minLife: 5.0,
    maxLife: 10.0
}
```


## Styling particles

**Color**

As we've already seen, the *image* property determines which texture to use for the particles.  That image can be modulated with a color, which can change over the particles lifetime, to produce a more interesting effect.

For example, let's make the fire particles redish when they are born and then transition to a partially transparent yellow as they die.  Add the following settings to your particleSystem.

```
particleSystem : {
    startColor: Cesium.Color.RED.withAlpha(0.7),
    endColor: Cesium.Color.YELLOW.withAlpha(0.3)
}
```

** Size **
There are various settings we can use to control the size of the particles in a particle system.  The general size of a particle is controlled by the *minimumWidth*, *maximumWidth*, *minimumHeight*, and *maximumHeight* settings.  The particle will be born with a width in pixels between *minimumWidth* and *maximumWidth* and a height between *minimumHeight* and *maximumHeight*.

Let's make the particles have a size between 30 and 60 pixels.  Add the following to your particleSystem.
```
particleSystem : {
    minimumWidth: 30.0,
    maximumWidth: 60.0,
    minimumHeight: 30.0,
    maximumHeight: 60.0
}
```

You can also control the size of a particle during it's lifetime with the *startScale* and *endScale* properties.  This lets you make particles grow or shink over time.  Let's make the particles grow to 4x their start size as they age.  Add the following to your particleSystem.
```
particleSystem : {
    startScale: 1.0,
    endScale: 4.0
}
```

** Speed **
While the emitter controls the initial position and velocity vector of the particle, how fast it actually goes is controlled by the *minimumSpeed* and *maximumSpeed* settings.  Let's make our particles go between 5 and 10 and meters per second.
```
particleSystem : {
    minimumSpeed: 5.0,
    maximumSpeed: 10.0
}
```

## Forces

You can create any number of interesting effects by applying forces to your particle system such as gravity or wind.  Each particle system has an array of force callbacks that allow you to modify properties of the particle during the simulation.  A force callback is a function that takes in a particle as well as the change in simulation time.  For physics based effects, you'll usually modify the velocity vector to change direction or speed.  Let's make the particle system react to gravity.  Add this function to your example:
```
var gravityScratch = new Cesium.Cartesian3();
function applyGravity(p, dt) {
    // We need to compute a local up vector for each particle in geocentric space.
    var position = p.position;
    Cesium.Cartesian3.normalize(position, gravityScratch);
    Cesium.Cartesian3.multiplyByScalar(gravityScratch, -9.8 * dt, gravityScratch);
    p.velocity = Cesium.Cartesian3.add(p.velocity, gravityScratch, p.velocity);
}
```

This function computes a localized gravity vector for each particle, and uses the acceleration of gravity (-9.8 meters per second squared) to alter the velocity of the particle.

Now add the force to the particle systems force array like this:
```
particleSystem: {
	forces: [applyGravity]
}
```

We can't wait to see what cool effects you build with Cesium's new particle system!