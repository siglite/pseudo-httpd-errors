# -*- coding: utf-8 -*-

task :default => :make_config

def split path
  status = path[/\d{3}/]              # "src/400.html" -> "400"
  redirect_to = path[/\/\d{3}\.html/] # "src/400.html" -> "/400.html"
  return status, redirect_to
end

def minify path
  html = File.open(path, "r").read
  # " -> \", LF -> '\n'
  html.gsub(/"/, "\\\"").gsub(/\n/, "\\n")
end

task :make_config do
  conf  = File.open("Apache-errors.conf", "w")
  files = Dir.glob("src/*.html").sort

  files.each do |f|
    status, redirect_to = split f
    conf.write "error_page #{status} = #{redirect_to};\n"
  end

  conf.write "\n"

  files.each do |f|
    status, redirect_to = split f
    html = minify f

    conf.write "location = #{redirect_to} {\n"
    conf.write "\tinternal;\n"
    conf.write "\treturn #{status} \"#{html}\";\n"
    conf.write "}\n"
  end
end
