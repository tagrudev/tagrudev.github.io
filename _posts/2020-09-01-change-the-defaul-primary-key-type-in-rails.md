---
title:  "Change the default primary key type in Rails"
date: 2020-09-01 08:00
---

Rails had changed the default type of the primary key from `int` to `big int` when creating a new table - [Change Default Primary Keys to BIGINT #26266](https://github.com/rails/rails/pull/26266){:target="blank"}, but unless you are working with a big set of records (bigger than 2^31) it's a waste of resources.

You can control that in your migration by passing `id: false` to the `create_table` method and then adding a `primary_key` column with you desired type ex:

```ruby
  create_table :exercises, id: false do |t|
    t.primary_key :id, :serial
    ...
  end
```

In this case we are going to replace the default `big int` with a `serial` which is PostgreSQL specific . Serial is the same as integer except that PostgreSQL will automatically generate and populate values into the SERIAL column. This is similar to AUTO_INCREMENT column in MySQL or AUTOINCREMENT column in SQLite.

