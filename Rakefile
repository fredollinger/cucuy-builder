# Package building Rakefile
# Put packages in directory specified by TOBUILD and they will be created in BUILT
# directory. If something gets messed up, it will be in the in-progress dir 

require 'rake/clean'
require 'fileutils'

XORG_CONFIG=ENV['XORG_CONFIG']

TOBUILD="to-build" # where packages to build are placed 
INPROGRESS="in-progress" # working dir--if a package is here it probably failed to compile
BUILT="built"  # where finished package source code goes
SRC="src" # where we store tarballs of completed packages for posterity

directory INPROGRESS
directory BUILT

task :default => [:build]

def unpack(fn)
	fn = TOBUILD + "/" + fn
	sh "tar -xkjvf #{fn} -C #{INPROGRESS}"
end

def compile(fn)
	sh "cd #{fn} && ./configure #{XORG_CONFIG} && make && make install"
end

desc "Configure things inside #{INPROGRESS} dir"
task :compile do
	Dir.foreach(INPROGRESS) do |fn|
		next if fn == '.' or fn == '..'
		prog = INPROGRESS + "/" + fn
		compile(prog)
	end
end

desc "Move built packages to BUILT and tarballs back to SRC directories"
task :tidy do
	Dir.foreach(INPROGRESS) do |fn|
		next if fn == '.' or fn == '..'
		prog = INPROGRESS + "/" + fn
		tarball = TOBUILD + "/" + fn 
		sh "rm -rfv #{BUILT}/#{fn}"
		sh "mv #{prog} #{BUILT}"
		sh "mv #{tarball}* #{SRC}"
	end
end

desc "Build things inside of to-build including unpacking and so on"
task :build => [:unpack, :compile, :tidy] do
end

desc "Unpack everything"
task :unpack do
	Dir.foreach(TOBUILD) do |fn|
		next if fn == '.' or fn == '..'
		unpack(fn)
	end
end

desc "Build things inside of to-build"
task :clean do
	sh "cd in-progress && rm -r *"
end
