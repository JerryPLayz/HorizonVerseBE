
# API Specification
- All requests to the API endpoints shall be pointed via `/api/*`.
- The API is split between globally accessible functions and those which are secured. These are:
  - `/api/open/*`
  - `/api/secure/*`
- The **secure** endpoint shall be further divided (if necessary) into: 
  - `/api/secure/admin/*` for server / service administration outside standard practice (if implemented)
  - `/api/secure/mapmaker/*` for mapmaker system functionality (if implemented)

- The API shall divide by **module**, representing a series or group of endpoints with similar functionality or purpose.
- Each module shall be defined in two parts: one for **open** and **secure**, as the need arises. When an endpoint is secure, it will be highlighted.

# 1 General
## 1.1 Search
The Search functionality allows users to search for content across the map. (Stars, planets {by name or default}, outpost stations or even entire nations or parts thereof)

This presumes that such functionality has been implemented.

### 1.1.1 Open Endpoints
- GET `/api/open/general/search/<terms>`

Takes generalized search terms and returns a list of possible matches across the database.
The information returned in this search is not authoritative, and may rely on the `/query` extension for contextualizing reqests.
```json
{
  "status": 200,
  "message": "OK",
  "search": {
    "hex_1234": {"type": "star", "preferred_name": "Beta Eridani"},
    "hex_1234a": {"type": "body", "preferred_name": "Beta Eridani A"}
  }
}
```
- GET `/api/open/general/query/<identifier>`

Queries the backend for an object with a specific identifier (always marked 'hex_1234*' in this documentation)
Will return a variable object.
```json
vgg
```


# Galaxy Map Module
- This module 


