---
title: Design pattern
date: 2024-02-12T07:09:05.055+05:30
draft: false
tags:
  - design_pattern
  - webdev
---
to rember the design pattern narrate it as real life story

resources https://youtu.be/taj_inLi-pY?si=2qu_7nqx6YRwX0Co

## Crational Design pattern

Once upon a time in the colorful land of Toyland, there were magical creatures known as Creators. Each Creator had a unique way of bringing toys to life, showcasing various creational design patterns.

1. **Abstract Factory Pattern: The Artisan Workshop**
    
    In the heart of Toyland, there was an Artisan Workshop where the master toymaker, the Abstract Artisan, crafted a variety of toys. Depending on the season – be it spring, summer, fall, or winter – the Artisan would use a specialized machine to create toys that matched the spirit of that season. The workshop could seamlessly switch between crafting delightful flower-shaped puzzles in spring to building snowflake-adorned snow globes in winter.
    
    **Design Pattern Explanation:** The Abstract Factory pattern is like the Artisan Workshop, creating toys that match the theme of each season using a specialized machine.
    
```js
class SeasonalToyFactory {
    createToy() {
        throw new Error("Abstract method. Please implement in concrete subclass.");
    }
}

class SpringToyFactory extends SeasonalToyFactory {
    createToy() {
        return new FlowerToy();
    }
}

class WinterToyFactory extends SeasonalToyFactory {
    createToy() {
        return new SnowmanToy();
    }
}

```
    
2. **Builder Pattern: The Custom Toy Shop**
    
    Right next to the Artisan Workshop, there was a Custom Toy Shop run by a jolly toymaker, the Builder Elf. Children from Toyland could visit the shop and choose the parts they wanted for their custom toy – selecting the type of body, color, and accessories. The Builder Elf would then assemble the chosen parts, creating a unique toy tailored to each child's preferences.
    
    **Design Pattern Explanation:** The Builder pattern is like the Custom Toy Shop, where a toymaker assembles a toy based on the specific choices made by the children.
    
```js
class CustomToyBuilder {
    constructor() {
        this.toy = new Toy();
    }

    buildBody(body) {
        this.toy.setBody(body);
    }

    buildColor(color) {
        this.toy.setColor(color);
    }

    buildAccessories(accessories) {
        this.toy.setAccessories(accessories);
    }

    getToy() {
        return this.toy;
    }
}

```
3. **Factory Method Pattern: The Animal Wonderland Factory**
    
    A bit further away, there was an Animal Wonderland Factory. Here, the Animal Creator designed a variety of stuffed animals, each with its own special feature. The factory had different machines, like the Bear-Maker and Bunny-Maker, each specializing in creating a particular type of stuffed animal. Every day, the Animal Creator would decide which adorable animal to make and use the corresponding machine.
    
    **Design Pattern Explanation:** The Factory Method pattern is like the Animal Wonderland Factory, where different machines create various stuffed animals based on the decision of the Animal Creator.
    
```js
class AnimalCreator {
    createAnimal() {
        throw new Error("Abstract method. Please implement in concrete subclass.");
    }
}

class BearCreator extends AnimalCreator {
    createAnimal() {
        return new Bear();
    }
}

class BunnyCreator extends AnimalCreator {
    createAnimal() {
        return new Bunny();
    }
}

```
4. **Singleton Pattern: The Timeless Toy Museum**
    
    In the center of Toyland stood the Timeless Toy Museum, housing the most beloved and classic toys. The museum had a magical guardian, the Singleton Guardian, who ensured there was only one entrance to the museum. No matter how many children wanted to enter, they all had to go through the same door, preserving the uniqueness and timelessness of the exhibited toys.
    
    **Design Pattern Explanation:** The Singleton pattern is like the Timeless Toy Museum, where the Singleton Guardian ensures there's only one entrance, maintaining the uniqueness and timelessness of the exhibited toys.
    
```js
class SingletonGuardian {
    static #instance;

    constructor() {
        if (!SingletonGuardian.#instance) {
            SingletonGuardian.#instance = this;
        }

        return SingletonGuardian.#instance;
    }
}

```

5. **Prototype Pattern: The Replicator's Workshop**
    
    Nestled on the outskirts of Toyland, there was the Replicator's Workshop. Here, the Prototype Puppeteer, a wizard with a magical wand, had the ability to duplicate toys effortlessly. When a special toy was created, the Puppeteer would wave their wand, and presto – an exact replica would appear! This allowed for the mass production of popular toys without starting from scratch each time.
    
    **Design Pattern Explanation:** The Prototype pattern is like the Replicator's Workshop, where the Prototype Puppeteer magically duplicates toys, making exact copies without crafting each one from the beginning.
    
```js
class Toy {
    clone() {
        return Object.assign(Object.create(Object.getPrototypeOf(this)), this);
    }
}

class PrototypePuppeteer {
    makeDuplicate(originalToy) {
        return originalToy.clone();
    }
}

```


## **Structural Design Patterns:**

In the charming town of Toyland, where toys came to life, there was an ingenious architect named StructureBuilder. StructureBuilder loved designing magical playhouses using various structural design patterns, each contributing to the uniqueness and durability of the toy structures.

1. **Adapter Pattern: The Magical Toy Connector**

   One day, the StructureBuilder wanted to connect a set of new magnetic building blocks with the existing wooden ones. To make them work together seamlessly, the architect created a Magical Toy Connector. This connector translated the language of magnets into the language of wooden pegs, allowing different types of building blocks to connect effortlessly.

   ```javascript
   class WoodenBlock {
       insertIntoSlot() {
           // Insert into a wooden slot
       }
   }

   class MagneticBlock {
       attachWithMagnet() {
           // Attach with a magnet
       }
   }

   class MagicalToyConnector {
       connectWoodenToMagnetic(woodenBlock, magneticBlock) {
           woodenBlock.insertIntoSlot();
           magneticBlock.attachWithMagnet();
       }
   }
   ```

2. **Bridge Pattern: The Enchanted Rope Bridge**

   StructureBuilder wanted to create an enchanting rope bridge connecting two treehouses. Using the Bridge pattern, the architect separated the abstraction of the bridge from its implementation. Now, whether it was a magical vine or a sparkling thread, it could be easily swapped without affecting the overall structure.

   ```javascript
   class RopeBridge {
       constructor(implementation) {
           this.implementation = implementation;
       }

       crossBridge() {
           this.implementation.cross();
       }
   }

   class MagicalVineBridge {
       cross() {
           // Cross the bridge using magical vines
       }
   }

   class SparklingThreadBridge {
       cross() {
           // Cross the bridge using sparkling threads
       }
   ```

3. **Composite Pattern: The Tower of Imagination**

   The StructureBuilder dreamt of creating a Tower of Imagination using a variety of building blocks. With the Composite pattern, individual blocks and complex structures could be treated uniformly. The Tower of Imagination could have both simple blocks and other towers as its components, forming a magnificent and unified structure.

   ```javascript
   class BuildingBlock {
       build() {
           // Build the block
       }
   }

   class TowerOfImagination {
       constructor() {
           this.components = [];
       }

       addComponent(component) {
           this.components.push(component);
       }

       build() {
           for (const component of this.components) {
               component.build();
           }
       }
   }
   ```

4. **Decorator Pattern: The Colorful Paintbrush**

   StructureBuilder wanted to add vibrant colors to a plain wooden carousel. Instead of modifying the carousel directly, the architect used the Decorator pattern. A ColorDecorator wrapped around the carousel, adding a burst of colors without altering its underlying structure.

   ```javascript
   class Carousel {
       spin() {
           // Spin the carousel
       }
   }

   class ColorDecorator {
       constructor(carousel, color) {
           this.carousel = carousel;
           this.color = color;
       }

       spin() {
           this.carousel.spin();
           this.addColor();
       }

       addColor() {
           // Add the chosen color to the carousel
       }
   }
   ```

5. **Proxy Pattern** 
	The proxy pattern provides a surrogate or placeholder for another object to control access to it. For instance, managing access to an object that is expensive to instantiate.

  ```
class AuthProxy {
    constructor(user, service) {
        this.user = user;
        this.service = service;
    }

    performAction() {
        if (this.user.hasAccess) {
            this.service.performAction();
        } else {
            console.log("Access denied: You do not have permission to perform this action.");
        }
    }
}
```

```
const user = { name: "John", hasAccess: false };
const realService = new RealService();
const proxy = new AuthProxy(user, realService);

proxy.performAction();
```
6. **Facade Pattern: The Magic Toy Workshop**

   In the heart of Toyland, StructureBuilder established the Magic Toy Workshop, a place of enchantment where various toys were crafted. To simplify the complexity of the toy-making process, StructureBuilder implemented the Facade pattern. The Magic Toy Workshop acted as a facade, providing a single entry point for creating different types of toys.

   ```javascript
   class MagicToyWorkshop {
       createTeddyBear() {
           // Create a teddy bear
       }

       createDoll() {
           // Create a doll
       }

       createToyCar() {
           // Create a toy car
       }
   }
   ```

7. **Flyweight Pattern** 

   The **Flyweight Pattern** is useful because it helps reduce memory consumption and improve performance by sharing common parts of objects across multiple instances.
   
   A text editor might contain millions of characters with different fonts, styles, and sizes. By using the Flyweight Pattern, the editor can share common glyphs and styles across characters, reducing memory usage.
   
```
class FontStyle {
    constructor(font, size, color) {
        this.font = font;
        this.size = size;
        this.color = color;
    }
}
```

```
class FontFactory {
    constructor() {
        this.styles = {};
    }

    getFontStyle(font, size, color) {
        const key = `${font}-${size}-${color}`;
        if (!this.styles[key]) {
            this.styles[key] = new FontStyle(font, size, color);
        }
        return this.styles[key];
    }
}
```

```
// Client code
const factory = new FontFactory();

const style1 = factory.getFontStyle('Arial', 12, 'Black');
const style2 = factory.getFontStyle('Arial', 12, 'Black');  // Reused

console.log(style1 === style2);
```