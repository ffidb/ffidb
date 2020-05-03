# This is free and unencumbered software released into the public domain.

require 'ffidb'
require 'rake'

task default: %w(readme)

task :readme do
  $cols = [13, 7, 15, 32]; #p 2 + ($cols.size-1)*3 + $cols.sum + 2
  def print_row(*vals)
    print '| ', vals.each_with_index.map { |s, i| s.send((i!=1) ? :ljust : :rjust, $cols[i]) }.join(' | '), " |\n"
  end
  def print_div
    print '| ', $cols.each_with_index.map { |n, i| ':'.send((i!=1) ? :ljust : :rjust, n, '-') }.join(' | '), " |\n"
  end

  print_row "Library", "Release", "`dlopen()`", "`#include`"
  print_div
  FFIDB::Registry.open(__dir__) do |registry|
    registry.each_library do |library|
      next if Dir["#{library.name}/stable/*.yaml"].empty?
      stable_version = File.readlink("#{library.name}/stable")
      print_row *[library.name,
        stable_version,
        "`#{library.dlopen.first}`",
        library.headers.map { |s| "`#{s}`" }.join(', ')
      ]
    end
  end
end
