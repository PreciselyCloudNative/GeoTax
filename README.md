# Vertex Vintage  
## Defined in Scala described in Python

### Appendix
| Title                                                                            |
|----------------------------------------------------------------------------------|
| 1. [TAID change for boundary codes that have not changed](#taid-change)          | 
| 2. [New boundary codes](#new-boundary-codes)                                     |
| 3. [Old boundary codes](#old-boundary-codes)                                     |
| 4. [Old TAID](#old-taid)                                                         |
| 5. [New TAID](#new-taid)                                                         |





## Vertex Vintage Difference

###### This process compares and identifies differences between current Vertex file & previous Vertex file.
1. Load VTOseq file into table naming it after the version release.  Create a 73 character record – in this example named concat as this data format is fixed width.
2. The column lists concatenated State FIPS, County FIPS, City GNIS, 10 SPD codes, and TAID.  We will refer to FIPS+GNIS+SPD as 'boundary codes'.
          
# SQL Queries  
## [TAID change for boundary codes that have not changed](#taid-change) 

```
SELECT DISTINCT
    LEFT(vtoseq0623.concat, 64) AS boundarycode,
    RIGHT(vtoseq0623.concat, 9) AS newTAID,
    RIGHT(vtoseq0523.concat, 9) AS oldTAID
FROM
    vtoseq0623
LEFT JOIN
    vtoseq0523
ON
    LEFT(vtoseq0623.concat, 64) = LEFT(vtoseq0523.concat, 64)
WHERE
    RIGHT(vtoseq0523.concat, 9) <> RIGHT(vtoseq0623.concat, 9);
```

## [New boundary codes](#new-boundary-codes)  

```
SELECT LEFT(vtoseq0623.concat, 64) AS boundarycode, RIGHT(vtoseq0623.concat, 9) AS newTAID
FROM vtoseq0623
LEFT JOIN vtoseq0523 ON LEFT(vtoseq0623.concat, 64) = LEFT(vtoseq0523.concat, 64)
WHERE vtoseq0523.concat IS NULL;
```

## [Old boundary codes](#old-boundary-codes) 

```
SELECT 
    LEFT(vtoseq0523.concat, 64) AS boundarycode,
    RIGHT(vtoseq0523.concat, 9) AS oldTAID 
FROM 
    vtoseq0623 
RIGHT JOIN 
    vtoseq0523 
ON 
    LEFT(vtoseq0623.concat, 64) = LEFT(vtoseq0523.concat, 64) 
WHERE 
    vtoseq0623.concat IS NULL;
```

## [Old TAID](#old-taid)

```
SELECT DISTINCT RIGHT(vtoseq0523.concat, 9) AS oldTAID
FROM vtoseq0523
LEFT JOIN vtoseq0623
ON RIGHT(vtoseq0523.concat, 9) = RIGHT(vtoseq0623.concat, 9)
WHERE RIGHT(vtoseq0623.concat, 9) IS NULL;
```

## [New TAID](#new-taid) 

```
SELECT DISTINCT RIGHT(vtoseq0623.concat, 9) AS newTAID
FROM vtoseq0523
RIGHT JOIN vtoseq0623 ON RIGHT(vtoseq0523.concat, 9) = RIGHT(vtoseq0623.concat, 9)
WHERE RIGHT(vtoseq0523.concat, 9) IS NULL;
```
