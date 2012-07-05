HTML_OUT  = 'index.html'
HAML_IN   = 'index.haml'

task :default => :make

desc 'Build the presentation from source'
task :make => HTML_OUT

file HTML_OUT => HAML_IN do
  sh "bundle exec haml #{HAML_IN} > #{HTML_OUT}"
end
