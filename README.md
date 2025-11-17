# DBA-Space-Tasks

#Check Space for EachDB on Server

EXEC sp_MSforeachdb 
'USE [?];
SELECT
    DB_NAME() AS DatabaseName,
    CONVERT(DECIMAL(10,2), SUM(size) * 8 / 1024) AS TotalSizeMB,
    CONVERT(DECIMAL(10,2), SUM(FILEPROPERTY(name, ''SpaceUsed'')) * 8 / 1024) AS UsedSpaceMB,
    CONVERT(DECIMAL(10,2), (SUM(size) - SUM(FILEPROPERTY(name, ''SpaceUsed''))) * 8 / 1024) AS FreeSpaceMB
FROM sys.database_files
GROUP BY type_desc;';
