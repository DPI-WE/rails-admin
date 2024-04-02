# Admin 🧑‍💼

[Lecture Video](https://youtu.be/GBTeAbqeC14)

Now that you've deployed your application, you'll need a way to monitor and administrate the data. This makes it easy to CRUD any record without needing to build a route/controller/view/etc. 

I use the [rails admin](https://github.com/railsadminteam/rails_admin) gem in my own applications.

<aside>
Notes on install:

Running `rails g rails_admin:install` will create a bunch of unnecessary files. You can discard everything except for `config/initializers/rails_admin.rb`. Set the rails admin asset source to sprockets `config.asset_source = :sprockets` in the `rails_admin.rb` file. Or, simply set this in the generator `rails g rails_admin:install --asset=sprockets`.

If you're using pundit, you'll need to add some actions to your ApplicationPolicy. (dashboard?, export?, history?, show_in_app?)
</aside>

You will also need a way to identify admin users. The simplest way is to simply add an `admin` boolean column to users. (Make sure it defaults to false)

```ruby
class AddAdminToUsers < ActiveRecord::Migration[7.1]
  def change
    add_column :users, :admin, :boolean, default: false, null: false
  end
end
```

Update your routes to make sure only authenticated admins can access it.

```ruby
# routes.rb
authenticate :user, ->(user) { user.admin? } do
  mount RailsAdmin::Engine, at: "admin", as: "rails_admin"
end
```
