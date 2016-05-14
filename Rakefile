require 'date'

desc "Run a dev server at localhost:4000"
task :dev do
  sh "bundle exec jekyll serve --config _config.yml,_config_dev.yml"
end


desc 'Build the site'
task :build do
  sh 'bundle exec jekyll build'
end

desc 'Publish to staging via rsync'
task :deploy do
  puts 'Publishing the contents of _site'
  user = 'git'
  server = 'staging.lucyparsonslabs.com'
  path = '/var/www/lucyparsonslabs.com'
  sh "rsync -rtzh --delete _site/ #{user}@#{server}:#{path}"
  puts 'New content copied to http://staging.lucyparsonslabs.com'
end

desc 'Publish to prod via rsync'
task :deploy_prod do
  puts 'Publishing the contents of _site'
  user = 'git'
  server = 'lucyparsonslabs.com'
  path = '/var/www/lucyparsonslabs.com'
  sh "rsync -rtzh --delete _site/ #{user}@#{server}:#{path}"
  puts 'New content copied to http://lucyparsonslabs.com'
end


desc "Update the warrant canary with your gpg key"
task :updatecanary do
# note: if you include any shell-special characters here, you get to fix the sh command
 canarytext = <<-END
We have not been contacted by any government agencies requesting\n
information about our workshop attendees or website visitors.\n
This post will be updated monthly.\n
Last updated #{Date.today.strftime("%B %e, %Y")}.
END
 sh "echo \"#{canarytext}\" | gpg --clearsign > _includes/canary.txt"
end
