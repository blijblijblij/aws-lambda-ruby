require 'rake/testtask'
require 'ant'

PROJECT_NAME = 'hello'.freeze
MAIN_SRC_DIR = 'lib'.freeze
JARS_DIR = 'jars'.freeze
BUILD_DIR = 'build'.freeze
DIST_DIR = "#{BUILD_DIR}/dist".freeze
COMPILE_DIR = "#{BUILD_DIR}/compile".freeze
CLASSES_DIR = "#{COMPILE_DIR}/classes".freeze

task default: %i[test clean package]

Rake::TestTask.new(:test) do |t|
  t.pattern = 'test/test_*.rb'
end

task :clean do
  ant.delete dir: BUILD_DIR
  puts
end

task :setup do
  ant.path id: 'classpath' do
    fileset dir: COMPILE_DIR
    fileset dir: JARS_DIR
  end
end

task package: :setup do
  make_jar MAIN_SRC_DIR, "#{PROJECT_NAME}.jar"
end

def make_jar(source_folder, jar_file_name)
  ant.mkdir dir: CLASSES_DIR
  `jrubyc ./lib/hello.rb -t #{CLASSES_DIR} --javac`
  ant.javac srcdir: source_folder,
            destdir: CLASSES_DIR,
            classpathref: 'classpath',
            source: '0.1',
            debug: 'yes',
            includeantruntime: 'no'
  ant.jar jarfile: "#{COMPILE_DIR}/#{jar_file_name}", basedir: CLASSES_DIR do
    zipgroupfileset dir: JARS_DIR, includes: 'jruby-complete-9.1.8.0.jar'
  end
  puts
end
