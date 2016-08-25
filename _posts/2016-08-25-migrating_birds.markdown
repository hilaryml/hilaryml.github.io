---
layout: post
title:  "Migrating Birds"
date:   2016-08-25 22:53:07 +0000
---


As I’m getting ever closer to the end of the ActiveRecord unit, I decided that it would be great to do a quick review post on ActiveRecord and Migrations.

After working with birds of prey for several years, I decided that birds would make the perfect objects for a migration example (of course!).

ActiveRecord gives you access to ready-made methods such as create, save, where and find, saving you from having to code them yourself. This means that with just a few simple lines of code, you can create a database that interacts with all of your classes. Easy peasy!

Let’s say we have a Bird class that we wanted to migrate into a database. Our file will be named bird.rb and it will live in our folders folder, within the app folder.

```
#app/models/bird.rb

class Bird < ActiveRecord::Base
end
```

Next we need to create a migration file in which we will create our birds table. Our migration file will be named 20160825063334_create_birds.rb. This file will live in our migrations folder, within our db folder. The number at the front of the file is just a timestamp, it will help make sure that our migration files are migrated in the correct order (as long as you code things out sequentially!).

```
#db/migrations/20160825063334_create_birds.rb

class CreateBirds < ActiveRecord::Migration
	def change
		create_table :birds do |t|
			t.string :species
			t.string :range
			t.string :habitat
		end
	end
end
```

Above, you’ll notice that when we named our class, we not only pluralized ‘Birds’, we also added the word ‘Create’ and camel-cased it. This is the appropriate syntax for a migration file. A class in a migration file must always match the name of the file. Migrations, and thus our class, will inherit from ActiveRecord::Migration (as opposed to ActiveRecord::Base, like our Bird class did).

Now that our files are both ready, let’s migrate!!

```
#terminal

rake db:migrate
```

Great! Now that our Birds have safely migrated into our database, any further changes will need to be made by adding new migration files and then migrating those over to the database.

Well, that’s all for now! For those of you who are fellow bird enthusiasts, keep your eyes on the sky as the Sharp-shinned Hawks start to migrate southward out of Canada in early September.
