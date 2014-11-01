# Heroku Buildpack: SmartMigrate

Simple Heroku buildpack to *smartly* run `rake db:migrate` whenever new migrations detected.
This buildpack is intended to be part of a [Multipack](https://github.com/ddollar/heroku-buildpack-multi) that has a preceding [Ruby buildpack](https://github.com/heroku/heroku-buildpack-ruby).

## Motivation
Many times, I forget to run `rake db:migrate` after I push to heroku, which results in a broken app.
Some people out there will just create a script/rake task to automate this step after the push.
This is sub-optimal because it will put the app in maintenance mode or leave it broken for some time
until the migration is finished. Morever, the migrations will ALWAYS run which penalizes the app by longer down time.
Some others have forked the official Ruby buildpack to just add the migrations at the end.
That's also not so good, because they have to maintain their fork to get all updates from the original repo.
Otherwise, their fork will be out-dated in a short time.

SmartMigrate should be invoked after a Ruby buildpack in a Multipack app. It will automatically run migrations
*only* if new files have been added since the last deployment.
