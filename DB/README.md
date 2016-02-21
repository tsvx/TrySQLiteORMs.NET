# Test Databases
## ra.db3
Just one table.

`datetime`s below are in DateTime.Ticks so a connection string should include `DateTimeFormat=Ticks`.

	CREATE TABLE [rafile] (
		[FileId] INTEGER PRIMARY KEY AUTOINCREMENT,
		[FileName] text NOT NULL,
		[MD5] blob, -- 16 bytes of MD5SUM of the file.
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
93729 rows.
## PlanCache.ulog.db3
Five tables and one view.

`DATETIME`s below are in Unix Epoch (seconds) so a connection string should include `DateTimeFormat=UnixEpoch`.

	CREATE TABLE [uptype] (
		[upt] INT PRIMARY KEY NOT NULL,
		[upstr] TEXT NOT NULL
	);

Static table (dictionary), its data is:

upt | upstr  
----|-------
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
55866 rows.

	CREATE TABLE [errors] (
		[cid] INTEGER REFERENCES commands,
		[time] DATETIME NOT NULL,
		[last] DATETIME,
		[num] INT DEFAULT 1,
		[type] TEXT NOT NULL,
		[msg] TEXT NOT NULL,
		[desc] TEXT
	);
50 rows.

	CREATE TABLE [updates] (
		[uid] INTEGER PRIMARY KEY AUTOINCREMENT,
		[cid] REFERENCES commands NOT NULL,
		[sid] INT NOT NULL,
		[upt] REFERENCES uptype
	);
57427 rows.

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
315438 rows.

	CREATE VIEW print AS SELECT
		session_id, orbit,
		datetime(date,'unixepoch') AS start_date,
		time(end_date,'unixepoch') AS end_date,
		board, board_code, nip_name, nip_num, input, mode, priority,
		datetime(start_time,'unixepoch') AS start_time,
		time(end_time,'unixepoch') as end_time,
		datetime(real_time,'unixepoch') AS real_time
	FROM sskp_sessions
	WHERE mode <> 'ВП' AND nip_num > 0 AND board_code NOT NULL
81634 rows.
