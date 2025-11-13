# Assignment 4 - Columnar Databases and HBase

## 7in7 - HBase - Day 1

### Part 1 - Interactive Reading

Read Day 1 and work through the examples in the chapter.
Save your final database in a directory `day1` as follows.

```
./save.bash wiki day1
```

You can use the same script to save your database between work sessions.
But you cannot save over an existing save. So use temporary names like:
`day1a`, `day1b`, etc. You can use the `./load.bash` script to restore
an empty database from a saved state.

Here are some additional tips:

* Skip **Configuring HBase**.
* Begin with the last command on p57: `version`.
* p59 - Do not run these in the shell, which is Ruby.
    These are illustrative examples in python.
* p64 - The last example is split over 2 lines. `hbase>` and `hbase*` are
    not part of the command.
* I have not been able to use the colons inside of quotes in the hbase commands. If they don't work with the colon, try them without.
* The command to allow unlimited  versions is:

`alter 'wiki', {NAME => 'text', VERSIONS => 2147483647 }`

### Part 2 - 7in7 - HBase Day 1 - Find

1. Figure our how to use the shell to do the following:

    * Delete individual column values in a row:


        delete 'wiki', 'Home', 'revision:comment'



    * Delete an entire row

        ```
        deleteall 'wiki', 'Home'


        ```


2. Bookmark the HBase API documentation for the version of HBase you’re using.

    ```
    https://hbase.apache.org/devapidocs/

    ```

### Part 3 - Create a family database

#### Step 1 - Create an hbase table that represents a family.

Specifically, you should have column families for the following:
* personal information: names of family members and birthdays
* favorites: foods and vacation locations
* location information: addresses including street, city, state, and zip. and phone numbers

Place the Hbase code to create the families below:
```
create 'family', 'personal', 'favorites', 'location'

```

#### Step 2 - Load five rows of data.
Make sure to have at least one row with more than one favorite food and at least one row with more than one favorite vacation location. Place the Hbase code below:
```
put 'family', 'member1', 'personal:name', 'John'
put 'family', 'member1', 'personal:birthday', '1990-01-15'
put 'family', 'member1', 'favorites:food', 'Pizza, Pasta'
put 'family', 'member1', 'favorites:vacation', 'Hawaii'
put 'family', 'location:address', '123 Elm St, Boston, MA'
put 'family', 'location:phone', '617-555-1111'

put 'family', 'member2', 'personal:name', 'Alice'
put 'family', 'member2', 'personal:birthday', '1995-03-20'
put 'family', 'favorites:food', 'Sushi'
put 'family', 'favorites:vacation', 'Paris, London'
put 'family', 'location:address', '456 Pine St, Springfield, MA'
put 'family', 'location:phone', '413-555-2222'

put 'family', 'member3', 'personal:name', 'David'
put 'family', 'personal:birthday', '2000-07-10'
put 'family', 'favorites:food', 'Burgers'
put 'family', 'favorites:vacation', 'New York'
put 'family', 'location:address', '789 Maple Ave, Boston, MA'
put 'family', 'location:phone', '617-555-3333'

put 'family', 'member4', 'personal:name', 'Emma'
put 'family', 'personal:birthday', '1988-11-02'
put 'family', 'favorites:food', 'Pizza'
put 'family', 'favorites:vacation', 'Hawaii, Florida'
put 'family', 'location:address', '101 Oak St, Hartford, CT'
put 'family', 'location:phone', '860-555-4444'

put 'family', 'member5', 'personal:name', 'Sophia'
put 'family', 'personal:birthday', '1998-05-23'
put 'family', 'favorites:food', 'Tacos, Pizza'
put 'family', 'favorites:vacation', 'New York'
put 'family', 'location:address', '222 Birch Rd, Boston, MA'
put 'family', 'location:phone', '617-555-5555'

```

#### Step 3 – Create HBase queries for the items below.

Place the Hbase code **and the results** after each query.

**Query 1:** Get complete information for a specific family member.
```
get 'family', 'member1'

```

**Query 2:** View only personal information for all family members.
```
scan 'family', {COLUMNS => 'personal'}

```

**Query 3:** Get the name, favorite foods, and vacation locations for one family member.
```
get 'family', 'member2', ['personal:name', 'favorites:food', 'favorites:vacation']
get 'family', 'member2', ['personal:name', 'favorites:food', 'favorites:vacation']



**Query 4:** Get a range of at least two family members.
```
scan 'family', {STARTROW => 'member2', STOPROW => 'member4'}

```

**Query 5:** Get the addresses for all family members.
```
scan 'family', {COLUMNS => 'location:address'}

```

**Query 6:** Get the names of family members who like a specific favorite food (e.g., pizza).
```
scan 'family', {FILTER => "ValueFilter(=, 'substring:PIZZA')"}

```

**Query 7:** Create a vacation preference list with names.
```
scan 'family', {COLUMNS => ['personal:name', 'favorites:vacation']}

```

### Part 4 - 7in7 - Day 3 - Wrap Up

Read Day 3 - Wrap Up. Then answer the following.

1. List the pros of HBase as described in our text.

    ```
    1. Highly scalable for big data workloads.
2. Provides strong consistency and atomic operations at the row level.
3. Built-in versioning and timestamped data storage.
4. Supports compression, garbage collection, and in-memory tables.
5. Integrates seamlessly with Hadoop ecosystem (Hive, Pig, etc.).
6. Ideal for large-scale analytics and logging systems.

    ```

2. List the cons of HBase as described in our text.

    ```
    1. Complex setup and configuration — not beginner-friendly.
2. Does not scale down well; requires multiple nodes for efficiency.
3. Slower for small datasets or low-latency transactional workloads.
4. Limited documentation; often requires reading API or source code.
5. Not a full relational system — no joins or schema enforcement.

    ```


### 7in7 - Day 2 - OPTIONAL

OPTIONAL - You may safely skip Day 2.

This section contains a code-heavy example of loading a large amount
of data into your HBase database. If you feel like a challenge and are
interested, feel free to work through it.

If you do this section, please **do not** use ./save.bash to save
your database, because it may become very large.

So if you are ready for the challenge, below are some instructions to
help you along. Good luck!

1. Download and extract the source code for the text: https://pragprog.com/titles/pwrdata/seven-databases-in-seven-weeks-second-edition/

2. Drag the following files into 02_hbase/local/scripts on GitPod.
    * source_code/02_hbase/import_from_wikipedia.rb
    * source_code/02_hbase/create_wiki_schema.rb

3. Start your database.

4. Run the following to create the wiki table.

    ```
    ./shell.bash create_wiki_schema.rb
    ```

5. Now you should be able to run the command below.
    BEFORE YOU DO... be ready to press CTRL+C to stop the process. This
    command will load a lot of data very fast.

    ```
    curl https://dumps.wikimedia.org/enwiki/latest/enwiki-latest-pages-articles.xml.bz2 | bzcat | ./shell.bash import_from_wikipedia.rb
    ```

6. After it appears to be working, press CTRL+C to stop it.

7. Connect to your database.

8. Run the command below to count the number of
    rows in your 'wiki' table.

    ```
    count 'wiki'
    ```

    Copy and paste the output of this command below.

9. Run the command below to get information about your database's regions.

    ```
    scan 'hbase:meta',{FILTER=>"PrefixFilter('wiki')"}
    ```

    Copy and past the output of this command below.
