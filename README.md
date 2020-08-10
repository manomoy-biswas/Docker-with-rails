# Docker-with-rails-sidekiq-elasticsearch

# Create new app using docker:

    $ docker-compose run [service_name] rails new . --force --no-deps –database=[database_name]
   
   Let service name is web and database is MySQL:
    
    $ docker-compose run web rails new . --force --no-deps –database=mysql

# Change the ownership of file (Only for linux platform)

  Using Docker on Linux, rails new create files owned by root user. This happens because the container runs as the root user. If this is the case, change the ownership of the new files.
    
    $ sudo chown -R $USER:$USER .

#  Add sidekiq gem in gemfile

    Write in ./Gemfile
	...
	<gem “sidekiq”>
	...
# Change database.yml file
  Remove every thing and all the below line in ./config/database.yml
   
#   For PostgreSQL Database:
	 
	 default: &default
	     adapter: postgresql
	     encoding: unicode
	     pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
	     username: <%= ENV['DB_USERNAME'] %>
	     password: <%= ENV['DB_PASSWORD'] %>
	     host: <%= ENV['DB_HOST'] %>

#   For MySQL Database:
	 
	 default: &default
	     adapter: mysql2
	     encoding: utf8mb4
	     pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
	     username: <%= ENV['DB_USERNAME'] %>
	     password: <%= ENV['DB_PASSWORD'] %>
	     host: <%= ENV['DB_HOST'] %>

	development:
	    <<: *default
	    database: test_development

	test:
	    <<: *default
	    database: app_test

	production:
	    <<: *default
	    database: app_production
	    username: app
	    password: <%= ENV['APP_DATABASE_PASSWORD'] %>
	    
# Run build command again

  As we change the gemfile, We need to run build again
 
    $ docker-compose build

# Run Docker-compose up
   
    $ docker-compose up
   
# System is ready now
# Thank you
