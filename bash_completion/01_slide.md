!SLIDE

# Programmable Completion #
## AKA Autocompletion ##

!SLIDE center
# Completion Variables #

COMP_LINE

COMP_WORDS

COMP_CWORD

COMP_POINT

!SLIDE commandline incremental center
# Complete command #

    $ complete -F _function function

    $ complete -C command function

!SLIDE smaller
# A simple example #

    @@@ sh
    xingdir="/Users/jorge.dias/development/xing"

    function cx() {
      dir="$xingdir"
      if [ -n $1 ]; then
        dir="$dir/$1"
      fi
      cd $dir
    }

    function _cx() {
      local cur

      cur=${COMP_WORDS[COMP_CWORD]}
      subdirectories="$( cd $xingdir && ls -d */ | sed 's/\///' )"
      COMPREPLY=( $( compgen -W "$subdirectories" -- "$cur" ) )
    }

    complete -F _cx cx

!SLIDE smaller
# A ruby example

    @@@ sh
    complete -C autocompletion -o default command

    @@@ ruby
    #!/usr/bin/env ruby
    unless ENV['COMP_LINE'].nil?
      puts Completion.new(ENV["COMP_LINE"]).matches
      exit 0
    end

    @@@ ruby
    class Completion
      def initialize(comp_line)
        @comp_words = comp_line.split(" ")
      end

      def matches
        if @comp_words.size == 1
          ["one", "two"]
        elsif @comp_words.size == 2
          ["one", "two"].select { |x| x.include? @comp_words[1] }
        end
      end
    end
