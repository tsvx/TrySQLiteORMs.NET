# TrySQLiteORMs.NET
Trying out ORM libs for working with SQLite on .NET Framework.

The purpose of this repo is to find out SQLite .NET ORM library that is appropriate for me.
I need it to be simple, fast, "code first" and, optionally but desirable, to have the ability to generate
DTO classes from an existing database.

My working environment is VS2012 on Win7 x64. For simplicity I will make projects targeting .NET 4.5,
though .NET 4.0 version is interesting to me too.

What is to research:

- [ ] [EntityFramework6](https://msdn.microsoft.com/en-us/data/ee712907.aspx)
- [ ] [ServiceStack.OrmLite](https://github.com/ServiceStack/ServiceStack.OrmLite)
- [ ] [sqlite-net](https://github.com/praeclarum/sqlite-net)
- [ ] [Dapper](http://code.google.com/p/dapper-dot-net/)
- [ ] [PetaPoco](http://www.toptensoftware.com/petapoco/)
- [ ] [Massive](https://github.com/robconery/massive)
- [ ] [Simple.Data](https://github.com/markrendle/Simple.Data)

Two [test databases](DB/README.md) are included.

First, let's make use of the official SQLite .NET wrapper -- [System.Data.SQLite](https://system.data.sqlite.org/).

## NoORM_SystemDataSQLite
Just simple querying with System.Data.SQLite.