---
layout: post
post_author: Tim Mecklem
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Using Ecto's Virtual Fields with select_merge
publish_date: 2018-09-20 13:45:00 +0000
feature_post_image: ''
post_images: []
slug: using-ectos-virtual-fields-with-select-merge

---
## Using Ecto's virtual fields with `select_merge`

You may know that Ecto has a "virtual" field type that you can use to include pretty much anything in an Ecto struct. But did you know that you can also populate a virtual field directly in an Ecto Query?

Say I have a `users` table, and joined to that I have a `posts` table. The `User` has_many `Posts`. The User schema looks like this:

```elixir
  schema "users" do
    field :name, :string
    
    has_many :posts, Post
  end
```

And the Post schema looks like this:

```elixir
  schema "posts" do
    field :date, :date
    
    belongs_to :user, User
  end
```

And let's say that in this app I'm writing, the date of a user's first post is important and displayed various places. Today I learned that I can map the first post's date directly onto the User struct. To do this, we have to add a virtual field to User, so that it looks like this:

```elixir
  schema "users" do
    field :name, :string
    field :first_post_date, :date, virtual: true

    has_many :posts, Post

    timestamps()
  end
```

Then, I can use two awesome Ecto features to select the first post date and merge it into the `User` struct. Here's an Ecto query to do just that!

```elixir
    Repo.all(
      from(
        user in User,
        left_join: post in subquery(from p in Post, order_by: :date, limit: 1),
        on: post.user_id == user.id,
                select_merge: %{first_post_date: post.date}
      )
    )
  end
```

The result is a list of `User` structs where the first_post date is populated (or is nil if the user hasn't posted yet). 

```elixir
[
  User{
    first_post_date: ~D[2011-01-05],
    id: 2,
    name: "Tim",
    ...
  },
  User{
    first_post_date: nil,
    id: 3,
    name: "Lurker McLurkerFace",
    ...
  }
]
```

### What strange magic happened here

The key components of Ecto that enable this are the following:

* The virtual `first_post_date` field on User
* The `left_join: post in subquery(from p in Post, order_by: :date, limit: 1)` that joins the user's first post if it exists
* The `select_merge: %{first_post_date: post.date}` that merges in the `first_post_date` value to the `User` struct.

What are other ways I could do this? 

In Ecto:

* I could preload the posts and have a function to select the first post's date and merge it in separately.
* I could select the first post date via a separate Ecto Query.

In the database:

* I could put a `first_post_date` in the `users` table, and have a trigger that updates the field.
* I could have a database view that contains first post using a similar join as above.

The cool thing about the solution above is that if I do choose to something in the database later, the only things that need to change are the field definition and the Ecto Query. The rest of the application can just keep using `first_post_date` like always!