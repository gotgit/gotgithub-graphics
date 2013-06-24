#!/usr/bin/ruby
# -*- coding: UTF-8 -*-

require 'fileutils'
require 'rubygems'

TARGET = "../images"
RESIZE = 600

if File.exist?('/Applications/Inkscape.app/Contents/Resources/bin/inkscape')
  CMD_INKSCAPE='/Applications/Inkscape.app/Contents/Resources/bin/inkscape'
else
  CMD_INKSCAPE='inkscape'
end
CMD_IMGMAGICK='convert'

desc "export images, default: rake convert[../images,600]"
task :convert, :target, :width do |t, args|
  args.with_defaults(:target => TARGET, :width => RESIZE)
  FileList["**/*.svg", "**/*.xcf", "**/*.jpg", "**/*.png", "**/*.gif"].each do |source|
    [".svg", ".png", ".gif", ".xcf"].each do |prefer|
      if File.exists?(source.sub(/\.[^.]+$/, prefer))
        source = source.sub(/\.[^.]+$/, prefer)
        break
      end
    end
    if source =~ /\.svg/
      output = args[:target] + '/' + source.sub(/\.[^.]+$/, '.png')
      command = "#{CMD_INKSCAPE} -w #{args[:width]} -f #{source} -e #{output}"
    elsif source =~ /\.xcf/
      output = args[:target] + '/' + source.sub(/\.[^.]+$/, '.png')
      command = "#{CMD_IMGMAGICK} -auto-level -flatten -resize '#{args[:width]}>' #{source} #{output}"
    elsif source =~ /\.(jpg|png|gif)/
      output = args[:target] + '/' + source
      command = "#{CMD_IMGMAGICK} -auto-level -resize '#{args[:width]}>' #{source} #{output}"
    end

    unless uptodate?(output, [source])
      Dir.mkdir(File.dirname(output)) unless File.exists?(File.dirname(output))
      sh command
      File.utime(0, Time.now, output)
    end
  end
end

task :default do
  system "rake -T"
end
