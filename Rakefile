HTML_OUT  = 'index.html'
HAML_IN   = 'index.haml'

HIGHLIGHT_OUT = 'highlight/build/highlight.pack.js'
HIGHLIGHT_IN  = 'highlight/src/highlight.js'

OUT_FILES = [HTML_OUT, HIGHLIGHT_OUT]

task :default => :make

desc 'Build the presentation from source'
task :make => OUT_FILES

file HTML_OUT => HAML_IN do
  sh "bundle exec haml #{HAML_IN} > #{HTML_OUT}"
end

file HIGHLIGHT_OUT => HIGHLIGHT_IN do
  Dir.chdir 'highlight' do
    sh 'python tools/build.py'
  end
end
