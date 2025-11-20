---
layout: post
title: "PostgreSQL stores procedures differently. Also, geopoints."
tags: [research, database]
---

## Changing engines  
### From T-SQL to PL/pgSQL  
Coming from SQL Server, I already knew the general idea of stored procedures — inputs, logic, output.  
But PostgreSQL approaches the concept differently, and this small geo-filtering procedure became my crash course in **PL/pgSQL**, **PostGIS**, and Postgres’ way of structuring executable logic.

This article is short and focused on what changed, what I had to relearn, and what PL/pgSQL brings to the table.

---

## What I learned  
### 1. PostgreSQL functions ≠ MSSQL procedures  
In SQL Server, stored procedures and functions are clearly separated.  
In PostgreSQL, the line is fuzzier:  
- “Procedures” exist, but they **cannot return rows**.  
- Most logic is done through **functions**, especially those returning tables.  

So the practical mindset shift was:  
> *In Postgres, a “stored procedure” that returns data is almost always a **function**.*

---

### 2. PL/pgSQL’s structure is closer to a scripting language  
Instead of T-SQL’s strict block style, PL/pgSQL feels more like lightweight scripting:
- Variables declared in a `DECLARE` block  
```sql
DECLARE
	-- Set input point to compare against
	input_geog geography(Point,4326) := ST_SetSRID(ST_MakePoint(in_lng, in_lat), 4326)::geography;
```
- `RETURN QUERY` to stream result sets  
```sql
	RETURN QUERY
	WITH aabb AS ( ... )
```
- Exceptions raised with simple `RAISE EXCEPTION` statements  
```sql
	IF x OR y THEN
		RAISE EXCEPTION 'z';
	END IF;
```
- `BEGIN … END` wrapping the logic  
```sql
BEGIN
	IF x OR y THEN
		RAISE EXCEPTION 'z';
	END IF;

	RETURN QUERY
	WITH aabb AS ( ... )
	SELECT h, i, j
	FROM aabb AS ab
	WHERE cc
	ORDER BY "h";
END;
```

It’s clean and flexible!

---

### 3. PostGIS changes everything  
PostgreSQL doesn’t ship with native geospatial types.  
Instead, PostGIS is installed to add a fully-featured set of spatial functions — and the learning curve is worth it.

Using functions like `ST_SetSRID`, `ST_MakePoint`, `ST_DWithin`, and `ST_Distance` make the query compact and readable.
```sql
DECLARE
	-- Set input point to compare against
	input_geopoint geography(Point,4326) := ST_SetSRID(ST_MakePoint(in_lng, in_lat), 4326)::geography;
BEGIN
	...
	RETURN QUERY
	WITH tbl_geopoint AS (
		SELECT
			tbl.*,
			ST_SetSRID(ST_MakePoint(tbl."Lng", tbl."Lat"), 4326)::geography AS tbl_geopoint
			FROM "Table" AS tbl
		)
	SELECT 	-- Which columns we want to extract
		...
		-- Calculate distance between input and stored positions
		ROUND( 
			(ST_Distance(tbl.tbl_geopoint, input_geopoint) / 1000.0)::numeric,
			1 	-- Number of decimals rounded to
		)::double precision AS "DistanceKm" -- Cast rounding to numeric and back for safety
	FROM tbl_geog AS tbl
	WHERE ST_DWithin(tbl.tbl_geopoint, input_geopoint, input_radius_m)
```
Look, I know this is a doosy, but here are the basics: 
(1) input_geopoint: Input lat/lng becomes a geographical point.
(2) tbl_geopoint: Set up a pull from datatable "Table", and set each of the lat/lng to a new datapoint.
(3) DistanceKm: ST_Distance the two points against each other, get output in m, convert to km.
(4) ST_DWithin to find out if the tbl_geopoint is within input_radius_m of input_geopoint. 

My main takeaway:  
> *PostGIS is extremely powerful, making distance measurements damn near trivial.*

---

## The interesting bit  
If I had to highlight *one* piece of functionality, it would be:  
```sql
	WITH hd_geog AS (
		SELECT
			hd.*,
			ST_SetSRID(ST_MakePoint(hd."Lng", hd."Lat"), 4326)::geography AS hd_geog
			FROM "Hairdressers" AS hd
		)
```
This sets up the Hairdresser geographical point from stored lat-/longitude values, which lets us avoid a whole host of other potentially necessary data, since we don't need an address anywhere, just a distance.

Wrapping up

What I gained from this:

A clearer understanding that Postgres prefers functions for returning data

Comfort with PL/pgSQL’s lightweight structure

A practical usecase for PostGIS and geographic querying

This was a compact but surprisingly dense learning session — and now the database layer is finally ready for real use.

Next topic awaits.