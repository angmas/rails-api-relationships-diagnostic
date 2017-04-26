# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    They help manage the data for entities that have many to
    many relationships with each other.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
  profiles table with the following columns
  id
  user_name

  movies table with the following columns
  id
  movie_title

  favorites table with the following
  id

  A profile has many favorites.
  A favorite belongs to one user and belongs to one movie (represents
  the relationship between one user and one movie).
  A movie has mnay favorite relationships.

  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites, dependent: :destroy
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many :profiles, through: :favorites
  has_many :favorites, dependent: :destroy
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
  belongs_to :movie, inverse_of: :favorites
  belongs_to :profile, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
  The serializer controls the format of the returned data. It can act as a filter.

  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :user_name, :movies
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rails generate scaffold favorite profile:references movie:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
  this is the attribute that is assigned to a relationship macro to
  tell ruby to enforce relational dependencies. In other words, in
  the case of movies, profiles, and favorites, this attribute will tell Ruby to delete all favorite records that have the foreign key that
  matches the table record being deleted. So if a profile were deleted,
  all corresponding favorite records will also be deleted. If a movie
  record were deleted, then all corresponding favorite records will
  also be deleted.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
  A doctor, patient, appointment model would have these relationships.

  The patient would have one doctor who is designated their primary
  care physician. A patient would have one primary care physician. A doctor can be the primary care physician for many patients.

  A doctor can have many appointmentss with many patients who do not have a primary care physician relationship with them. A patient can have many appointments with many doctors who are not the patient's primary care physician.
  ```
