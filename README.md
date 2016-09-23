# TumbleDRYer
#### Magic Night: 9/22/2016


Ruby quiz by Hugh Sasse

The Pragmatic Programmers (and others) recommend that you remove repetition from
code. They call this principle DRY: Don't Repeat Yourself. Benefits include not
having to track down multiple places to make a change affecting the whole
system, prevention of inconsistency, as well as reduced debugging time.
Sometimes code ends up being repetitive. Take this MySQL example from:

```sql
CREATE TABLE `authors` (
  `id` int(11) NOT NULL auto_increment,
  `firstname` varchar(50) NOT NULL default '',
  `name` varchar(50) NOT NULL default '',
  `nickname` varchar(50) NOT NULL default '',
  `contact` varchar(50) NOT NULL default '',
  `password` varchar(50) NOT NULL default '',
  `description` text NOT NULL,
  PRIMARY KEY  (`id`)
) TYPE=MyISAM AUTO_INCREMENT=3 ;

CREATE TABLE `categories` (
  `id` int(11) NOT NULL auto_increment,
  `name` varchar(20) NOT NULL default '',
  `description` varchar(70) NOT NULL default '',
  PRIMARY KEY  (`id`)
) TYPE=MyISAM AUTO_INCREMENT=3 ;

CREATE TABLE `categories_documents` (
  `category_id` int(11) NOT NULL default '0',
  `document_id` int(11) NOT NULL default '0',
) TYPE=MyISAM ;

CREATE TABLE `documents` (
  `id` int(11) NOT NULL auto_increment,
  `title` varchar(50) NOT NULL default '',
  `description` text NOT NULL,
  `author_id` int(11) NOT NULL default '0',
  `date` date NOT NULL default '0000-00-00',
  `filename` varchar(50) NOT NULL default '',
  PRIMARY KEY  (`id`),
  KEY `document` (`title`)
) TYPE=MyISAM AUTO_INCREMENT=14 ;
```

clearly there's a lot of repetition in there. It would be nice if it were
possible to eliminate it. This quiz is about writing a program which we'll call
TumbleDRYer, which, given repetitive input, will create ruby code to generate
that input, such that the generating program has much less repetition in it. One
way might be to generate a program like this:

```ruby
class WashingMachine
  def initialize
    @create = 'CREATE TABLE'
    @type = 'TYPE=MyISAM'
    @varchar_50 = "varchar(50) NOT NULL default '',"
    @varchar_20 = "varchar(20) NOT NULL default '',"
    @int_11 = "int(11) NOT NULL auto_increment,"
    @int_11_0 = "int(11) NOT NULL default '0',"
    @pk = "PRIMARY KEY  (`id`)"
    @auto = "AUTO_INCREMENT="
  end

  def output
    print "
   #{@create} `authors` (
     `id` #{@int_11}
     `firstname` #{@varchar_50}
     `name` #{@varchar_50}
     `nickname` #{@varchar_50}
     `contact` #{@varchar_50}
     `password` #{@varchar_50}
     `description` text NOT NULL,
     #{@pk}
   ) #{@type} #{@auto}3 ;

   #{@create} `categories` (
     `id` #{@int_11}
     `name` #{@varchar_20}
     `description` varchar(70) NOT NULL default '',
     #{@pk}
   ) #{@type} #{@auto}3 ;

   #{@create} `categories_documents` (
     `category_id` #{@int_11_0}
     `document_id` #{@int_11_0}
   ) #{@type} ;

   #{@create} `documents` (
     `id` #{@int_11}
     `title` #{@varchar_50}
     `description` text NOT NULL,
     `author_id` #{@int_11_0}
     `date` date NOT NULL default '0000-00-00',
     `filename` #{@varchar_50}
     #{@pk},
     KEY `document` (`title`)
   ) #{@type} #{@auto}14 ;
"
  end
end

WashingMachine.new.output
```

Obviously the more human readable to the resulting program is the better: the
point is to produce something that simplifies maintenance, rather than data
compression. Techniques from data compression would seem to be useful, however.
Building up a dictionary of character groups and their frequencies might be
useful, and various strategies are possible for how to fragment the string into
repeated chunks, such as: the longest repeated substring; splitting on some kind
of boundary; or techniques from Ziv-Lempel coding to create the groups.
