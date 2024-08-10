# System Generation Specification

A System is defined as a collection of stars and a number of bodies; which can be children of other bodies/stars.


A System Hierarchy may look like (ignoring realistic requirements):
```
System XYZ
|
| -> Star 1 (systemid=System XYZ, ...)
|     | -> Mercury (*,parent=Star 1, ...)
|     | -> Venus (*,parent=Star 1, ...)
|     | -> Earth (*,parent=Star 1, ...)
|           | -> Luna (*,parent=Earth, ...)
|     | -> Mars (*,parent=Star 1, ...)
|           | -> Pheibos (*,parent=Mars, ...)
|              ...
|  
|
| -> Star 2 (systemid=System XYZ, ...)
|     | -> Saturn (*,parent=Star 2, ...)
|           | -> Europa (*,parent=Saturn, ...)
|     | -> Jupiter (*, parent=Star 2, ...)
```
All Objects in a system have a `Distance` and `Angular_Offset_Deg`, 
which define the distance (in simulation units) 
and angular rotation around the major body that it is parented to. 
- In the case of **Asteroid Belts**, `Distance` shall be the distance offset from that of the 
Belt (positive or negative). Bodies which are the direct children of an Asteroid Belt **do not generate orbit lines**. 
- In the case of a body (whose **parent is an Asteroid Belt**, i.e. '**Major Body**') which has child bodies ('**Minor Bodies**'), orbit lines will be generated for those Minor Bodies around the Major body.
- In the case of **Planets** (or '**Major Bodies**'), `Distance` shall be the **distance from the body or star it is parented to**.
- In the case of **Stars**, `Distance` shall be the distance from the **Barycenter** of the system (virtual midpoint in simulation terms). **Stars do not have orbit lines around the barycenter (for obvious reasons)**

**Orbit Lines** are circular, no eccentricity, procession or other real-life orbital properties. This is for the sake of ease of simulation.

# Encoding
The Information Above is encoded over three tables: `SystemIndexTB`, `StarsTB` and `BodiesTB`
Encoding is based on the API scopes.
- `/api/open/sysmap/get/system/<systemid>`
```json
{
   "status": 200,
   "message": "OK",
   "system_id": "System_XYZ",
   "datetime": "{datetime osi object repr}",
   "System_XYZ": {
      "Gal_X": 0,
      "Gal_Y": 0,
      "Gal_Z": 0,
      "Preferred_Name": "Epsilon Eridani",
      "Arity": 1,
      "Temperature": [0,0,0]
   }
}
```
Note that Arity and Temperature are determined fields by the number of stars in the system and composite spectrum; it's not present in the `SystemTB` datamodel.


- `/api/open/sysmap/get/stars/<systemid>`


# Decoding (from Json from Flask REST API)
This will be decoded in series on the front end in the order shown below (to ensure referential integrity):
1. Identify System
2. Get Stars in System
3. Position Stars around the System 'Barycenter' (distance from barycenter and angle from 'north')
4. Get Bodies in System (prefer direct children of a Star; a "*Major Body*")
5. Position Major Bodies around Stars us
6. For each body `B` (whose parent is a star);
   1. Get bodies whose parent is body `B`
   2. For each body in 