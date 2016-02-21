# Test Databases

## RaFiles (`ra.db3`)

	CREATE TABLE [rafile] (
		[FileId] INTEGER PRIMARY KEY AUTOINCREMENT,
		[FileName] text NOT NULL,
		[MD5] blob,
		[Packets] bigint NOT NULL,
		[Bytes] bigint NOT NULL,
		[FirstTime] datetime NOT NULL,
		[LastTime] datetime,
		[TimeIsValid] bit NOT NULL,
		[SrcName] text,
		[SrcAddress] text NOT NULL,
		[SrcPort] int,
		[DstAddress] text NOT NULL,
		[DstPort] int,
		[Comment] text
	);

## PlanCacheUpdateLog (`PlanCache.ulog.db3`)

	CREATE TABLE [uptype] (
		[upt] INT PRIMARY KEY NOT NULL,
		[upstr] TEXT NOT NULL
	);

Static table (dictionary), its data is:
upt | upstr  
------------
0   | nothing
1   | equal  
2   | minor  
3   | show   
4   | hide   
5   | added  
6   | removed
7   | changed

	
	CREATE TABLE [commands] (
		[cid] INTEGER PRIMARY KEY AUTOINCREMENT,
		[time] DATETIME NOT NULL,
		[src] TEXT NOT NULL,
		[type] TEXT NOT NULL,
		[date] DATETIME,
		[sid] INT,
		[input] TEXT,
		[upt] INT REFERENCES uptype
	);

	CREATE TABLE [errors] (
		[cid] INTEGER REFERENCES commands,
		[time] DATETIME NOT NULL,
		[last] DATETIME,
		[num] INT DEFAULT 1,
		[type] TEXT NOT NULL,
		[msg] TEXT NOT NULL,
		[desc] TEXT
	);

	CREATE TABLE [updates] (
		[uid] INTEGER PRIMARY KEY AUTOINCREMENT,
		[cid] REFERENCES commands NOT NULL,
		[sid] INT NOT NULL,
		[upt] REFERENCES uptype
	);

	CREATE TABLE [new_sessions] (
		[uid] INTEGER REFERENCES updates NOT NULL,
		[ssid] INT,
		[date] DATETIME,
		[end_date] DATETIME,
		[orbit] INT,
		[board] TEXT,
		[board_code] SMALLINT,
		[nip_name] TEXT,
		[nip_num] INT,
		[input] SMALLINT,
		[mode] TEXT,
		[priority] SMALLINT,
		[start_time] DATETIME,
		[end_time] DATETIME,
		[real_time] DATETIME,
		[session_comment] TEXT,
		[board_comment] TEXT,
		[zone_comment] TEXT,
		[display] BOOLEAN
	);
