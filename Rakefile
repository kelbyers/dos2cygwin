#

require 'open3'

def dos_file path
  ft = Open3.popen3('file', path)[1].read
  return ft.match(/CRLF/)
end

desc "Make vagrant script a unix file, so it works with bash"
vagrant_bin = '/cygdrive/c/vagrant/vagrant/bin/vagrant'
task :vagrant do
  if dos_file(vagrant_bin)
    system('dos2unix', vagrant_bin)
  end
end

desc "Make vagrant work with rvm installed"
task :vagrant_rvm => [:vagrant] do
  if system("grep", '^${EMBEDDED_DIR}/bin/ruby', vagrant_bin)
    system('sed', '-i', 's/^\(\${EMBEDDED_DIR}\/bin\/ruby\)/GEM_HOME= GEM_PATH= \1/', vagrant_bin)
  end
end

desc "allow vagrant ssh to use cygwin's ssh command"
task :ssh => '/cygdrive/c/vagrant/vagrant/embedded/lib/ruby/gems/1.9.1/gems/vagrant-1.0.3/lib/vagrant/ssh.rb'
file '/cygdrive/c/vagrant/vagrant/embedded/lib/ruby/gems/1.9.1/gems/vagrant-1.0.3/lib/vagrant/ssh.rb' => ['vagrant/ssh.rb'] do |t|
  cp t.prerequisites[0], t.name
end

desc "setup up vagrant from standard install to work with cygwin"
task :vagrant_all => [:vagrant, :ssh, :vagrant_rvm]

desc "default => [vagrant_all]"
task :default => [:vagrant_all]
