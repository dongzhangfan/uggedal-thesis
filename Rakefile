module RakedLaTeX

  # Provides configuration options that can be utilized by changing many
  # of the class' instance variables. This can easily be done with a block:
  #
  #   RakedLaTeX::Configuration.new do |t|
  #     t.klass = { :book => %w(12pt a4paper twoside) }
  #
  #     t.packages << { :fontenc => ['T1'] }
  #     t.packages << { :natbib => [] }
  #
  #     t.title = 'Dev Null and Nothingness'
  #     t.author = { :name => 'Eivind Uggedal', :email => 'eu@redflavor.com' }
  #
  #     t.table_of_contents = true
  #
  #     t.main_content = %w(introduction previous.research method data)
  #
  #     t.appendices = %w(data.tables concent.forms)
  #   end
  #
  class Configuration

    # Class of the document and it's optional options. Defaults to the
    # article class with no options enabled. Example:
    #
    #   { :book => ['12pt', 'a4paper', 'twoside']
    #
    attr_accessor :klass

    # List of packages to use and their optional options. Example:
    #
    #   [ { :appendix => [:titletoc, :page] },
    #     { :fontenc => ['T1'] },
    #     { :longtable => [] } ]
    #
    attr_accessor :packages

    # Title of the document. No front title will be generated if this value
    # is empty. Examples:
    #
    #   'Types of Social Navigation in Modern Web Applications'
    #
    attr_accessor :title

    # The author of the document. Example:
    #
    #   { :name => 'Eivind Uggedal',
    #     :email => 'eu@redflavor.com' }
    #
    attr_accessor :author

    # SCM information like name, revision of the tip/head/trunk and it's
    # date. Example:
    #
    #   { :name => 'Mercurial',
    #     :revision => 45,
    #     :date => '12th of August 2005' }
    #
    attr_accessor :scm

    # Often it's neccessary to include extra configuration, custom commands,
    # etc in the preamble of the document. This hook enables such behaviour.
    # Example:
    #
    #   '\include{my.comstom.commands}'
    #
    #   or
    #
    #   %q(\sloppy
    #      \raggedbottom)
    #
    attr_accessor :preamble_extras

    # The documents abstract. Example:
    #
    #   %q(This document tries to answer some random research question.
    #      This research is important and wort studying because of this and
    #      that. The data used to illuminate this problem is /dev/null and
    #      the method of procrastination is to analyse this naturally
    #      occuring data. The main findings are equal to nil and their
    #      implications in the light of other research is not known.)
    #
    attr_accessor :abstract

    # The documents acknowledgments. Example:
    #
    #   %q(I would like to thank all those who refused to belive in me and
    #      my work for all those years.)
    #
    attr_accessor :acknowledgments

    # Wether to include a table of contents in the document. Defaults to
    # false.
    attr_accessor :table_of_contents

    # Wether to include a list of figures in the document. Defaults to
    # false.
    attr_accessor :list_of_figures

    # Wether to include a list of tables in the document. Defaults to
    # false.
    attr_accessor :list_of_tables

    # List of latex files that will be included in the main matter of the
    # document. Usually this is used to include one file per chapter of the
    # document. The file suffix shuld be ommitted as '.tex' is always
    # appended to these files.
    # Example:
    #
    #   %w(introduction littrature methodology data conclusion)
    #
    attr_accessor :main_content

    # List of latex files that will be included as appendices of the
    # document. The file suffix shuld be ommitted as '.tex' is always
    # appended to these files.
    # Example:
    #
    #   %w(content.inventory content.mapping history)
    #
    attr_accessor :appendices

    # Latex file which will be used as the bibliography and the style to use
    # for it's listings. The file suffix should be omitted as '*.bib' is
    # always appended to this file. Example:
    #
    #   { :citations => :newapa }
    #
    attr_accessor :bibliography

    # Returns a list of all latex and bibtex source files(main_content,
    # appendices, and bibliography) with file extension (.tex and .bib).
    def collect_source_files
      (@main_content + @appendices).collect { |a| "#{a}.tex" } +
        @bibliography.keys.collect { |a| "#{a}.bib" }
    end

    # Directory for outputting the latex file generated from the template and
    # where all it's dependencies should be placed. Defaults to the same
    # directory as this file is placed in. Note that this is not the same as
    # the current working directory (pwd).
    attr_accessor :source_directory

    # Directory for building source files into binary files.
    # Defaults to the same directory as this file is placed in. Note that this
    # is not the same as the current working directory (pwd).
    attr_accessor :build_directory

    # File name for the base latex file. Defaults to 'base.tex'.
    attr_accessor :base_latex_file

    # Returns the bibtex counterpart to the base latex file.
    def base_bibtex_file
      @base_latex_file.gsub(/\.tex$/, '.aux') if @base_latex_file
    end

    def initialize
      @klass = { :article => [] }
      @packages = []
      @title = nil
      @author = nil
      @scm = {}
      @preamble_extras = nil
      @abstract = nil
      @acknowledgments = nil
      @table_of_contents = false
      @list_of_figures = false
      @list_of_tables = false
      @main_content = []
      @appendices = []
      @bibliography = {}

      @source_directory = File.dirname(__FILE__)
      @build_directory = File.dirname(__FILE__)
      @base_latex_file = 'base.tex'

      yield self if block_given?
    end

    def values
      binding
    end

    def base_path
      File.join(@source_directory, @base_latex_file)
    end
  end

  # Handles command line output. Could be useful in the future with regards to
  # verbosity settings and color support.
  module Output
    def notice(message)
      puts message
    end

    def warning(message)
      puts "  - #{message}"
    end

    def error(message)
      puts "  * #{message}"
    end
  end

  # Provides a template for a base latex file. This template can be configured
  # quite extencively by changing many of the class' instance variables. This
  # can easily be done with a block:
  #
  #   RakedLaTeX::Template.new('Dev Null and Nothingness') do |t|
  #     t.klass = { :book => %w(12pt a4paper twoside) }
  #
  #     t.packages << { :fontenc => ['T1'] }
  #     t.packages << { :natbib => [] }
  #
  #     t.author = { :name => 'Eivind Uggedal', :email => 'eu@redflavor.com' }
  #
  #     t.table_of_contents = true
  #
  #     t.main_content = %w(introduction previous.research method data)
  #
  #     t.appendices = %w(data.tables concent.forms)
  #   end
  #
  class BaseTemplate
    require 'erb'
    include RakedLaTeX::Output

    def create_file(config)
      File.open(config.base_path, 'w') do |f|
        f.puts generate(config.values)
      end
      notice "Creation completed for: #{config.base_latex_file} in " +
             "#{config.source_directory}"
    end

    def generate(values)
      ERB.new(template, 0, '%<>').result(values)
    end

    def template
      %q(
        % @klass.each do |klass, options|
        \documentclass[<%=options.join(',')%>]{<%=klass%>}
        % end

        % @packages.each do |package|
        %   package.each do |name, options|
        \usepackage[<%=options.join(',')%>]{<%=name%>}
        %   end
        % end

        % if @title
        \title{<%=@title%>}
        %   if @author && @author[:name]
        \author{%
          <%=@author[:name]%>
        %     if @author[:email]
          \\\\
          \texttt{<%=@author[:email]%>}
        %     end
        %     if @scm
        %       if @scm[:name]
          \\\\ \\\\ \\\\
          <%=@scm[:name]%> Stats
        %       end
        %       if @scm[:revision]
          \\\\
          \texttt{<%=@scm[:revision]%>}
        %       end
        %       if @scm[:date]
          \\\\
          \texttt{<%=@scm[:date]%>}
        %       end
        %     end
        }
        %   end
        % end

        % if @preamble_extras
        <%=@preamble_extras%>
        % end

        % if @abstract
        \documentabstract{%
          <%=@abstract%>
        }
        % end

        % if @acknowledgments
        \acknowledgments{%
          <%=@acknowledgments%>
        }
        % end

        \begin{document}
          \frontmatter
        % if @title
            \maketitle
        % end
        % if @table_of_contents
            \tableofcontents
        % end
        % if @list_of_figures
            \listoffigures
        % end
        % if @list_of_tables
            \listoftables
        % end

          \mainmatter
        % @main_content.each do |content|
            \include{<%=content%>}
        % end

        % if @appendices && @appendices.any?
            \begin{appendices}
        %   @appendices.each do |appendix|
              \include{<%=appendix%>}
        %   end
            \end{appendices}
        % end

          \backmatter
        % @bibliography.each do |file, style|
            \bibliographystyle{<%=style%>}
            \bibliography{file}
        % end
        \end{document}
      ).gsub(/^[ ]{8}/, '')
    end
  end

  # Extracts changeset stats from various SCM systems. This info can be
  # included in the title page of the latex document and is especially helpful
  # when working with draft versions.
  module ScmStats
    class Base

      # The name of the SCM system. 
      attr_accessor :name

      # The Mercurial executable.
      attr_accessor :executable

      # The revision and date of the tip/head/latest changeset.
      attr_accessor :revision, :date

      def collect_scm_stats
        { :name => @name,
          :revision => @revision,
          :date => @date }
      end
    end

    class Mercurial < Base

      def initialize
        super

        @name = 'Mercurial'
        @executable = 'hg' if system 'which hg > /dev/null'

        @revision, @date = parse_scm_stats

        yield self if block_given?
      end

      def parse_scm_stats
        return [nil, nil] unless @executable

        raw_stats = `hg tip`
        revision = raw_stats.scan(/^changeset: +(.+)/).first.first
        date = raw_stats.scan(/^date: +(.+)/).first.first

        [revision, date]
      end
    end

    class Subversion < Base

      def initialize
        super

        @name = 'Subversion'
        @executable = 'svn' if system 'which svn > /dev/null'

        @revision, @date = parse_scm_stats

        yield self if block_given?
      end

      def parse_scm_stats
        return [nil, nil] unless @executable

        raw_stats = `svn info`
        revision = raw_stats.scan(/^Revision: (\d+)/).first.first
        date = raw_stats.scan(/^Last Changed Date: (.+)/).first.first

        [revision, date]
      end
    end
  end

  # Takes care of running latex and related utilities. Gives helpful
  # information if input files are missing and also cleans up the output of
  # these utilities.
  module Runner
    class Base
      include RakedLaTeX::Output

      # The file to be run trough the latex process.
      attr_accessor :input_file

      # If true no messages is sendt to standard out. Defaults to false.
      attr_accessor :silent

      # The LaTeX executable. Defaults to plain `latex` if it's found on
      # the system.
      attr_accessor :executable

      # Contains a list of possible warnings after a run.
      attr_accessor :warnings

      def initialize(input_file, silent, executable)
        @input_file = input_file
        @executable = executable if system "which #{executable} > /dev/null"
        @silent = silent
        @warnings = []

        if File.exists? @input_file
          run
        else
          error "Running of #@executable aborted. " +
                "Input file: #@input_file not found"
        end
      end

      def run
      end

      def feedback
        unless @warnings.empty? || @silent
          notice "Warnings from #@executable:"
          @warnings.each do |message|
            warning message
          end
        end
      end
    end

    class LaTeX < Base
      def initialize(input_file, silent=false, executable='latex')
        super
      end

      def run
        # TODO: Currently this does not work when latex awaits user input:
        @warnings = `latex #@input_file`.grep(/^(Overfull|Underfull|No file)/)
        feedback
      end
    end

    class BibTeX < Base
      def initialize(input_file, silent=false, executable='bibtex')
        super
      end

      def run
        @warnings = `bibtex #@input_file`.grep(/^I (found no|couldn't open)/)
        feedback
      end
    end
  end

  # Handles the business of building latex (and bibtex if needed) source
  # files into binary formats as dvi, ps, and pdf. The latex and bibtex
  # utilites need to be run a certain number of times so that things like
  # table of contents, references, citations, etc become proper. This module
  # tries to solve this issue by running the needed utilities only as many
  # times as needed.
  module Builder
    class Base
      include RakedLaTeX::Output

      # The directory where the files to be built should be located.
      attr_accessor :source_directory

      # The directory where the build should be run. This directory is
      # created if it's not present.
      attr_accessor :build_directory

      # List of source files that are part of the build. If such a list is
      # present all files are verified of existence before the process
      # proceeds.
      attr_accessor :source_files


      def initialize(source_directory, build_directory, source_files=[])
        @source_directory = source_directory
        @build_directory = build_directory
        @source_files = source_files

        @build_name = 'base'
      end

      def build_directory
        if @build_directory
          mkdir_p @build_directory unless File.exists? @build_directory
          cd @build_directory do
            copy_source_files
            return false unless source_files_present?
            yield
          end
        else
          return false unless source_files_present?
          yield
        end
        true
      end

      def copy_source_files
        %w(tex bib sty).each do |file_extension|
          FileList["#{@source_directory}/*.#{file_extension}"].each do |file|
            cp(file,  @build_directory)
          end
        end
      end

      def source_files_present?
        @source_files.each do |file|
          unless File.exists? file
            error "Build of #@build_name aborted. " +
                  "Source file: #{file} not found"
            return false
          end
        end
        true
      end
    end

    class Dvi < Base
      def initialize(*args)
        super
        @build_name = 'dvi'
      end

      def build(base_latex_file)
        success = build_directory do
          run = RakedLaTeX::Runner::LaTeX.new(base_latex_file, true)
          if run.warnings.join =~ /No file .+\.(aux|toc)/
            run = RakedLaTeX::Runner::LaTeX.new(base_latex_file, true)
          end
          run.silent = false
          run.feedback
        end
        notice "Build of #{@build_name} completed for: #{base_latex_file} " +
               "in #{@build_directory}" if success
      end
    end
  end
end

task :default => :generate_base

task :generate_base do
  puts RakedLaTeX::BaseTemplate.new.generate(CONFIG.values)
end

task :create_base do
  RakedLaTeX::BaseTemplate.new.create_file(CONFIG)
end

task :run_latex do
  RakedLaTeX::Runner::LaTeX.new(CONFIG.base_path)
end

task :run_bibtex do
  RakedLaTeX::Runner::BibTeX.new(CONFIG.base_bibtex_file)
end

task :build_dvi do
  RakedLaTeX::Builder::Dvi.new(CONFIG.source_directory,
     CONFIG.build_directory,
     CONFIG.collect_source_files).build(CONFIG.base_latex_file)
end

CONFIG = RakedLaTeX::Configuration.new do |t|
  t.klass = { :book => %w(11pt a4paper twoside) }

  t.packages << { :hyperref => %w(ps2pdf
                                  bookmarks=true
                                  breaklinks=false
                                  raiselinks=true
                                  pdfborder={0 0 0}
                                  colorlinks=false) }
  t.packages << { :fontenc => ['T1'] }
  t.packages << { :mathpazo => [] }
  t.packages << { :courier => [] }
  t.packages << { :helvet => [] }
  t.packages << { :appendix => %w(titletoc page) }
  t.packages << { :longtable => [] }
  t.packages << { :booktabs => [] }
  t.packages << { :natbib => [] }

  t.title = 'Draft: Social Navigation'
  t.author = { :name => 'Eivind Uggedal', :email => 'eivindu@ifi.uio.no' }

  t.scm = RakedLaTeX::ScmStats::Mercurial.new.collect_scm_stats

  t.preamble_extras = '\include{commands}'

  t.table_of_contents = true

  t.main_content = %w(content.analysis)

  t.appendices = %w(content.inventory
                    content.mapping)

  t.bibliography = { :bibliography => :kluwer }

  t.source_directory += '/src'
  t.build_directory += '/build'
end