# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why join tables may be valuable.

  ```md
    They allow you to have many to many relationships between two tables
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    A Profile (given_name, surname, email) has many Movies (title, release_date, length) through Favorites (movie_id, profile_id)
    A Movie (title, release_date, length) is listed on many Profiles (given_name, surname, email)through Favorites (movie_id, profile_id)
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :movies, through :favorites
    has_many :favorites

    validates :given_name, presence: true
    validates :surname, presence: true
    validates :email, presence: true
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :profiles, through :favorites
    has_many :favorites

    validates :title, presence: true
    validates :release_date, presence: true
    validates :length, presence: true
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :movies
    belongs_to :favorites

    validates :movie_id, presence: true
    validates :profile_id, presence: true
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    it lists the attributes
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :given_name, :surname, :email, :movies

    def movies
      object.movies.pluck(:id)
    end
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    rails g scaffold favorite movie:references profile:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    it is a way to remove something that is dependent on another e.g. favorites is dependent of both movies and profiles so if you delete a profile or a movie the favorites table should be updated accordingly

  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    A gift-giver (one) will have a list of gift-receivers (many)
    A gift-receiver (one) will have lists of gift-ideas (many) for them
    Gift ideas (many) will have Occasion choices (many) through a list of occasions
  ```
