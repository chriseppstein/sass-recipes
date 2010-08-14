task :default do
  sh "compass compile"
  FileList.new('**/*.haml').each do |file|
    puts "Compiling #{file} to html"
    sh "haml #{file} #{file.sub(/haml/,'html')}"
  end
end

task :pages do
  require 'git'
  require 'fileutils'
  repo = Git.open('.')
  FileUtils.rm_rf "tmp"
  FileUtils.mkdir("tmp")
  (FileList.new('**/*.html')+FileList.new('**/*.css')).each do |file|
    FileUtils.mkdir_p(File.dirname("tmp/#{file}"))
    FileUtils.cp(file, "tmp/#{file}")
  end
  repo.branch("gh-pages").checkout
  FileUtils.rm_rf "recipes/*"
  (FileList.new('tmp/**/*.html')+FileList.new('tmp/**/*.css')).each do |file|
    FileUtils.mkdir_p(File.dirname("recipes/#{file[4..-1]}"))
    FileUtils.cp(file, "recipes/#{file[4..-1]}")
  end
  FileUtils.rm_rf("tmp")
  Dir["recipes/**/*"].each {|f| repo.add(f) }
  repo.status.deleted.each {|f, s| repo.remove(f)}
  message = ENV["MESSAGE"] || "Updated at #{Time.now.utc}"
  repo.commit(message)
end
