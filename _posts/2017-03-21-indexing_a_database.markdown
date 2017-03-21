---
layout: post
title:  "Indexing a Database"
date:   2017-03-21 00:42:13 -0400
---


## What is an Index?

When searching through a table in a database using a sql statement like

> Select * from user where name = “bob” 
 
You could imagine that a database would search through every row of data in the table and find all matches. Imagine having a table of millions of rows of data, this would be an exhaustive time consuming process each type a simple where clause query is called. This is known as a full table scan and not a very efficient way to search. So how do SQL databases handle requests like these efficiently?

Enter Indexing. Indexing creates data structures on a column of a table so that queries can be performed much faster. An index is a data structure that contains keys which have pointers that point to a row on the table. A good way to think about an index is to think about an index in the a book. A book can be thought of as the table and at the back of the book you have an index which tells you where you can find information on specific words through out the book. The index is sorted alphabetically, so rather than scanning the whole book to find a word, all you have to do is go to the index, find the word which is sorted alphabetically and it contains all pages you can find this word. 


## Why are Indexes Useful?

Indexes are simply data structures, that are ordered on a specific column of data. Indexes help when: selecting for a certain value on the indexed column as well as updating and deleting, as the latter two require finding the specific row to delete or update first. Indexes also helps sort on a specific indexed column, since the data structure already keeps the data ordered. There are however a few disadvantages to indexing. Some indexes (non-clustered) require additional disk space. Also, even though indexes help locate data while updating and deleting, they can actually require more processing time if multiple index data structure need to be updated. 

## How do Indexes work?

The most common types of index data structures are B-trees. B-trees are self balancing tree data structures that are similar to binary tree searches. They perform all actions in a logarithmic time and can be sorted. 


![](http://use-the-index-luke.com/static/fig01_02_tree_structure.en.LAuPsMu2.png)

A B-tree structure is made up of root, branch, and leaf nodes. Root node entries correspond to the highest branch node entries, and the highest branch node entries correspond to the highest leaf node enteries. The leaf node entries contain pointers to the rows of date that have that value. The size of the node correspond to the size of a disc block and the depth of tree is the logramithic function of the node size and the # entries.

![](http://use-the-index-luke.com/static/fig01_03_tree_traversal.en.RJRAetvl.png)

As seen in the diagram, traversing a B-tree is very efficient. It starts at the root node and moves in ascending order until it finds a value greater than or equal to itself. It then follows that node to the next node and so forth until it finds the value it is looking for with pointers to the rows of data that will be returned. See a more detailed description of this as well as how exactly inserting works at: [Geeks for Geeks](http://www.geeksforgeeks.org/b-tree-set-1-introduction-2/). 

Other data structures that are less common, but are available include hash tables, r-trees, and bitmap indexes. Hash tables store the column value as a key and the  value of the key would be pointers to the row of data. Hash tables are really good for looking up a specific column value, but they are not sorted so hash tables are not flexible for other queries. 
[R-trees](https://en.wikipedia.org/wiki/R-tree)  are primarly used for spatial data like Google Maps and [Bitmap indexes](https://en.wikipedia.org/wiki/Bitmap_index)  for low complexity data like boolean values.

There is one other important note about Indexes in a table. There are actually 2 types of indexes: clustered and non-clustered indexes. The primary difference between a clustered index and a non-clustered index is that a clustered index actually stores the data in the leaf node rather than a pointer. This means that while clustered indexes can be faster to search, there can only be one clustered per table.


## Implementing an Index (SQL and Rails)

In SQL and Rails, your primary id for a table is automatically a clustered index. In SQL you can override this by creating a table without a primary key and then adding a clustered index to any column.

```
CREATE TABLE dbo.User (
    userID        INT NOT NULL,
    name   VARCHAR(255) NULL,
    DOB          DATETIME NULL)
 
-- Creating the Clustered Index separately on an other column:
CREATE CLUSTERED INDEX [CI_User_name] ON dbo.User(name ASC)
```

Rails has a lot of magic going on and is generally not adviced to change a clustered index away from the primary key. For nonclustered indexes in SQL:

```
CREATE INDEX idx_name
ON Persons (name);
```

Or directly in the Create Table Statement:

```

CREATE TABLE dbo.User (
    ID        INT NOT NULL PRIMARY KEY,
    name   VARCHAR(255) NULL,
    DOB          DATETIME NULL,
		INDEX (name))

```

In Rails you can add an index to your migration by: 

```
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :name, :null => false
			t.date :dob
    end
		
		add_index :users, :name
  end
end

```


More Resources:
[Use the Index, Luke](http://use-the-index-luke.com/sql/anatomy/the-tree)

