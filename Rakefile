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
  FileList["**/*.svg", "**/*.xcf", "**/*.jpg", "**/*.png", "**/*.gif"].each do |t|
    [".svg", ".png", ".gif", ".xcf"].each do |prefer|
      if File.exists?(t.sub(/\.[^.]+$/, prefer))
        t = t.sub(/\.[^.]+$/, prefer)
        break
      end
    end
    if t =~ /\.svg/
      f = args[:target] + '/' + t.sub(/\.[^.]+$/, '.png')
      command = "#{CMD_INKSCAPE} -w #{args[:width]} -f #{t} -e #{f}"
    elsif t =~ /\.xcf/
      f = args[:target] + '/' + t.sub(/\.[^.]+$/, '.png')
      command = "#{CMD_IMGMAGICK} -auto-level -flatten -resize '#{args[:width]}>' #{t} #{f}"
    elsif t =~ /\.(jpg|png|gif)/
      f = args[:target] + '/' + t
      command = "#{CMD_IMGMAGICK} -auto-level -resize '#{args[:width]}>' #{t} #{f}"
    end

    unless uptodate?(f, t)
      Dir.mkdir(File.dirname(f)) unless File.exists?(File.dirname(f))
      sh command
      File.utime(0, Time.now, f)
    end
  end
end

task :default do
  system "rake -T"
end
