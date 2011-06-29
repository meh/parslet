Introduction
------------
Parslet makes developing complex parsers easy. It does so by

* providing the best **error reporting** possible
* **not generating** reams of code for you to debug

Parslet takes the long way around to make **your job** easier. It allows
for incremental language construction. Often, you start out small,
implementing the atoms of your language first; _parslet_ takes pride in making
this possible.

Eager to try this out? Please see the associated web site:
http://kschiess.github.com/parslet

Synopsis
--------

```ruby
require 'parslet'
include Parslet

# Constructs a parser using a Parser Expression Grammar like DSL: 
parser =  str('"') >> 
          (
            str('\\') >> any |
            str('"').absnt? >> any
          ).repeat.as(:string) >> 
          str('"')

# Parse the string and capture parts of the interpretation (:string above)        
tree = parser.parse('"This is a \\"String\\" in which you can escape stuff"')

tree # => {:string=>"This is a \\\"String\\\" in which you can escape stuff"}

# Here's how you can grab results from that tree:

transform = Parslet::Transform.new do
  rule(:string => simple(:x)) { 
    puts "String contents: #{x}" }
end
transform.apply(tree)
```

Compatibility
-------------
This library should work with most rubies. I've tested it with MRI 1.8 
(except 1.8.6), 1.9, rbx-head, jruby. Please report as a bug if you encounter 
issues.

Note that due to Ruby 1.8 internals, Unicode parsing is not supported on that 
version. 

Status 
------
At version 1.2.1 - See HISTORY.txt for changes.

(c) 2010 Kaspar Schiess
