require 'fileutils'
require 'pathname'

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

desc '"Publish" the presentation to the gh-pages branch'
task :publish => OUT_FILES do
  `git diff --quiet`
  raise 'dirty worktree' if $?.exitstatus != 0

  base  = Pathname.new(File.join(__FILE__, '..')).realpath
  build  = base + 'build'
  FileUtils.rm_rf build
  FileUtils.mkdir_p build
  FileUtils.cp_r(base + 'assets', build)
  FileUtils.cp_r(base + 'reveal', build)
  FileUtils.cp_r(base + 'highlight', build)
  FileUtils.cp(base + 'causes.css', build)
  FileUtils.cp(base + HTML_OUT, build)
  FileUtils.rm_rf(base + 'highlight')
  FileUtils.rm_rf(base + 'reveal')

  `git checkout gh-pages`

  FileUtils.rm_rf(base + 'assets')
  FileUtils.mv(Dir.glob(build + '*'), base, :force => true)
  FileUtils.rm_r(build)

  `git add --all assets reveal highlight causes.css index.html`
  `git commit -m "Built presentation #{Time.now}"`
  `git checkout master`
  `git submodule update`
end
