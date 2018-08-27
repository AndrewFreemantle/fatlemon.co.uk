---
title: 'Foreign Keys to custom Primary Key caveats in Ruby on Rails'
author: Andrew Freemantle
layout: post
permalink: /2018/08/foreign-keys-to-custom-primary-key-caveats-in-ruby-on-rails/
tags:
  - Rails
  - Rails 5
  - Foreign Keys
---

![Image Credit: S Egger, 2007]({{ site.imageurl }}2018/keys-header.png)

The Ruby on Rails model convention of automatically including primary keys named `id` and foreign keys named `{primary_key_model}_id` works well for the vast majority of models, but what if the object we're modelling already has a unique numerical property? It would make sense to use this property instead of `id`, and Rails allows custom primary keys but there are some gotchas..

Let's walk through an example using `Books` and `Chapters`

First of all we need a new Rails app which we'll call `Bookshelf`: <span class="text-muted">*(I'm using Rails 5.2.1 here)*</span>  
<i class="fa fa-terminal"></i>`rails new bookshelf`  
<i class="fa fa-terminal"></i>`cd bookshelf`  
<i class="fa fa-terminal"></i>`rails db:create`  

Now we can create our `Book` model - but instead of `id` we'll specify [the International Standard Book Number (ISBN)](https://en.wikipedia.org/wiki/International_Standard_Book_Number) as the primary key [^1]  
<i class="fa fa-terminal"></i>`rails generate model Book isbn:integer title:string genre:string`

We can't specify the primary key change with the command-line generator so we need to edit the migration Rails created for us, from this:  
``` ruby
# db/migrate/20180825101955_create_books.rb
class CreateBooks < ActiveRecord::Migration[5.2]
  def change
    create_table :books do |t|
      t.integer :isbn
      t.string :title
      t.string :genre

      t.timestamps
    end
  end
end
```
to this:
``` ruby
# db/migrate/20180825101955_create_books.rb
class CreateBooks < ActiveRecord::Migration[5.2]
  def change
    create_table :books, id: false, primary_key: :isbn do |t|
      t.primary_key :isbn
      t.string :title
      t.string :genre

      t.timestamps
    end
  end
end
```

Then we can apply it and check the primary key is as expected by creating a `Book`:  
<i class="fa fa-terminal"></i>`rails db:migrate`  
``` bash
== 20180825101955 CreateBooks: migrating ======================================
-- create_table(:books, {:id=>false, :primary_key=>:isbn})
   -> 0.0024s
== 20180825101955 CreateBooks: migrated (0.0025s) =============================
```
<i class="fa fa-terminal"></i>`rails c`  
<i class="fa fa-terminal"><span>irb(main):001:0></span></i>`Book.create(isbn: 9780099518471, title: 'Brave New World', genre: 'Science Fiction')`  
<i class="fa fa-terminal"><span>irb(main):002:0></span></i>`Book.find(9780099518471)`  
``` ruby
  Book Load (0.3ms)  SELECT  "books".* FROM "books" WHERE "books"."isbn" = ? LIMIT ?  [["isbn", 9780099518471], ["LIMIT", 1]]
=> #<Book isbn: 9780099518471, title: "Brave New World", genre: "Science Fiction", created_at: "2018-08-25 10:36:37", updated_at: "2018-08-25 10:36:37">
```

So far so good. Now let's create our `Chapter` association: <span class="text-muted">*(type `quit` to exit `rails c`)*</span>  
<i class="fa fa-terminal"></i>`rails generate model Chapter title:string no:integer book:references`  

<div class="panel panel-warning">
  <div class="panel-body bg-warning">
    <i class="fa fa-exclamation fa-border text-danger"></i> If we run the <code>Chapter</code> migration now it will apply, but if we try to create a <code>Chapter</code> and reference our <code>Book</code> we get a <span class="text-danger">rollback transaction</span> error:<br/>
    <i class="fa fa-terminal"></i><code>rails db:migrate</code><br/>
    <i class="fa fa-terminal"></i><code>rails c</code><br/>
    <i class="fa fa-terminal"><span>irb(main):001:0></span></i><code>Chapter.create!(title: "Chapter 1", no: 1, book: Book.find(9780099518471))</code><br/>
    <pre><code class="language-ruby">  Book Load (0.5ms)  SELECT  "books".* FROM "books" WHERE "books"."isbn" = ? LIMIT ?  [["isbn", 9780099518471], ["LIMIT", 1]]
   (0.1ms)  begin transaction
  Chapter Create (0.7ms)  INSERT INTO "chapters" ("title", "no", "book_id", "created_at", "updated_at") VALUES (?, ?, ?, ?, ?)  [["title", "Chapter 1"], ["no", 1], ["book_id", 9780099518471], ["created_at", "2018-08-25 11:26:44.984648"], ["updated_at", "2018-08-25 11:26:44.984648"]]
   (0.0ms)  rollback transaction
ActiveRecord::StatementInvalid: SQLite3::SQLException: foreign key mismatch - "chapters" referencing "books": INSERT INTO "chapters" ("title", "no", "book_id", "created_at", "updated_at") VALUES (?, ?, ?, ?, ?)</code></pre>
    There's no way to create a <code>Chapter</code> because the foreign key constraint is trying to enforce values against <code>books.id</code> which doesn't exist so we will never get a foreign key match
  </div>
</div>

There are a couple of manual steps which are pointed to in [the foreign key documentation](https://guides.rubyonrails.org/active_record_migrations.html#foreign-keys) to get this working properly:

> If the column names can not be derived from the table names, you can use the `:column` and `:primary_key` options

Let's throw in a little complication at this point - we don't want our foreign key column name to be `chapters.book_id` - we'd like it to be `chapters.book_isbn` as that's in keeping with the `Books` model and looks better in the database too.

We need to make a few changes to our `Chapters` migration, from this:  
``` ruby
# db/migrate/20180825111413_create_chapters.rb
class CreateChapters < ActiveRecord::Migration[5.2]
  def change
    create_table :chapters do |t|
      t.string :title
      t.integer :no
      t.references :book, foreign_key: true

      t.timestamps
    end
  end
end
```
to this:
``` ruby
# db/migrate/20180825111413_create_chapters.rb
class CreateChapters < ActiveRecord::Migration[5.2]
  def change
    create_table :chapters do |t|
      t.string :title
      t.integer :no
      t.references :book_isbn, references: :books, null: false # creates 'book_isbn_id'

      t.timestamps
    end

    rename_column :chapters, :book_isbn_id, :book_isbn
    add_foreign_key :chapters, :books, column: 'book_isbn', primary_key: 'isbn'
  end
end
```

As you can see we're taking the default naming convention for the foreign key references column, renaming it, and then adding the foreign key constraint with the `column:` and `primary_key:` options as per the documentation.

Now we can apply our migration:  
<i class="fa fa-terminal"></i>`rails db:migrate`  
``` bash
== 20180825111413 CreateChapters: migrating ===================================
-- create_table(:chapters)
   -> 0.0032s
-- rename_column(:chapters, :book_isbn_id, :book_isbn)
   -> 0.0328s
-- add_foreign_key(:chapters, :books, {:column=>"book_isbn", :primary_key=>"isbn"})
   -> 0.0000s
== 20180825111413 CreateChapters: migrated (0.0366s) ==========================
```

<div class="panel panel-warning">
  <div class="panel-body bg-warning">
    <i class="fa fa-exclamation fa-border text-danger"></i> If we try to create a Chapter and reference our Book now we get a <span class="text-danger">missing attribute</span> error:<br/>
    <i class="fa fa-terminal"></i><code>rails c</code><br/>
    <i class="fa fa-terminal"><span>irb(main):001:0></span></i><code>Chapter.create!(title: "Chapter 1", no: 1, book: Book.find(9780099518471))</code><br/>
    <pre><code class="language-ruby">  Book Load (0.2ms)  SELECT  "books".* FROM "books" WHERE "books"."isbn" = ? LIMIT ?  [["isbn", 9780099518471], ["LIMIT", 1]]
ActiveModel::MissingAttributeError: can't write unknown attribute `book_id`</code></pre>
    We still can't create a <code>Chapter</code> because Rails doesn't know we're using custom column and primary key names - the error message is telling us there's a problem with our <code>Chapter</code> model
  </div>
</div>

The final peice is to tell Rails about our custom primary key and while we're editing the model, we can add our associations at the same time.  
Change the `Book` from this:
``` ruby
# app/models/book.rb
class Book < ApplicationRecord
end
```
to this:
``` ruby
# app/models/book.rb
class Book < ApplicationRecord
  self.primary_key = 'isbn'
  has_many :chapters, primary_key: 'isbn', foreign_key: 'book_isbn'
end
```

And then change the `Chapter` from this:
``` ruby
# app/models/chapter.rb
class Chapter < ApplicationRecord
  belongs_to :book
end
```
to this:
``` ruby
# app/models/chapter.rb
class Chapter < ApplicationRecord
  belongs_to :book, foreign_key: 'book_isbn'
end
```

Now let's try adding a Chapter or two..  

<i class="fa fa-terminal"></i>`rails c`  
<i class="fa fa-terminal"><span>irb(main):001:0></span></i>`Chapter.create!(title: "Chapter 1", no: 1, book: Book.find(9780099518471))`  
``` ruby
  Book Load (0.5ms)  SELECT  "books".* FROM "books" WHERE "books"."isbn" = ? LIMIT ?  [["isbn", 9780099518471], ["LIMIT", 1]]
   (0.1ms)  begin transaction
  Chapter Create (1.8ms)  INSERT INTO "chapters" ("title", "no", "book_isbn", "created_at", "updated_at") VALUES (?, ?, ?, ?, ?)  [["title", "Chapter 1"], ["no", 1], ["book_isbn", 9780099518471], ["created_at", "2018-08-27 07:29:24.381030"], ["updated_at", "2018-08-27 07:29:24.381030"]]
   (4.0ms)  commit transaction
=> #<Chapter id: 1, title: "Chapter 1", no: 1, book_isbn: 9780099518471, created_at: "2018-08-27 07:29:24", updated_at: "2018-08-27 07:29:24">
```
Success!  

And because we've added the associations, we can also do:  
<i class="fa fa-terminal"><span>irb(main):001:0></span></i>`bravenewworld = Book.find(9780099518471)`  
<i class="fa fa-terminal"><span>irb(main):002:0></span></i>`bravenewworld.chapters.create!(title: "Chapter 2", no: 2)`  

And to show the association works the other way..  
<i class="fa fa-terminal"><span>irb(main):003:0></span></i>`chapter2 = Chapter.where(book_isbn: 9780099518471, no: 2).first`  
<i class="fa fa-terminal"><span>irb(main):004:0></span></i>`chapter2.book`  

<br/>
<hr/>

[^1]: In reality ISBN probably isn't a good choice for a primary key as the specification states the 10-digit versions can start with a zero, and leading zeros are dropped by integer datatypes.