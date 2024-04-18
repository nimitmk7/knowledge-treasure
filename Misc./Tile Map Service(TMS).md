A Tile Map Service (TMS) provides access to cartographic maps of geo-referenced data, not direct access to the data itself. This document standardizes the way in which map tiles are requested by clients, and the ways that servers describe their holdings.

The Tiled Web Service provides access to resources, in particular, to rendered cartographic tiles at fixed scales. Access to these resources is provided via a "REST" interface, starting with a root resource describing available layers, then map resources with a set of scales, then scales holding sets of tiles

